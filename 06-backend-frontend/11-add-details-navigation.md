# Step 11 - Add navigation to product details

The list already loads data. Now we will modify only its UI to give each row a details link. The destination page will be implemented in Step 12.

## 1. Add an Actions heading

In `Pages/Products/Index.cshtml`, add this heading immediately after the Discontinued heading:

```cshtml
<th scope="col">Actions</th>
```

This adds a column for operations on one product. It belongs inside the existing `<thead>` row, not in a second table.

## 2. Add a link to each row

Inside the existing `foreach` row, immediately after the Discontinued cell, insert:

```cshtml
<td>
    <a asp-page="./Details" asp-route-id="@product.ProductID"
       aria-label="View details for @product.ProductName">Details</a>
</td>
```

- `asp-page="./Details"` selects a sibling page in `Pages/Products`.
- `asp-route-id` supplies the product identifier. 
- In the next step, `@page "{id:int}"` will turn that value into a URL such as `/Products/Details/1`. 
- The accessible label identifies the product when several rows have the same visible link text.

## Checkpoint

Build the frontend and confirm that the new `Actions` column as well as a `Details` link appear for every row. Do not expect the details destination to work yet: routing cannot resolve it without the next step's page. 

[Previous: Step 10 - Load the products page](10-list-products.md) | [Next: Step 12 - Display product details](12-product-details.md)