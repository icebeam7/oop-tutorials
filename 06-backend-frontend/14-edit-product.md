# Step 14 - Edit a product

Editing has two requests: GET loads the current data, and POST validates the submitted values and sends an API PUT. We will reuse the fields from Step 13.

## 1. Extend the API client

Add this method inside `ProductApiClient`:

```csharp
public async Task<bool> UpdateAsync(int id, Product product)
{
    using var response = await _httpClient.PutAsJsonAsync($"api/products/{id}", product);

    if (response.StatusCode == HttpStatusCode.NotFound)
    {
        return false;
    }

    response.EnsureSuccessStatusCode();
    return true;
}
```

`PutAsJsonAsync` sends the new field values to the product URL. `false` means that the target no longer exists; other failures raise an exception. We only need success or missing here, because the next details request will load the saved record.

## 2. Create the edit PageModel

Create `Pages/Products/Edit.cshtml.cs`:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class EditModel : PageModel
{
    private readonly ProductApiClient _products;

    public EditModel(ProductApiClient products)
    {
        _products = products;
    }

    [BindProperty]
    public Product Input { get; set; } = new();

    public bool CanEdit { get; private set; }
    public string? ErrorMessage { get; private set; }

    public async Task<IActionResult> OnGetAsync(int id)
    {
        try
        {
            var product = await _products.GetByIdAsync(id);

            if (product is null)
            {
                return NotFound();
            }

            Input = product;
            CanEdit = true;
            return Page();
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ErrorMessage = "The product could not be loaded for editing.";
            return Page();
        }
    }

    public async Task<IActionResult> OnPostAsync(int id)
    {
        CanEdit = true;

        if (!ModelState.IsValid)
        {
            return Page();
        }

        try
        {
            Input.ProductID = id;
            var updated = await _products.UpdateAsync(id, Input);

            if (!updated)
            {
                return NotFound();
            }

            return RedirectToPage("./Details", new { id });
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ModelState.AddModelError(string.Empty,
                "The update could not be confirmed. Check the product details before trying again.");
            return Page();
        }
    }
}
```

GET pre-fills `Input`, while POST uses model binding to receive the edited fields. The route ID is used for the API URL and copied to the payload; we do not trust a hidden posted ID. `CanEdit` prevents rendering an empty edit form when the initial GET fails, but keeps submitted values visible after validation or API errors. A record deleted between GET and POST still produces 404.

## 3. Create the edit view

Create `Pages/Products/Edit.cshtml`:

```cshtml
@page "{id:int}"
@model Frontend.Web.Pages.Products.EditModel
@{
    ViewData["Title"] = "Edit product";
}

<h1>Edit product</h1>

@if (Model.ErrorMessage is not null)
{
    <div class="alert alert-danger" role="alert">@Model.ErrorMessage</div>
}

@if (Model.CanEdit)
{
    <form method="post" asp-route-id="@RouteData.Values["id"]">
        <div asp-validation-summary="All" class="text-danger" role="alert"></div>
        <partial name="_ProductFields" for="Input" />
        <button type="submit" class="btn btn-primary">Save</button>
        <a asp-page="./Index" class="ms-2">Cancel</a>
    </form>
}
else
{
    <a asp-page="./Index">Back to products</a>
}

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

The integer route identifies the product for both handlers. The form explicitly preserves that route ID, and the partial binds to `Input` exactly as it did on Create. Returning `Page()` after an invalid POST preserves the attempted values and validation messages rather than replacing them with another API GET.

## 4. Add an Edit link to the list

In `Pages/Products/Index.cshtml`, add this link in the existing Actions cell after Details:

```cshtml
<a asp-page="./Edit" asp-route-id="@product.ProductID" class="ms-2"
   aria-label="Edit @product.ProductName">Edit</a>
```

This passes the row's ID to the edit page without changing the existing list-loading logic or adding another Actions column.

## Checkpoint

Build and restart the frontend. Edit a price, save, and verify it on Details and in the list. Try invalid input, Cancel, and a missing ID. 

[Previous: Step 13 - Create a product](13-create-product.md) | [Next: Step 15 - Delete a product](15-delete-product.md)