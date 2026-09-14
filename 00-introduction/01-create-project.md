# 01 - Create and Run a .NET Console Application

[← Previous: Overview](README.md) | [Next: Variables and input →](02-variables-input.md)

Let's create a .NET Console application using the terminal in VS Code.

## 1. Check your .NET installation

Open the terminal in VS Code:

**Terminal → New Terminal**

Run:

```bash
dotnet --version
```

You should see a version number.

## 2. Create the application

Run:

```bash
dotnet new console -n StudentTaskManager
cd StudentTaskManager
code .
```

Your project should contain a file named:

```text
Program.cs
```

Run the application:

```bash
dotnet run
```

You should see:

```text
Hello, World!
```

## 3. Modify the program

Replace the content of `Program.cs` with:

```csharp
Console.WriteLine("Student Task Manager");
Console.WriteLine("====================");
Console.WriteLine("Welcome to the application!");
```

Run it again:

```bash
dotnet run
```

## What just happened?

`Console.WriteLine()` writes text to the terminal.

C# statements usually end with a semicolon:

```csharp
;
```

Unlike languages such as Python, C# is compiled before it runs.

When you execute:

```bash
dotnet run
```

.NET compiles and then executes the application.

## Your turn

Modify the application so it prints:

- Your name
- Your favorite programming language
- One reason you want to learn .NET

### Investigate

Find out:

> What is the difference between `Console.Write()` and `Console.WriteLine()`?

Test both in your program.

---

[← Previous: Overview](README.md) | [Next: Variables and input →](02-variables-input.md)
