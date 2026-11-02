# Step 1 - Check the .NET web workload

Before creating the application, we need to confirm that the **ASP.NET Core web workload** is available. If this check fails, the project cannot be created or run correctly.

## Check the installed tools

Launch a Terminal / Command Prompt and execute the following command:

```bash
dotnet new list webapp
```

If the ASP.NET Core template appears in the list, we are ready to create the web app.

## Create the project

Run the following command:

```bash
dotnet new webapp -n AiDemo
```

This is the first and most important validation step because it prevents wasting time debugging a project that was created without the required web templates.

## Open the project

Open Visual Studio Code, then open the `AiDemo` folder.

---

## What we are preparing for

Once the project exists, the next step will add the configuration needed to connect to cloud resources securely, without writing sensitive information directly in source code.

---

[Previous: Overview](README.md) | [Next: Step 2 - Store Foundry secrets safely](02-setup-secrets.md)
