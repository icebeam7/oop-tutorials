# 03 - Conditions: if, else and switch

[← Previous: Variables and input](02-variables-input.md) | [Next: Loops and lists →](04-loops-lists.md)

Let's learn how to make decisions based on the data entered by the user.

## `if` and `else`

Add this code at the end of your program:

```csharp
Console.Write("How many tasks have you completed today? ");
int completedTasks = int.Parse(Console.ReadLine());

if (completedTasks == 0)
{
    Console.WriteLine("Time to get started!");
}
else if (completedTasks < 3)
{
    Console.WriteLine("Good start. Keep going!");
}
else
{
    Console.WriteLine("Great progress!");
}
```

## Comparison operators

```text
==   equal
!=   not equal
>    greater than
<    less than
>=   greater than or equal
<=   less than or equal
```

## Logical operators

```text
&&   AND
||   OR
!    NOT
```

Example:

```csharp
int tasks = 4;
bool hasTime = true;

if (tasks >= 3 && hasTime)
{
    Console.WriteLine("You can probably finish your daily goal.");
}
```

## `switch`

Ask the user to choose an option.

```csharp
Console.WriteLine();
Console.WriteLine("1. Add task");
Console.WriteLine("2. View tasks");
Console.WriteLine("3. Exit");

Console.Write("Choose an option: ");
int option = int.Parse(Console.ReadLine());

switch (option)
{
    case 1:
        Console.WriteLine("Adding a task...");
        break;

    case 2:
        Console.WriteLine("Showing tasks...");
        break;

    case 3:
        Console.WriteLine("Goodbye!");
        break;

    default:
        Console.WriteLine("Invalid option.");
        break;
}
```

## Your turn

Extend the menu:

```text
1. Add task
2. View tasks
3. Complete task
4. Remove task
0. Exit
```

Write the `switch` cases.

For now, each case only needs to print a message.

---

[← Previous: Variables and input](02-variables-input.md) | [Next: Loops and lists →](04-loops-lists.md)
