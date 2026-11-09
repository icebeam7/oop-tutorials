# Step 5 - Prepare the API endpoint configuration

First, configure the backend and keep `Program.cs` focused on application setup. In the next step, we will move the product route mappings into a dedicated endpoint file.

We will add the following operations:

- Get all products
- Get one product by id
- Add a new product
- Delete a product
- Update a product
- Search products by name

## 1. Confirm the Swagger package is already installed

In Part 1 we added the Swagger UI package to the backend project so the application can expose the API documentation page:

```bash
dotnet add backend/Backend.Api/Backend.Api.csproj package Swashbuckle.AspNetCore --version 10.0.0
```

### Why this package is needed

Swagger helps generate and expose a visual API documentation page. This is useful for developers and students because it makes testing endpoints easier without needing a separate tool such as Postman.

If you already added the package in step 1, you can continue without running the command again.

## 2. Update the project setup in `Program.cs`

Open `backend/Backend.Api/Program.cs` and replace the default content with:

```csharp
using Backend.Api.Data;
using Backend.Api.Endpoints;
using Backend.Api.Services;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var connectionString = builder.Configuration.GetConnectionString("AzureSql");

if (string.IsNullOrWhiteSpace(connectionString))
{
    throw new InvalidOperationException("Azure SQL connection string was not found.");
}

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(connectionString));

builder.Services.AddScoped<IProductService, ProductService>();

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "Backend API v1");
    });
}

app.MapProductEndpoints();

app.Run();
```

## 3. Explain the configuration block

### `builder.Services.AddEndpointsApiExplorer();`

This adds the API explorer, which Swagger uses to understand the available endpoints.

### `builder.Services.AddSwaggerGen();`

This registers Swagger generation for the project. Without this, Swagger UI would not know how to generate the documentation.

### `builder.Configuration.GetConnectionString("AzureSql")`

This reads the connection string from Secret Manager or the configuration system. This allows the app to access the database configuration without leaking the secret to the source code.

### `builder.Services.AddScoped<IProductService, ProductService>();`

This registers the service in the dependency injection container. A new `ProductService` is created for each request scope, and ASP.NET Core supplies it to endpoints that request `IProductService`.

## 4. Register the product endpoints

### `app.MapProductEndpoints();`

This calls an extension method that we will create in the next step. The extension method contains the product route mappings, keeping the startup file easier to scan.

## 5. Understand the endpoint namespace

The `using Backend.Api.Endpoints;` directive makes the endpoint extension method available in `Program.cs`. The endpoint class will use the model and service namespaces in its own file, so those imports do not need to be in `Program.cs`.

## 6. Build and check for errors

Run:

```bash
dotnet build
```

If there are no errors, continue to the next step to start the API and test it with Swagger.

## 7. What comes next

The application setup is ready. Next, create the endpoint class and add the product routes.

[Previous: Step 4 - Create the service layer](04-create-service-layer.md) | [Next: Step 6 - Map the product endpoints](06-map-product-endpoints.md)
