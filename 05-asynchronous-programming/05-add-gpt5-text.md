# Step 5 - Add GPT-5 text generation

After the image feature works, we add a second AI flow on the same page. To keep the two actions separate, we create a different page handler for text generation and a second submit button that calls it from the same form.

## 1) Add the secret configuration:

Use the **Text generation model name** value from Moodle and use it to replace the placeholder in the next command. Then, run the command:

```bash
dotnet user-secrets set "Foundry:TextModel" "your-text-model-name"
```

## 2) Add the GPT-5 model classes

For the Responses API, the request body uses the `model` and `input` fields. We also keep the response parsing simple and focused on the text returned by the model.

Add these two new classes in the `Models` folder:

**TextGenerationRequest.cs**:

```csharp
namespace AiDemo.Models;

using System.Text.Json.Serialization;

public class TextGenerationRequest
{
    [JsonPropertyName("model")]
    public string Model { get; set; } = string.Empty;

    [JsonPropertyName("input")]
    public string Input { get; set; } = string.Empty;

    [JsonPropertyName("max_output_tokens")]
    public int MaxOutputTokens { get; set; } = 300;
}
```

**TextGenerationResponse.cs**:

```csharp
namespace AiDemo.Models;

public class TextGenerationResponse
{
    public string? Answer { get; set; }
    public string? Model { get; set; }
    public string? ErrorMessage { get; set; }
}
```

## 3) Add the text generation handler and properties

We expand the page model with the new input and result properties and add `OnPostGenerateText`. Add the code inside the `IndexModel` class in `Pages/Index.cshtml.cs`.

Again, the form value must be bound explicitly with `[BindProperty]` so the submitted text reaches the handler correctly.

```csharp
[BindProperty]
public string TextPrompt { get; set; } = string.Empty;

public string TextAnswer { get; set; } = string.Empty;

public async Task OnPostGenerateText()
{
    var endpoint = _configuration["Foundry:Endpoint"];
    var apiKey = _configuration["Foundry:ApiKey"];
    var model = _configuration["Foundry:TextModel"];

    if (string.IsNullOrWhiteSpace(endpoint) || string.IsNullOrWhiteSpace(apiKey) || string.IsNullOrWhiteSpace(model))
    {
        TextAnswer = "Missing GPT-5 secret configuration.";
        return;
    }

    var client = _httpClientFactory.CreateClient();
    client.DefaultRequestHeaders.Add("api-key", apiKey);

    var request = new TextGenerationRequest
    {
        Model = model,
        Input = TextPrompt,
        MaxOutputTokens = 250
    };

    var requestJson = JsonSerializer.Serialize(request);
    using var content = new StringContent(requestJson, Encoding.UTF8, "application/json");

    var textEndpoint = endpoint.TrimEnd('/');
    if (!textEndpoint.EndsWith("/responses", StringComparison.OrdinalIgnoreCase))
    {
        textEndpoint = textEndpoint.Contains("/openai", StringComparison.OrdinalIgnoreCase)
            ? $"{textEndpoint}/responses"
            : $"{textEndpoint}/openai/v1/responses";
    }

    var response = await client.PostAsync($"{textEndpoint}?api-version=v1", content);

    if (response.IsSuccessStatusCode)
    {
        var body = await response.Content.ReadAsStringAsync();
        using JsonDocument responseJson = JsonDocument.Parse(body);

        string? answer = null;
        if (responseJson.RootElement.TryGetProperty("output", out var output))
        {
            foreach (var item in output.EnumerateArray())
            {
                if (!item.TryGetProperty("content", out var contentArray))
                {
                    continue;
                }

                foreach (var contentItem in contentArray.EnumerateArray())
                {
                    if (contentItem.TryGetProperty("type", out var type) &&
                        type.GetString() == "output_text" &&
                        contentItem.TryGetProperty("text", out var textElement))
                    {
                        answer = textElement.GetString();
                        break;
                    }
                }

                if (!string.IsNullOrWhiteSpace(answer))
                {
                    break;
                }
            }
        }

        TextAnswer = answer ?? "No answer was returned.";
    }
    else
    {
        var errorBody = await response.Content.ReadAsStringAsync();
        Console.WriteLine(errorBody);
        TextAnswer = "The GPT-5 request failed.";
    }
}
```

This keeps the image logic separate from the text logic and uses the correct endpoint for the GPT-5 Responses API.

## 4) Add the second button in the same form

Add a second button that uses a different `asp-page-handler` value along with the corresponding `textarea` HTML element so the user can enter a prompt that will be sent to the AI Model:

```cshtml
<form ...>

    <div class="mt-4">
        <label for="textPrompt" class="form-label">Ask for text</label>
        <textarea id="textPrompt"
                  name="TextPrompt"
                  class="form-control"
                  rows="4"
                  placeholder="Example: Write a short product description for a travel app"></textarea>
    </div>

    <div class="mt-3">
        <button type="submit" asp-page-handler="GenerateText" class="btn btn-success">
            Generate text
        </button>
    </div>
</form>
```

Add this code below the div element that contains the generated image:

```cshtml
<div class="mt-3">
    <label for="textAnswer" class="form-label">Answer</label>
    <textarea id="textAnswer"
              class="form-control"
              rows="5"
              readonly>@Model.TextAnswer</textarea>
</div>
```


The second button calls `OnPostGenerateText`, while the image button still calls `OnPostGenerateImage`. This keeps the page-handler pattern simple and avoids unnecessary duplicate form markup.

## 5) Test text generation

Build and run the app using the web browser. This time, enter a prompt and click the **Generate text** button. The text will be generated in the `textAnswer` text area element.

---

## Final result

At the end of this tutorial, the app can:

- generate an image from a prompt using a Foundry MAI-Image deployment
- generate a text answer using GPT-5 model
- keep all secrets in Secret Manager
- use classes with properties for the request and response objects
- use page handlers to separate the image flow from the text flow

---

[Previous: Step 4 - Send the image request to Foundry](04-call-image-model.md)  
[Next: Overview](README.md)
