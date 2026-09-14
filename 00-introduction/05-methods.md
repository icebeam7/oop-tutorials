# 05 - Methods and Refactoring

[← Previous: Loops and lists](04-loops-lists.md) | [Next: Enums and integration →](06-enums-integration.md)

We can break a growing program into smaller reusable pieces.

At this point, `Program.cs` is probably getting long.

Methods let us group related logic.

## A simple method

Add this below your main program code:

```csharp
void ShowTitle()
{
    Console.WriteLine();
    Console.WriteLine("Student Task Manager");
    Console.WriteLine("====================");
}
```

Call it:

```csharp
ShowTitle();
```

## Method with a parameter

```csharp
void ShowMessage(string message)
{
    Console.WriteLine($"[INFO] {message}");
}
```

Call it:

```csharp
ShowMessage("Application started.");
ShowMessage("Task added.");
```

## Method with a return value

```csharp
int GetTaskCount(List<string> tasks)
{
    return tasks.Count;
}
```

Use it:

```csharp
int count = GetTaskCount(tasks);
Console.WriteLine($"You currently have {count} tasks.");
```

## Refactor the task list

Create:

```csharp
void ShowTasks(List<string> tasks)
{
    if (tasks.Count == 0)
    {
        Console.WriteLine("No tasks available.");
        return;
    }

    for (int i = 0; i < tasks.Count; i++)
    {
        Console.WriteLine($"{i + 1}. {tasks[i]}");
    }
}
```

Then your `switch` becomes simpler:

```csharp
case 2:
    ShowTasks(tasks);
    break;
```

## Your turn

Create these methods yourself:

```text
AddTask(...)
RemoveTask(...)
ShowMenu(...)
```

Do not copy all logic into the `switch`.

Your goal is for the main loop to look approximately like this:

```csharp
while (running)
{
    ShowMenu();

    // read option

    switch (option)
    {
        case 1:
            AddTask(tasks);
            break;

        case 2:
            ShowTasks(tasks);
            break;

        case 3:
            RemoveTask(tasks);
            break;

        // ...
    }
}
```

You must determine:

- What parameters each method needs.
- Whether each method should return something.
- Whether the method should use `void`.

### Investigate

Find out:

1. What is **variable scope**?
2. Can a variable declared inside one method be accessed directly from another method?

---

[← Previous: Loops and lists](04-loops-lists.md) | [Next: Enums and integration →](06-enums-integration.md)
