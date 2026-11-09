# Step 3 - Create the product model

Now we start organizing the backend using OOP principles. The first layer is the model layer, which represents the data structure used by the API.

In this tutorial, we will use a simple `Product` entity. The same pattern can be extended to `Customer`, `Order`, or any other domain class.

## 1. Create the Models folder

Inside the backend project, create a folder named `Models`:

```bash
mkdir Models
```

Then create a file named `Product.cs` inside it.

## 2. Add the model code

```csharp
namespace Backend.Api.Models;

public class Product
{
    public int ProductID { get; set; }
    public string ProductName { get; set; } = string.Empty;
    public string Category { get; set; }
    public decimal UnitPrice { get; set; }
    public int UnitsInStock { get; set; }
    public bool Discontinued { get; set; } = false;
}
```

The model is the representation of your domain. It becomes the contract between layers:

- the endpoint receives and returns `Product` objects
- the service uses the model to manipulate data
- the database layer later maps these properties to table columns

This separation is an important part of OOP and clean architecture.

[Previous: Step 2 - Add a database connection string](02-configure-secret-manager.md) | [Next: Step 4 - Create the service layer](04-create-service-layer.md)
