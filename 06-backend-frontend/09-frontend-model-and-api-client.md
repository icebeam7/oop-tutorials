# Step 9 - Add the frontend model and API client

We will separate the data contract from HTTP access and UI logic. 

Create `Models` and `Services` folders inside `frontend/Frontend.Web`. 

## 1. Create the product model

Create `Product.cs` in the `Models` folder:

```csharp
using System.ComponentModel.DataAnnotations;

namespace Frontend.Web.Models;

public class Product
{
    public int ProductID { get; set; }

    [Display(Name = "Product name")]
    [Required(ErrorMessage = "{0} Must Be Filled In.")]
    public string ProductName { get; set; } = string.Empty;

    [Required]
    [MaxLength(255)]
    public string Category { get; set; } = string.Empty;

    [Range(0.0, double.MaxValue, ErrorMessage = "Unit price cannot be negative.")]
    [Display(Name = "Unit price")]
    public decimal UnitPrice { get; set; }

    [Range(0, int.MaxValue)]
    [Display(Name = "Units in stock")]
    public int UnitsInStock { get; set; }

    public bool Discontinued { get; set; }
}
```

- The property names match the backend JSON contract, including `ProductID`. 
- `Required` rejects empty names and categories, while `Range` rejects negative prices and stock. 
- `Display` will be used in form labels. 

These attributes will validate Razor Pages forms on the server. 

## 2. Create the HTTP client class

Create `ProductApiClient.cs` in `Services` folder:

```csharp
using System.Net.Http.Json;
using Frontend.Web.Models;

namespace Frontend.Web.Services;

public class ProductApiClient
{
    private readonly HttpClient client;

    public ProductApiClient(HttpClient httpClient)
    {
        client = httpClient;
    }

    public async Task<List<Product>> GetAllAsync()
    {
        return await client.GetFromJsonAsync<List<Product>>("api/products")
            ?? new List<Product>();
    }
}
```

- Constructor: injection supplies a configured `HttpClient`. 
- `GetFromJsonAsync` does several things:
    - It sends a GET request
    - Then, checks for a successful status
    - If it's successful, it deserializes the JSON value
    - It including case-insensitive property matching. 
    - A JSON `null` value becomes an empty list. 
- HTTP failures will both return an empty list and will also raise an exception that the page will handle.

## 3. Register the typed client

In `Program.cs`, add these imports at the top:

```csharp
using Frontend.Web.Services;
using Microsoft.Extensions.Options;
```

Immediately after the existing `builder.Services.AddRazorPages();` and before `builder.Build()`, insert:

```csharp
var backendUrl = builder.Configuration["BackendApi:BaseUrl"];

Uri.TryCreate(backendUrl, UriKind.Absolute, out var backendUri);

builder.Services.AddHttpClient<ProductApiClient>(client =>
{
    client.BaseAddress = backendUri;
    client.Timeout = TimeSpan.FromSeconds(15);
});
```

- `AddHttpClient` registers a typed client and lets the framework manage underlying HTTP connection lifetimes. 
- `BaseAddress` supplies the common URL prefix; the timeout prevents an unavailable API from keeping a page waiting indefinitely.

## Checkpoint

From the `Frontend.Web` folder, run:

```bash
dotnet build
```

This compiles the model and client registration. If you run the frontend, you will see it still displays the template home page; in the next step, we will make the first product request.

[Previous: Step 8 - Create the Razor Pages frontend](08-create-razor-pages-project.md) | [Next: Step 10 - Load the products page](10-list-products.md)
