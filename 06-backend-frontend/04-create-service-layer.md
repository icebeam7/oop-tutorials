# Step 4 - Create the service layer

Now we move to the service layer, which is one of the key OOP pieces in this backend. The service layer contains the business logic and acts as the bridge between the API endpoints and the data source.

In this part, we will create:

- an interface for the contract
- a class that implements the interface
- the service logic that will later work with the Azure SQL connection string from Secret Manager

## 1. Create the Services folder

Inside the backend project, create a folder named `Services`:

## 2. Create the interface

Create a file named `IProductService.cs`:

```csharp
using Backend.Api.Models;

namespace Backend.Api.Services;

public interface IProductService
{
    Task<IEnumerable<Product>> GetAllAsync();
    Task<Product?> GetByIdAsync(int id);
    Task<Product> AddAsync(Product product);
    Task<bool> DeleteAsync(int id);
    Task<Product?> UpdateAsync(int id, Product product);
    Task<IEnumerable<Product>> SearchAsync(string term);
}
```

The interface defines the contract that any implementation must follow.

## 3. Explanation

### `Task<IEnumerable<Product>> GetAllAsync();`

A method that will return all products asynchronously.

### `Task<Product?> GetByIdAsync(int id);`

A method that will return one product by its identifier, or `null` if it does not exist.

### `Task<Product> AddAsync(Product product);`

A method that will add a new product and return the created object.

### `Task<bool> DeleteAsync(int id);`

A method that will delete a product and return whether the operation succeeded.

### `Task<Product?> UpdateAsync(int id, Product product);`

A method that will update an existing product and return the updated value.

### `Task<IEnumerable<Product>> SearchAsync(string term);`

A method that will search products by a term, such as the product name.

## 4. Create the implementation class

Create a file named `ProductService.cs`:

```csharp
using Backend.Api.Data;
using Backend.Api.Models;
using Microsoft.EntityFrameworkCore;

namespace Backend.Api.Services;

public class ProductService : IProductService
{
    private readonly AppDbContext _context;

    public ProductService(AppDbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<Product>> GetAllAsync()
    {
        return await _context.Products.ToListAsync();
    }

    public async Task<Product?> GetByIdAsync(int id)
    {
        return await _context.Products.FindAsync(id).AsTask();
    }

    public async Task<Product> AddAsync(Product product)
    {
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        return product;
    }

    public async Task<bool> DeleteAsync(int id)
    {
        var product = await _context.Products.FindAsync(id);

        if (product is null)
        {
            return false;
        }

        _context.Products.Remove(product);
        await _context.SaveChangesAsync();
        return true;
    }

    public async Task<Product?> UpdateAsync(int id, Product product)
    {
        var existingProduct = await _context.Products.FindAsync(id);

        if (existingProduct is null)
        {
            return null;
        }

        existingProduct.ProductName = product.ProductName;
        existingProduct.Category = product.Category;
        existingProduct.UnitPrice = product.UnitPrice;
        existingProduct.UnitsInStock = product.UnitsInStock;
        existingProduct.Discontinued = product.Discontinued;

        await _context.SaveChangesAsync();
        return existingProduct;
    }

    public async Task<IEnumerable<Product>> SearchAsync(string term)
    {
        return await _context.Products
            .Where(p => p.ProductName.Contains(term))
            .ToListAsync();
    }
}
```

## 5. Explain the implementation

### `private readonly AppDbContext _context;`

This property stores the EF Core database context. It is the object that knows how to talk to Azure SQL through the SQL Server provider.

### `public ProductService(AppDbContext context)`

This constructor receives the `AppDbContext`, which was registered in step 2 with `AddDbContext` and `UseSqlServer(...)`.

### `GetAllAsync()`

This method uses `_context.Products.ToListAsync()` to fetch all products from the database.

### `GetByIdAsync(int id)`

This method uses `FindAsync(id)` to locate a product by its primary key.

### `AddAsync(Products product)`

This method uses `_context.Products.Add(product)` and then `SaveChangesAsync()` to persist the entity.

### `DeleteAsync(int id)`

This method searches for the product, removes it with `Remove(...)`, and saves the changes.

### `UpdateAsync(int id, Products product)`

This method finds the existing row, updates its values, and writes the changes back to the database.

### `SearchAsync(string term)`

This method filters the products by `ProductName` using LINQ and the EF Core query provider.

## 6. Why this design is useful

This is a very common backend pattern:

- models describe the data
- interfaces define the behavior
- implementations contain the logic
- endpoints call these services

This keeps your API organized and makes the code much easier to replace later with a real Azure SQL repository implementation.

## 7. What is next

The service layer is ready. The next step is to connect these services to HTTP endpoints so clients can call them through a web API.

[Previous: Step 3 - Create the product model](03-create-models.md) | [Next: Step 5 - Prepare the API endpoint configuration](05-create-endpoints.md)
