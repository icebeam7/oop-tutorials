# 06 - Enums, Constants and Final Integration

[← Previous: Methods](05-methods.md) | [Next: Final challenges →](07-challenges.md)

We can replace unclear numeric or text values with meaningful C# types.

Suppose every task has a priority:

```text
Low
Medium
High
```

We could represent priority using a string:

```csharp
string priority = "High";
```

But strings allow invalid values:

```csharp
string priority = "VeryImportant";
```

An `enum` restricts the possible values.

## Create an enum

Place this at the bottom of `Program.cs`:

```csharp
enum TaskPriority
{
    Low,
    Medium,
    High
}
```

Now create a variable:

```csharp
TaskPriority priority = TaskPriority.High;

Console.WriteLine(priority);
```

## Use an enum with switch

```csharp
switch (priority)
{
    case TaskPriority.Low:
        Console.WriteLine("This task can wait.");
        break;

    case TaskPriority.Medium:
        Console.WriteLine("Try to finish this task today.");
        break;

    case TaskPriority.High:
        Console.WriteLine("Prioritize this task.");
        break;
}
```

## Constants

Add:

```csharp
const int MaxTasks = 10;
```

Before adding a task:

```csharp
if (tasks.Count >= MaxTasks)
{
    Console.WriteLine("Maximum number of tasks reached.");
}
else
{
    // add task
}
```

## Final target

Your application should now contain:

```text
Student Task Manager
====================

1. Add task
2. View tasks
3. Remove task
4. Show task count
0. Exit
```

## Your turn

Add a new feature:

```text
4. Show task count
```

Example:

```text
You currently have 3 tasks.
Maximum allowed: 10
Remaining capacity: 7
```

---

[← Previous: Methods](05-methods.md) | [Next: Final challenges →](07-challenges.md)
