# Step 15 - Delete a product with confirmation

A GET must never delete data. We will first display the product, then require a confirmation form POST before sending DELETE to the API.

## 1. Add the delete client method

Inside `ProductApiClient`, add:

```csharp
public async Task<bool> DeleteAsync(int id)
{
    using var response = await _httpClient.DeleteAsync($"api/products/{id}");

    if (response.StatusCode == HttpStatusCode.NotFound)
    {
        return false;
    }

    response.EnsureSuccessStatusCode();
    return true;
}
```

The API normally returns `204 No Content`, so there is no JSON body to read. A missing product returns `false`; an unavailable API or another unsuccessful response is an error.

## 2. Create the Delete PageModel

Create `Pages/Products/Delete.cshtml.cs`:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class DeleteModel : PageModel
{
    private readonly ProductApiClient _products;

    public DeleteModel(ProductApiClient products)
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
            ErrorMessage = "The product could not be loaded for deletion.";
            return Page();
        }
    }

    public async Task<IActionResult> OnPostAsync(int id)
    {
        try
        {
            var deleted = await _products.DeleteAsync(id);

            if (!deleted)
            {
                return NotFound();
            }

            return RedirectToPage("./Index");
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ErrorMessage = "Deletion could not be confirmed. Return to the list to check the current state.";
            return Page();
        }
    }
}
```

GET only reads the product. POST sends the delete request using the route ID and redirects to the list on success. No posted product fields are needed. Razor Pages validates the antiforgery token before invoking the POST handler. If the request fails, we do not immediately retry a destructive operation or assume that the backend left the record unchanged.

## 3. Create the delete view

Create `Pages/Products/Delete.cshtml`:

```cshtml
@page "{id:int}"
@model Frontend.Web.Pages.Products.DeleteModel
@{
    ViewData["Title"] = "Delete product";
}

<h1>Delete product</h1>

@if (Model.ErrorMessage is not null)
{
    <div class="alert alert-danger" role="alert">@Model.ErrorMessage</div>
}
else if (Model.Item is { } product)
{
    <p>Delete <strong>@product.ProductName</strong> (ID @product.ProductID)?</p>
    <p>This action cannot be undone.</p>
    <form method="post" asp-route-id="@product.ProductID">
        <button type="submit" class="btn btn-danger">Delete</button>
        <a asp-page="./Index" class="ms-2">Cancel</a>
    </form>
}

@if (Model.ErrorMessage is not null)
{
    <a asp-page="./Index">Back to products</a>
}
```

The form exists only after a successful read. The product name and ID let the user verify the target before confirming. On an uncertain deletion result, the view shows a return link instead of another confirmation form. Cancel is a GET link and performs no deletion.

## 4. Add the navigation link

Add this link after Edit in the existing Actions cell of `Pages/Products/Index.cshtml`:

```cshtml
<a asp-page="./Delete" asp-route-id="@product.ProductID" class="ms-2 text-danger"
   aria-label="Delete @product.ProductName">Delete</a>
```

Despite its label, this link only opens the confirmation page. The operation itself is performed by its form POST.

## Checkpoint

Build and restart the frontend. Create a disposable product, open Delete, and choose Cancel; verify the record still exists. Repeat and confirm deletion; it should disappear from the list. Visit its old details URL and expect 404. Submitting a POST without the generated antiforgery token should return 400 before the handler runs.

[Previous: Step 14 - Edit a product](14-edit-product.md) | [Next: Step 16 - Search products](16-search-products.md)