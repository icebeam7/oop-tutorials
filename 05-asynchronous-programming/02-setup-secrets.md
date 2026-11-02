# Step 2 - Store Foundry secrets safely

In order to generate an image, we need to access a model that is hosted on Azure (Microsoft's cloud). Following information is required:

* An endpoint (a specific URL destination on a server that accepts HTTP requests)
* The model deployment name (to generate an image)
* An API key (to authenticate HTTP requests)

Above elements are confidential and should not live in source code. **Secret Manager** lets us keep these values locally on the developer's machine while the app reads them at runtime.

## Initialize user secrets

In Visual Studio Code, open a terminal and run the following command:

```bash
dotnet user-secrets init
```

## Identify the confidential information

In Moodle, you will find the values for 3 elements: 

* Endpoint
* Image generation model name
* Api Key

Identify the data, you will use it to replace placeholders in the next section.

## Add image-generation secrets

Let's start with the endpoint. Replace the placeholder in the following command with the **Endpoint** value from the previous section. Then, run the command:

```bash
dotnet user-secrets set "Foundry:Endpoint" "your-endpoint"
```

Next, use the **Image generation model name** value from the previous section and use it to replace the placeholder in the next command. Then, run the command:

```bash
dotnet user-secrets set "Foundry:ImageModel" "your-image-model-name"
```

Finally, get the **Api Key** value from the previous section and use it to replace the placeholder in the next command. Then, run the command:

```bash
dotnet user-secrets set "Foundry:ApiKey" "your-key"
```

The important idea here is that these values stay outside the Git repository and are loaded from configuration at runtime. This is the safest and simplest approach for local development.

The app will use the values from configuration later, so we can keep the implementation clean and avoid accidentally sharing secrets in the code.

## Register a dependency for HTTP Requests:

Add this code in `Program.cs` below `builder.Services.AddRazorPages();`:

builder.Services.AddHttpClient();

---

[Previous: Step 1 - Check the .NET web workload](01-check-web-workload.md) | 
[Next: Step 3 - Create request/response models and the web page](03-models-and-ui.md)
