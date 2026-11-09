# Step 7 - Run the app and test it with Swagger UI

This is the final step of part 1. We will run the backend application and use Swagger UI to verify the endpoints work as expected.

## 1. Start the backend app

From the project folder, run:

```bash
cd backend/Backend.Api
dotnet run
```

You should see output similar to:

```text
Now listening on: https://localhost:xxxx
Application started. Press Ctrl+C to shut down.
```

The port number will vary, depending on your machine and launch settings.

## 2. Open Swagger UI

In the browser, open the Swagger page. Usually it is:

```text
https://localhost:<port>/swagger
```

Swagger displays all the API routes grouped by endpoint and method.

## 3. Explore the operations

You should see routes such as:

- `GET /api/products`
- `GET /api/products/{id}`
- `POST /api/products`
- `PUT /api/products/{id}`
- `DELETE /api/products/{id}`
- `GET /api/products/search`

This is a clean and simple way to understand the API contract.

## 4. Test the `Get all` endpoint

Click on:

```text
GET /api/products
```

Then press the **Try it out** button and **Execute**.

### Expected response

A JSON array of product objects is returned, such as:

```json
[
  {
    "productID": 1,
    "productName": "Laptop",
    "category": "Electronics",
    "unitPrice": 1200.00,
    "unitsInStock": 10,
    "discontinued": false
  },
  {
    "productID": 2,
    "productName": "Mouse",
    "category": "Accessories",
    "unitPrice": 29.99,
    "unitsInStock": 50,
    "discontinued": false
  }
]
```

The exact result depends on the records in your Azure SQL database.

## 5. Test the `Get one` endpoint

Use an ID such as `1` and call:

```text
GET /api/products/1
```

Expected response: the details of the product with id 1, if it exists in the database.

## 6. Test the `Add` endpoint

Open the `POST /api/products` endpoint and send a JSON payload like:

```json
{
  "productName": "Headphones",
  "category": "Electronics",
  "unitPrice": 199.99,
  "unitsInStock": 15,
  "discontinued": false
}
```

### What happens internally

- the API receives the JSON object
- the endpoint passes it to `AddAsync`
- the service adds the entity to the EF Core context and saves it to Azure SQL
- the response returns `201 Created`

## 7. Test the `Delete` endpoint

Use:

```text
DELETE /api/products/3
```

If the product exists, the status should be `204 No Content`.

## 8. Test the `Update` endpoint

Use `PUT /api/products/2` with a JSON body like:

```json
{
  "productName": "Gaming Mouse",
  "category": "Accessories",
  "unitPrice": 45.99,
  "unitsInStock": 25,
  "discontinued": false
}
```

The response should be `200 OK` and the updated record.

## 9. Test the search endpoint

Use:

```text
GET /api/products/search?query=mouse
```

This should return products whose names contain `mouse`.

## 10. Why Swagger is valuable

Swagger UI is very helpful because it lets you:

- visualize the endpoints
- learn the expected request shape
- test the API without writing custom client code
- validate status codes and response bodies quickly

This is especially useful while building and debugging a backend.

## 11. Common problems and fixes

### Problem: Swagger does not open

Check whether the app is still running and verify that the app started without errors.

### Problem: 500 error or missing configuration

Make sure the secret was configured correctly:

```bash
dotnet user-secrets list
```

Verify that the key `ConnectionStrings:AzureSql` exists.

### Problem: app does not start

Check the terminal output for exceptions. A missing secret or a misconfigured startup block will usually show a clear error message.

## 12. Final reflection

At this point, you have created a working Minimal API backend with:

- models
- a service interface and implementation
- a full CRUD API
- a search endpoint
- Swagger UI
- Secret Manager configuration for Azure SQL

This is the foundation for the next part: adding the frontend project to the same solution.

[Previous: Step 6 - Map the product endpoints](06-map-product-endpoints.md)

> Part 2 will add the frontend app and connect it to this backend.
