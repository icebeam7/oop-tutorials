# Step 12 - Display product details

Now we will complete the destination introduced in Step 11. The page will request exactly one product and return 404 if the API says it does not exist.

## 1. Extend the API client

Add `using System.Net;` at the top of `Services/ProductApiClient.cs`, then add this method inside its existing class, below `GetAllAsync`:

```csharp
public async Task<Product?> GetByIdAsync(int id)
{
    using var response = await _httpClient.GetAsync($"api/products/{id}");

    if (response.StatusCode == HttpStatusCode.NotFound)
    {
        return null;
    }

    response.EnsureSuccessStatusCode();
    return await response.Content.ReadFromJsonAsync<Product>();
}
```

- The method treats 404 as an expected missing product rather than an application failure. 
- Other unsuccessful status codes raise `HttpRequestException`. 
- `using var` disposes the HTTP response after its content has been read. 

## 2. Create the details PageModel

Create `Pages/Products/Details.cshtml.cs`:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class DetailsModel : PageModel
{
    private readonly ProductApiClient _products;

    public DetailsModel(ProductApiClient products)
    {
        _products = products;
    }

    public Product? Item { get; private set; }
    public string? ErrorMessage { get; private set; }

    public async Task<IActionResult> OnGetAsync(int id)
    {
        try
        {
            Item = await _products.GetByIdAsync(id);
            return Item is null ? NotFound() : Page();
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ErrorMessage = "Product details could not be loaded. Please try again later.";
            return Page();
        }
    }
}
```

- Model binding reads `id` from the route. 
- Returning `Page()` renders the view.
- `NotFound()` returns HTTP 404. 
- The nullable `Item` reflects that loading can fail, and the view below checks it before rendering fields. 
- A backend failure is displayed separately from a missing product.

## 3. Create the details view

Create `Pages/Products/Details.cshtml`:

```cshtml
@page "{id:int}"
@model Frontend.Web.Pages.Products.DetailsModel
@{
    ViewData["Title"] = "Product details";
}

<h1>Product details</h1>

@if (Model.ErrorMessage is not null)
{
    <div class="alert alert-danger" role="alert">@Model.ErrorMessage</div>
}
else if (Model.Item is not null)
{
    var product = Model.Item;

    <dl class="row">
        <dt class="col-sm-3">ID</dt>
        <dd class="col-sm-9">@product.ProductID</dd>
        <dt class="col-sm-3">Name</dt>
        <dd class="col-sm-9">@product.ProductName</dd>
        <dt class="col-sm-3">Category</dt>
        <dd class="col-sm-9">@product.Category</dd>
        <dt class="col-sm-3">Unit price</dt>
        <dd class="col-sm-9">@product.UnitPrice.ToString("N2")</dd>
        <dt class="col-sm-3">Units in stock</dt>
        <dd class="col-sm-9">@product.UnitsInStock</dd>
        <dt class="col-sm-3">Discontinued</dt>
        <dd class="col-sm-9">@(product.Discontinued ? "Yes" : "No")</dd>
    </dl>
}

<a asp-page="./Index">Back to products</a>
```

The integer route constraint rejects URLs with a non-integer ID. The description list pairs each label with its value. 

## Checkpoint

- Build and restart the frontend. 
- Choose a product and click on its `Details` link. See its data in a new page. 
- You can also modify the URL to visit a non-existent ID and confirm HTTP 404 error. 
- Choose a valid product and navigate to its `Details` page. Stop the backend, refresh the `Details` page, confirm the error message appears.

[Previous: Step 11 - Add details navigation](11-add-details-navigation.md) | [Next: Step 13 - Create a product](13-create-product.md)