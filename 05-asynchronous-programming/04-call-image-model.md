# Step 4 - Send the image request to Foundry

The page is ready, but the form still needs a real submit handler. In Razor Pages, we can use a page handler so the same page can manage different actions cleanly and clearly.

## 1) Add the image handler to the page model

We add `OnPostGenerateImage` to `Pages/Index.cshtml.cs`. This method reads the secrets, builds the request, sends the image generation request, and updates the image preview.

The page model property that receives the form value must be marked with `[BindProperty]`, otherwise the submitted `Prompt` can be empty when the handler runs.

```csharp
namespace AiDemo.Pages;

using System.Text;
using System.Text.Json;
using AiDemo.Models;
using Microsoft.AspNetCore.Mvc.RazorPages;

using Microsoft.AspNetCore.Mvc;

public class IndexModel : PageModel
{
    private readonly IHttpClientFactory _httpClientFactory;
    private readonly IConfiguration _configuration;

    [BindProperty]
    public string Prompt { get; set; } = string.Empty;

    public string GeneratedImageUrl { get; set; } = "https://placehold.co/800x500?text=Image+Preview";

    public IndexModel(IHttpClientFactory httpClientFactory, IConfiguration configuration)
    {
        _httpClientFactory = httpClientFactory;
        _configuration = configuration;
    }

    public async Task OnPostGenerateImage()
    {
        var endpoint = _configuration["Foundry:Endpoint"];
        var apiKey = _configuration["Foundry:ApiKey"];
        var model = _configuration["Foundry:ImageModel"];

        if (string.IsNullOrWhiteSpace(endpoint) || string.IsNullOrWhiteSpace(apiKey) || string.IsNullOrWhiteSpace(model))
        {
            GeneratedImageUrl = "https://placehold.co/800x500?text=Missing+secrets";
            return;
        }

        var client = _httpClientFactory.CreateClient();
        client.DefaultRequestHeaders.Add("api-key", apiKey);

        var request = new ImageGenerationRequest
        {
            Prompt = Prompt,
            Width = 1024,
            Height = 768,
            Model = model
        };

        var json = JsonSerializer.Serialize(request);
        using var content = new StringContent(json, Encoding.UTF8, "application/json");

        var response = await client.PostAsync(
            $"{endpoint.TrimEnd('/')}/mai/v1/images/generations",
            content);

        if (response.IsSuccessStatusCode)
        {
            var body = await response.Content.ReadAsStringAsync();

            using JsonDocument json = JsonDocument.Parse(body);
            string? base64Image = json.RootElement
                .GetProperty("data")[0]
                .GetProperty("b64_json")
                .GetString();

            if (!string.IsNullOrWhiteSpace(base64Image))
            {
                byte[] imageBytes = Convert.FromBase64String(base64Image);
                GeneratedImageUrl = $"data:image/png;base64,{Convert.ToBase64String(imageBytes)}";
            }
            else
            {
                GeneratedImageUrl = "https://placehold.co/800x500?text=Image+Result+Missing";
            }
        }
        else
        {
            GeneratedImageUrl = "https://placehold.co/800x500?text=Image+Request+Failed";
        }
    }
}
```

The important part is the request body and the custom page handler name. The image request is sent to the Foundry deployment only when the user clicks the image button.

##2) Run the app:


Use `dotnet run` in a Terminal, you will get an address such as `http://localhost:port` where your app is running on a specific port. Copy and paste the value in a browser and test the app.

---

## Notes

Here is the workflow:

- read the endpoint from Secret Manager
- read the API key from Secret Manager
- build the image request body with a prompt
- send the HTTP POST request
- show the result or a placeholder if something fails

This completes the first real AI feature: generating an image from a user prompt.

---

[Previous: Step 3 - Create request/response models and the web page](03-models-and-ui.md)  
[Next: Step 5 - Add GPT-5 text generation](05-add-gpt5-text.md)
