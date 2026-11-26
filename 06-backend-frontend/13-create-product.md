# Step 13 - Create a product

We will add a creation form, submit it to the frontend with POST, and let the PageModel send JSON to the API. The browser never posts directly to the backend.

## 1. Add the client operation

Inside `ProductApiClient`, add:

```csharp
public async Task<Product> CreateAsync(Product product)
{
    using var response = await _httpClient.PostAsJsonAsync("api/products", product);
    response.EnsureSuccessStatusCode();

    return await response.Content.ReadFromJsonAsync<Product>()
        ?? throw new HttpRequestException("The API returned no created product.");
}
```

`PostAsJsonAsync` serializes the model and sets the JSON content type. The backend returns the created record, including its generated ID. Checking the status before reading prevents an error response from being mistaken for a product.

## 2. Add the creation PageModel

Create `Pages/Products/Create.cshtml.cs`:

```csharp
using Frontend.Web.Models;
using Frontend.Web.Services;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace Frontend.Web.Pages.Products;

public class CreateModel : PageModel
{
    private readonly ProductApiClient _products;

    public CreateModel(ProductApiClient products)
    {
        _products = products;
    }

    [BindProperty]
    public Product Input { get; set; } = new();

    public void OnGet()
    {
    }

    public async Task<IActionResult> OnPostAsync()
    {
        if (!ModelState.IsValid)
        {
            return Page();
        }

        try
        {
            Input.ProductID = 0;
            var created = await _products.CreateAsync(Input);
            return RedirectToPage("./Details", new { id = created.ProductID });
        }
        catch (Exception exception) when (exception is HttpRequestException or TaskCanceledException)
        {
            ModelState.AddModelError(string.Empty,
                "The request could not be completed. Check the product list before trying again.");
            return Page();
        }
    }
}
```

- `BindProperty` binds submitted fields to `Input`. 
- `ModelState.IsValid` checks the Step 9 validation attributes before calling the API. 
- The server resets the ID because creation must use a database-generated identifier, not a value supplied by the browser. 
- After success, `Post/Redirect/Get` navigates to the new details page and avoids repeating the POST on refresh.
- A network failure can occur after the API has saved a record. The error asks the user to check the list before retrying so a lost response does not encourage duplicate creation. 

## 3. Create reusable product fields

Create `Pages/Products/_ProductFields.cshtml`:

```cshtml
@model Frontend.Web.Models.Product

<div class="mb-3">
    <label asp-for="ProductName" class="form-label"></label>
    <input asp-for="ProductName" class="form-control" />
    <span asp-validation-for="ProductName" class="text-danger"></span>
</div>
<div class="mb-3">
    <label asp-for="Category" class="form-label"></label>
    <input asp-for="Category" class="form-control" />
    <span asp-validation-for="Category" class="text-danger"></span>
</div>
<div class="mb-3">
    <label asp-for="UnitPrice" class="form-label"></label>
    <input asp-for="UnitPrice" type="number" min="0" step="0.01" class="form-control" />
    <span asp-validation-for="UnitPrice" class="text-danger"></span>
</div>
<div class="mb-3">
    <label asp-for="UnitsInStock" class="form-label"></label>
    <input asp-for="UnitsInStock" type="number" min="0" step="1" class="form-control" />
    <span asp-validation-for="UnitsInStock" class="text-danger"></span>
</div>
<div class="form-check mb-3">
    <input asp-for="Discontinued" class="form-check-input" />
    <label asp-for="Discontinued" class="form-check-label"></label>
</div>
```

- This **partial view** contains only editable fields. 
- Tag Helpers generate labels, names, current values, checkbox handling, and validation attributes from the model. 
- Numeric inputs provide browser hints, while server-side validation still checks submitted values. 
- `Create` and `Edit` pages will share this partial page to keep their fields consistent.

## 4. Add the creation view

Create `Pages/Products/Create.cshtml`:

```cshtml
@page
@model Frontend.Web.Pages.Products.CreateModel
@{
    ViewData["Title"] = "Create product";
}

<h1>Create product</h1>

<form method="post">
    <div asp-validation-summary="All" class="text-danger" role="alert"></div>
    <partial name="_ProductFields" for="Input" />
    <button type="submit" class="btn btn-primary">Create</button>
    <a asp-page="./Index" class="ms-2">Cancel</a>
</form>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

- The form posts to the current page. 
- The Form Tag Helper generates an antiforgery token, which Razor Pages validates automatically. 
- The `for="Input"` partial invocation preserves the `Input` field prefix, so names such as `Input.ProductName` bind to the PageModel correctly. 
- Keep `for`, rather than replacing it with `model`, to preserve that prefix.

The validation summary shows both field errors and API failures. `_ValidationScriptsPartial` is included in the generated template and enables client-side validation as a convenience; server checks work even without JavaScript.

## 5. Add the list-page link

Immediately after `<h1>Products</h1>` in `Pages/Products/Index.cshtml`, insert:

```cshtml
<p><a asp-page="./Create">Create product</a></p>
```

This is navigation to the form.

## Checkpoint

Build and restart the frontend. Create a product, confirm the redirect to its details, and return to the list. Submit an empty name or negative stock and confirm validation prevents submission. Test with JavaScript disabled as well to verify server-side validation. Confirm the new product in the database.

[Previous: Step 12 - Display product details](12-product-details.md) | [Next: Step 14 - Edit a product](14-edit-product.md)