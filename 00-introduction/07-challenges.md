# 07 - Independent Challenges

[← Previous: Enums and integration](06-enums-integration.md) | [Back to tutorial overview](README.md)

Choose challenges based on your experience level.

---

## 1 - Input validation

Currently, this can crash:

```csharp
int option = int.Parse(Console.ReadLine());
```

Modify the application so invalid input does not crash it.

Example:

```text
Choose: hello
Invalid option. Please enter a number.
```

Hint:

```text
int.TryParse
```

---

## 2 - Prevent empty tasks

The application should reject:

```text
""
```

or input containing only spaces.

Investigate:

```text
string.IsNullOrWhiteSpace(...)
```

---

## 3 - Search tasks

Add:

```text
4. Search tasks
```

Example:

```text
Search: C#

Results:
1. Review C# variables
2. Practice C# methods
```

---

## 4 - LINQ

If you already know C#, investigate LINQ.

Try to discover how these work:

```csharp
Where(...)
Count(...)
Any(...)
OrderBy(...)
```

Possible challenge:

> Display only tasks containing the word `"C#"`.

---

## 5 - Persist the tasks

Currently all tasks disappear when the program exits.

Investigate how .NET can:

- Write text to a file.
- Read text from a file.
- Store structured information using JSON.

Useful concepts to research:

```text
File.WriteAllLines
File.ReadAllLines
System.Text.Json
```

---

[← Previous: Enums and integration](06-enums-integration.md) 
