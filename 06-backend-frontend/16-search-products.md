# Step 16 - Search products from the list page

Search will use a query string so that results can be bookmarked and reloaded without submitting a form POST.

## 1. Add the client search method

Inside `ProductApiClient`, add:

```csharp
public async Task<List<Product>> SearchAsync(string query)
{
    var encodedQuery = Uri.EscapeDataString(query);

    return await _httpClient.GetFromJsonAsync<List<Product>>(
        $"api/products/search?query={encodedQuery}")
        ?? new List<Product>();
}
```

Encoding the term prevents spaces, ampersands, and other reserved characters from changing the meaning of the URL. The parameter name is `query`, matching the backend endpoint. Searching stays in the backend so the frontend does not need to download the whole catalog to filter it.

## 2. Replace the list PageModel

Replace `Pages/Products/Index.cshtml.cs` with:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class IndexModel : PageModel
{
    private readonly ProductApiClient _products;

    public IndexModel(ProductApiClient products)
    {
        _products = products;
    }

    [BindProperty(SupportsGet = true)]
    public string? Query { get; set; }

    public List<Product> Items { get; private set; } = new();
    public string? ErrorMessage { get; private set; }

    public async Task OnGetAsync()
    {
        try
        {
            Query = Query?.Trim();
            Items = string.IsNullOrWhiteSpace(Query)
                ? await _products.GetAllAsync()
                : await _products.SearchAsync(Query);
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ErrorMessage = "Products could not be loaded. Please try again later.";
        }
    }
}
```

`SupportsGet = true` permits query-string binding into `Query`; normal `BindProperty` binds POST by default. Trimming avoids treating spaces as a meaningful term. An empty term calls the original list operation instead of sending an invalid search request. The list and error properties remain unchanged, so the existing table and action links continue working.

## 3. Add a search form

In `Pages/Products/Index.cshtml`, insert this form after the Create product link and before the error/empty/table conditional:

```cshtml
<form method="get" asp-page="./Index" class="mb-3">
    <label asp-for="Query" class="form-label">Search by product name</label>
    <div class="d-flex flex-wrap gap-2">
        <input asp-for="Query" type="search" class="form-control w-auto"
               placeholder="Product name" />
        <button type="submit" class="btn btn-primary">Search</button>
        <a asp-page="./Index" class="align-self-center">Clear</a>
    </div>
</form>
```

The form submits a safe GET such as `/Products?Query=mouse`. `asp-for` keeps the search term in the input after the request. Clear links to the list without a query string, restoring all records. The wrapping layout accommodates narrow screens. Keep the table, Actions heading, and Details/Edit/Delete links added in earlier steps.

## Checkpoint

Build and restart the frontend. Search for part of a product name, reload the URL, and confirm the same results. Clear should restore the complete list. 

[Previous: Step 15 - Delete a product](15-delete-product.md)