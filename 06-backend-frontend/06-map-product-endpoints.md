# Step 6 - Map the product endpoints

In this step, we will move all product route mappings out of `Program.cs` and into a dedicated endpoint class. This keeps application setup separate from the HTTP operations while preserving the Minimal API style.

## 1. Create the endpoint file

Create an `Endpoints` folder in `backend/Backend.Api`, then add `ProductEndpoints.cs` inside it:

## 2. Add the route mappings

Add the following code to `Endpoints/ProductEndpoints.cs`:

```csharp
using Backend.Api.Models;
using Backend.Api.Services;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Routing;

namespace Backend.Api.Endpoints;

public static class ProductEndpoints
{
    public static IEndpointRouteBuilder MapProductEndpoints(this IEndpointRouteBuilder app)
    {
        app.MapGet("/api/products", async (IProductService productService) =>
        {
            var products = await productService.GetAllAsync();
            return Results.Ok(products);
        });

        app.MapGet("/api/products/{id:int}", async (int id, IProductService productService) =>
        {
            var product = await productService.GetByIdAsync(id);
            return product is null ? Results.NotFound() : Results.Ok(product);
        });

        app.MapPost("/api/products", async (Product product, IProductService productService) =>
        {
            if (product is null)
            {
                return Results.BadRequest();
            }

            var createdProduct = await productService.AddAsync(product);
            return Results.Created($"/api/products/{createdProduct.ProductID}", createdProduct);
        });

        app.MapDelete("/api/products/{id:int}", async (int id, IProductService productService) =>
        {
            var wasDeleted = await productService.DeleteAsync(id);
            return wasDeleted ? Results.NoContent() : Results.NotFound();
        });

        app.MapPut("/api/products/{id:int}", async (int id, Product product, IProductService productService) =>
        {
            var updatedProduct = await productService.UpdateAsync(id, product);
            return updatedProduct is null ? Results.NotFound() : Results.Ok(updatedProduct);
        });

        app.MapGet("/api/products/search", async (string query, IProductService productService) =>
        {
            if (string.IsNullOrWhiteSpace(query))
            {
                return Results.BadRequest("The search term is required.");
            }

            var products = await productService.SearchAsync(query);
            return Results.Ok(products);
        });

        return app;
    }
}
```

## 3. Understand the endpoint class

### `public static class ProductEndpoints`

The class is static because it groups route registration code and does not need to hold per-request state.

### `MapProductEndpoints(this IEndpointRouteBuilder app)`

The `this` modifier makes this method an extension method. `IEndpointRouteBuilder` is the ASP.NET Core abstraction used to register routes. Returning `app` also allows endpoint registration methods to be chained if needed.

Each `MapGet`, `MapPost`, `MapPut`, or `MapDelete` call associates an HTTP method and URL pattern with a handler. The handler receives its parameters from the route, query string, request body, or dependency injection. Here, ASP.NET Core provides `IProductService` to each handler.

## 4. Review the routes and responses

- `GET /api/products` returns the product list with `200 OK`.
- `GET /api/products/{id}` returns one product with `200 OK`, or `404 Not Found` if it does not exist.
- `POST /api/products` creates a product and returns `201 Created`, including the new resource location.
- `DELETE /api/products/{id}` returns `204 No Content` when deleted, or `404 Not Found` if the product does not exist.
- `PUT /api/products/{id}` updates a product and returns `200 OK`, or `404 Not Found` if it does not exist.
- `GET /api/products/search?query=laptop` searches product names. An empty search term returns `400 Bad Request`.

These status codes let an API client understand whether a request succeeded and what happened to the requested resource.

## 5. Connect the endpoint class to `Program.cs`

In Step 5, `Program.cs` imports `Backend.Api.Endpoints` and calls:

```csharp
app.MapProductEndpoints();
```

That call registers every route in this class. Keep it after `builder.Build()` and the middleware configuration, and before `app.Run()`.

## 6. Build and check for errors

From the solution folder, run:

```bash
dotnet build
```

If the build succeeds, the API routes are ready to test.

[Previous: Step 5 - Prepare the API endpoint configuration](05-create-endpoints.md) | [Next: Step 7 - Run and test with Swagger UI](07-run-and-test-with-swagger.md)
