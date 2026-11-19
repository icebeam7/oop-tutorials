# Step 10 - Load products when the page opens

This step implements only the product list. We will not add action buttons or forms yet. Create a `Pages/Products` folder.

## 1. Add the PageModel

Create `Pages/Products/Index.cshtml.cs`:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class IndexModel : PageModel
{
    private readonly ProductApiClient _products;

    public IndexModel(ProductApiClient products)
    {
        _products = products;
    }

    public List<Product> Items { get; private set; } = new();
    public string? ErrorMessage { get; private set; }

    public async Task OnGetAsync()
    {
        try
        {
            Items = await _products.GetAllAsync();
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ErrorMessage = "Products could not be loaded. Please try again later.";
        }
    }
}
```

Razor Pages invokes `OnGetAsync` when the browser visits this page with GET. The constructor receives the typed client from dependency injection, and `Items` carries the result to the view. HTTP errors and timeouts produce a readable error message, not a misleading empty-database message. Unexpected programming errors are left to the application's normal error handling.

## 2. Add the Razor view

Create `Pages/Products/Index.cshtml`:

```cshtml
@page
@model Frontend.Web.Pages.Products.IndexModel
@{
    ViewData["Title"] = "Products";
}

<h1>Products</h1>

@if (Model.ErrorMessage is not null)
{
    <div class="alert alert-danger" role="alert">@Model.ErrorMessage</div>
}
else if (Model.Items.Count == 0)
{
    <p>No products found.</p>
}
else
{
    <div class="table-responsive">
        <table class="table table-striped align-middle">
            <thead>
                <tr>
                    <th scope="col">Name</th>
                    <th scope="col">Category</th>
                    <th scope="col">Unit price</th>
                    <th scope="col">Stock</th>
                    <th scope="col">Discontinued</th>
                </tr>
            </thead>
            <tbody>
                @foreach (var product in Model.Items)
                {
                    <tr>
                        <td>@product.ProductName</td>
                        <td>@product.Category</td>
                        <td>@product.UnitPrice.ToString("N2")</td>
                        <td>@product.UnitsInStock</td>
                        <td>@(product.Discontinued ? "Yes" : "No")</td>
                    </tr>
                }
            </tbody>
        </table>
    </div>
}
```

`@page` makes this a routable Razor Page, while `@model` selects its PageModel. The conditional blocks distinguish failure, an empty result, and a populated table. Razor HTML-encodes product text automatically. `N2` displays two decimal places without assuming a currency, and the responsive wrapper allows horizontal scrolling on narrow screens. The generated template already includes Bootstrap.

## 3. Add a navigation entry

In `Pages/Shared/_Layout.cshtml`, insert this item inside the existing navbar `<ul>` next to Home and Privacy:

```cshtml
<li class="nav-item">
    <a class="nav-link text-dark" asp-page="/Products/Index">Products</a>
</li>
```

The `asp-page` Tag Helper generates a link from the Razor Page path. The template's `Pages/_ViewImports.cshtml` already enables MVC Tag Helpers, so no additional registration is needed.

## Checkpoint

Build the frontend as in Step 9 and restart it. Visit `http://localhost:5200/Products`. The page should load records from the API on each visit. An empty database should show "No products found." Stop the backend briefly, reload, and confirm the error message; then restart it before the next step.

[Previous: Step 9 - Add the frontend model and API client](09-frontend-model-and-api-client.md) | [Next: Step 11 - Add details navigation](11-add-details-navigation.md)