# Step 8 - Create the Razor Pages frontend

Part 2 builds a frontend for the backend we have just created. We will implement the following features:

- List products
- Navigate to the details of a product
- Create a product
- Edit a product
- Delete a product
- Search products. 

Keep the backend from Steps 1-7 available (running by using `dotnet run`) throughout these lessons.

## 1. Add a second project to the solution

From the folder containing `CompanySolution.slnx`, run the following commands:

```bash
dotnet new webapp -n Frontend.Web -o frontend/Frontend.Web
dotnet sln CompanySolution.slnx add frontend/Frontend.Web/Frontend.Web.csproj
```

- `webapp` creates an ASP.NET Core Razor Pages application. 
- `-o` places it next to the backend rather than inside it. 

Adding the project to the existing solution lets you build both projects together. 

## 2. Choose local addresses

In a dedicated terminal, move to the backend project and run it:

```bash
cd backend/Backend.Api
dotnet run 
```

Take note of the port that the backend is using. You

## 3. Configure the API address

Edit `appsettings.Development.json` in the frontend project. Add a top-level `BackendApi` section. Do not modify the rest of the file:

```json
{
  "BackendApi": {
    "BaseUrl": "http://localhost:xxxx/"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

- `BaseUrl` identifies the backend url and port. 
- The trailing slash allows relative paths such as `api/products` to resolve correctly. 
- Replace the port placeholder (`xxxx`) with the actual port that is used by the backend.

The browser will request Razor Pages from the frontend. Its C# PageModels will call the API on the server, so this design does not require browser CORS configuration. It also does not require a project reference to `Backend.Api` or an EF Core package in the frontend.

## 4. Run the generated frontend

In a second terminal, from the solution folder, move to the project folder and run the application:

```bash
cd frontend/Frontend.Web
dotnet run
```

This starts the frontend. Open `http://localhost:xxxx` (replace the placeholder with the port that you see in the Terminal) and confirm that the generated home page loads. Keep separate terminals for the two applications (backend and frontend). Stop only the frontend with Ctrl+C before continuing with the rest of steps.

## Checkpoint

The solution now contains a backend and a Razor Pages frontend. Neither project starts the other automatically. At this stage the frontend still displays the template pages and makes no product API calls.

[Previous: Step 7 - Run and test with Swagger UI](07-run-and-test-with-swagger.md) | [Next: Step 9 - Add the frontend model and API client](09-frontend-model-and-api-client.md)
