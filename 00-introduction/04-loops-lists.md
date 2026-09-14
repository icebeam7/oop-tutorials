# 04 - Loops and Lists

[← Previous: Conditions and switch](03-conditions-switch.md) | [Next: Methods →](05-methods.md)

Now we can store multiple tasks and repeat operations using lists and loops.

## Lists

A `List<T>` stores multiple values of the same type.

Create a list:

```csharp
List<string> tasks = new List<string>();
```

Add some tasks:

```csharp
tasks.Add("Review C# variables");
tasks.Add("Practice loops");
tasks.Add("Create a Console application");
```

Print the number of tasks:

```csharp
Console.WriteLine($"Number of tasks: {tasks.Count}");
```

## `foreach`

Use `foreach` when you want to process every item:

```csharp
foreach (string task in tasks)
{
    Console.WriteLine(task);
}
```

## `for`

A `for` loop gives you access to the index:

```csharp
for (int i = 0; i < tasks.Count; i++)
{
    Console.WriteLine($"{i + 1}. {tasks[i]}");
}
```

## Adding user input to the list

```csharp
Console.Write("Enter a new task: ");
string newTask = Console.ReadLine();

tasks.Add(newTask);

Console.WriteLine("Task added.");
```

## Repeating the menu with `while`

Now create an application loop:

```csharp
bool running = true;

while (running)
{
    Console.WriteLine();
    Console.WriteLine("1. Add task");
    Console.WriteLine("2. View tasks");
    Console.WriteLine("0. Exit");

    Console.Write("Choose: ");
    int option = int.Parse(Console.ReadLine());

    switch (option)
    {
        case 1:
            Console.Write("Enter task: ");
            string task = Console.ReadLine();
            tasks.Add(task);
            break;

        case 2:
            for (int i = 0; i < tasks.Count; i++)
            {
                Console.WriteLine($"{i + 1}. {tasks[i]}");
            }
            break;

        case 0:
            running = false;
            break;

        default:
            Console.WriteLine("Invalid option.");
            break;
    }
}
```

Now your application keeps running until the user chooses `0`.

## Your turn

Add a menu option:

```text
3. Remove task
```

The user should enter the number of the task to remove.

Example:

```text
1. Learn variables
2. Practice loops
3. Review lists

Task to remove: 2
```

Afterward:

```text
1. Learn variables
2. Review lists
```

### Investigate

You will need to discover at least one of these:

- `List<T>.Remove()`
- `List<T>.RemoveAt()`

Determine which one is more appropriate when the user selects a task by number.

Also investigate:

> Why might you need to subtract `1` from the number entered by the user?

---

[← Previous: Conditions and switch](03-conditions-switch.md) | [Next: Methods →](05-methods.md)
