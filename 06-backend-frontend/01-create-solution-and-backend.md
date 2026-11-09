# Step 1 - Create the .NET solution and backend project

In this first step, we create the project structure we will use throughout the tutorial. We will work inside VS Code and use the terminal to create a .NET solution and a backend project.

## 1. Create the solution

Run this command in the terminal:

```bash
dotnet new sln -n CompanySolution
```

This creates an empty .NET solution named `CompanySolution`.

A solution is a container that can hold multiple projects.

## 2. Create a folder for your source code

We will keep the backend in a dedicated folder:

```bash
mkdir backend
cd backend
```

## 3. Create the backend project

We will create a Minimal API backend project named `Backend.Api` targeting .NET 10:

```bash
dotnet new webapi -n Backend.Api
```

Minimal API is a lightweight way to build APIs in .NET. It keeps the code clear and focused.

## 4. Add the project to the solution

Go back to the solution root and add the backend project:

```bash
cd ..
dotnet sln CompanySolution.slnx add backend/Backend.Api/Backend.Api.csproj
```

Now the solution contains the backend project.

## 5. Add the Swagger UI NuGet package

Before we run the app, add the Swagger package so the API documentation UI is available:

```bash
cd backend/Backend.Api
dotnet add package Swashbuckle.AspNetCore --version 10.0.0
```

Swagger UI generates a visual documentation page for the API. You can use it to test each route directly in the browser without needing additional tools.

## 6. Run the default app to confirm everything works

Execute the project:

```bash
dotnet run
```

When the app starts, you should see the localhost URL in the terminal. By default, the app includes a weather forecast example.

Open the browser and navigate to:

```text
https://localhost:<some-port>/swagger
```

If the app starts and Swagger loads, the backend is correctly configured.

## 7. Explain the default generated code

The file `Program.cs` is the entry point of the app. In a Minimal API, the application is configured in a single place.

A basic generated program includes:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

### What each line does

- `WebApplication.CreateBuilder(args)` creates the application builder object.
- `builder.Build()` builds the app pipeline.
- `app.MapGet(...)` registers an HTTP GET endpoint.
- `app.Run()` starts the web server.

[Previous: README](README.md) | [Next: Step 2 - Add a database connection string](02-configure-secret-manager.md)
