# Step 5 - Add GPT-5 text generation

After the image feature works, we add a second AI flow on the same page. To keep the two actions separate, we create a different page handler for text generation and a second submit button that calls it from the same form.

## 1) Add the secret configuration:

Use the **Text generation model name** value from Moodle and use it to replace the placeholder in the next command. Then, run the command:

```bash
dotnet user-secrets set "Foundry:TextModel" "your-text-model-name"
```

## 2) Add the GPT-5 model classes

We create the request and response classes for the question-and-answer flow.

```csharp
namespace AiDemo.Models;

public class TextGenerationRequest
{
    public string Prompt { get; set; } = string.Empty;
    public int MaxTokens { get; set; } = 300;
    public double Temperature { get; set; } = 0.7;
}
```

```csharp
namespace AiDemo.Models;

public class TextGenerationResponse
{
    public string? Answer { get; set; }
    public string? Model { get; set; }
    public string? ErrorMessage { get; set; }
}
```

These classes match the JSON structure the model returns and keep the request/response objects strongly typed.

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
        Prompt = TextPrompt,
        MaxTokens = 250,
        Temperature = 0.4
    };

    var json = JsonSerializer.Serialize(request);
    using var content = new StringContent(json, Encoding.UTF8, "application/json");

    var response = await client.PostAsync(
        $"{endpoint.TrimEnd('/')}/models/{model}/chat/completions?api-version=2024-05-01-preview",
        content);

    if (response.IsSuccessStatusCode)
    {
        var result = await response.Content.ReadFromJsonAsync<TextGenerationResponse>();
        TextAnswer = result?.Answer ?? "No answer was returned.";
    }
    else
    {
        TextAnswer = "The GPT-5 request failed.";
    }
}
```

This keeps the image logic separate from the text logic and makes it easier to reason about each request.

## 4) Add the second button in the same form

We keep one form and add a second button that uses a different `asp-page-handler` value:

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
