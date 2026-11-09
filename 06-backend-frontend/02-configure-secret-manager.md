# Step 2 - Protect the Azure SQL connection string with Secret Manager

In a real application, sensitive values such as database connection strings should never be committed directly to source control. To protect them, we use .NET Secret Manager.

We will add a database connection string SQL in the Secret Manager.

## 1. Why we use Secret Manager

When you work locally, the app can read secrets from your machine instead of a file in the repository. This allows you to keep connection strings and other sensitive values out of `appsettings.json`.

This is especially important when you use external services.


## 2. Initialize user secrets

Run:

```bash
dotnet user-secrets init
```

This creates a user-secrets store for the project. The secret values are saved on your local machine and are not part of the repository.

## 3. Add the database connection string

Now set the connection string as a secret:

```bash
dotnet user-secrets set "ConnectionStrings:AzureSql" "your-connection-string"
```

> Replace the placeholder with the database connection string value that is available in Moodle.

## 4. Read the secret from the application

In `Program.cs`, access it through the configuration system:

```csharp
var builder = WebApplication.CreateBuilder(args);

var connectionString = builder.Configuration.GetConnectionString("AzureSql");

if (string.IsNullOrWhiteSpace(connectionString))
{
    throw new InvalidOperationException("Azure SQL connection string was not found.");
}
```

### Why this code is important

- `builder.Configuration` reads configuration values from multiple sources.
- `GetConnectionString("AzureSql")` looks up the value saved in Secret Manager.
- `string.IsNullOrWhiteSpace(...)` ensures the value exists before the app continues.
- `throw new InvalidOperationException(...)` stops the app early with a clear error if the secret is missing.

## 5. Register EF Core with the Azure SQL connection string

Now that we have the secret, we can configure Entity Framework Core to use it.

First, install the SQL Server provider:

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Then create a database context class in a folder named `Data`:

```csharp
using Backend.Api.Models;
using Microsoft.EntityFrameworkCore;

namespace Backend.Api.Data;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
    {
    }

    public DbSet<Product> Products => Set<Product>();
}
```

Register it in `Program.cs`:

```csharp
using Backend.Api.Data;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

var connectionString = builder.Configuration.GetConnectionString("AzureSql");

if (string.IsNullOrWhiteSpace(connectionString))
{
    throw new InvalidOperationException("Azure SQL connection string was not found.");
}

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(connectionString));
```

### Why this is important

`AddDbContext` registers the database context in the dependency injection container. Then the service layer can receive the context through its constructor instead of manually creating SQL connections.

The `UseSqlServer(...)` method tells EF Core to use Azure SQL as the database provider.

## 6. Use the DbContext later in services

Once the context is registered, the service layer can use it directly. This is the pattern we will use in step 4:

```csharp
public class ProductService
{
    private readonly AppDbContext _context;

    public ProductService(AppDbContext context)
    {
        _context = context;
    }
}
```

From here, EF Core methods such as `Add`, `Find`, `Update`, and `SaveChanges` can be used for the CRUD operations.

## 7. Best practices

- never hardcode credentials into source code
- keep secrets in Secret Manager or environment variables
- use `appsettings.json` only for non-sensitive defaults or examples
- do not push `.usersecrets` files to Git

[Previous: Step 1 - Create the .NET solution and backend project](01-create-solution-and-backend.md) | [Next: Step 3 - Create the product model](03-create-models.md)
