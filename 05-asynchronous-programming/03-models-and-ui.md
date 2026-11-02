# Step 3 - Create request/response models and the web page

We need a clean structure for the data sent to the cloud and the data returned from it. 

We also need to design our UI (a web page).

## Create the model classes

Create a `Models` folder and add the following 2 classes:

**ImageGenerationRequest.cs**:

```csharp
namespace AiDemo.Models;

using System.Text.Json.Serialization;

public class ImageGenerationRequest
{
    [JsonPropertyName("prompt")]
    public string Prompt { get; set; } = string.Empty;
    
    [JsonPropertyName("width")]
    public int Width { get; set; } = 1024;
    
    [JsonPropertyName("height")]
    public int Height { get; set; } = 768;

    [JsonPropertyName("model")]
    public string Model { get; set; } = string.Empty;
}
```

**ImageGenerationResponse.cs**:

```csharp
namespace AiDemo.Models;

public class ImageGenerationResponse
{
    public string? ImageUrl { get; set; }
    public string? FileName { get; set; }
    public string? ErrorMessage { get; set; }
}
```

These are request and response classes with properties. They make it easier to map the JSON payload to C# objects and keep the code readable.

## Design the page

Update the home page (`Pages/Index.cshtml`) to include a label, a placeholder text input, a preview image, and a button. In Razor Pages, the button will later call a page handler named `GenerateImage`.

```cshtml
@page
@model IndexModel
@{
    ViewData["Title"] = "AI Demo";
}

<form method="post" class="mt-4">
    <div class="mb-3">
        <label for="prompt" class="form-label">Describe the image</label>

        <input id="prompt"
               name="Prompt"
               class="form-control"
               placeholder="Example: a futuristic city skyline at sunset"
               value="@Model.Prompt" />
    </div>

    <button type="submit" asp-page-handler="GenerateImage" class="btn btn-primary">
        Generate image
    </button>



</form>

<div class="mt-4">
    <img src="@Model.GeneratedImageUrl"
         alt="Generated image"
         class="img-fluid border rounded"
         style="max-height: 420px;" />
</div>
```

The important idea is that the form will submit to a page handler named `GenerateImage`, and the model will store the generated image URL in `GeneratedImageUrl` so the image can appear in the page after the request returns.

A very common beginner problem is that the field from the form does not bind to the page model. In Razor Pages, the fix is to mark the property with `[BindProperty]` so the posted value is copied into the `Prompt` property before the handler executes.

```csharp
[BindProperty]
public string Prompt { get; set; } = string.Empty;
```

Without this, the request may arrive with an empty prompt even though the input field exists in the HTML.

---

## Next step

The next step adds the page model method `OnPostGenerateImage`, which is the code that reads the secret values, calls the image model, and assigns the result to `GeneratedImageUrl`.

---

[Previous: Step 2 - Store Foundry secrets safely](02-setup-secrets.md) | 
[Next: Step 4 - Send the image request to Foundry](04-call-image-model.md)
