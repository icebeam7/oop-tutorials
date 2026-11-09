# 06 - Backend + Frontend (Part 1)

This section is the first part of a full-stack project where we will build a .NET 10 solution with two projects:

- a backend project using Minimal API
- a frontend project using ASP.NET Core Razor Pages

This first part focuses only on the backend. We will create the solution, the backend project, add the Swagger UI package in the first stage, configure secret management for an Azure SQL connection string, build the object model, create the service layer, and expose the endpoints with CRUD and search operations.

## Learning objectives

By the end of this part, you will be able to:

- create a .NET 10 solution and project with VS Code and the terminal
- add the Swagger UI NuGet package during the backend setup
- use .NET Secret Manager to protect sensitive configuration values
- organize the backend with OOP layers: models, service interface, service implementation, and endpoints
- build a Minimal API that exposes endpoint operations
- run the app and test it through Swagger UI

## Content

1. [Step 1 - Create the .NET solution and backend project](01-create-solution-and-backend.md)
2. [Step 2 - Add a database connection string](02-configure-secret-manager.md)
3. [Step 3 - Create the product model](03-create-models.md)
4. [Step 4 - Create the service layer](04-create-service-layer.md)
5. [Step 5 - Create the API endpoints](05-create-endpoints.md)
6. [Step 6 - Map the product endpoints](06-map-product-endpoints.md)
7. [Step 7 - Run the app and test it with Swagger UI](07-run-and-test-with-swagger.md)

[Start here: Step 1 - Create the .NET solution and backend project](01-create-solution-and-backend.md)
