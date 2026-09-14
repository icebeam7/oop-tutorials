# 02 - Variables, Data Types, Input and Constants

[← Previous: Create the project](01-create-project.md) | [Next: Conditions and switch →](03-conditions-switch.md)

We are going to store data in variables and read information from the keyboard.

Replace your current code with:

```csharp
Console.WriteLine("Student Task Manager");
Console.WriteLine("====================");

Console.Write("Enter your name: ");
string name = Console.ReadLine();

Console.Write("How many tasks do you want to complete today? ");
int taskGoal = int.Parse(Console.ReadLine());

Console.Write("How many hours do you have available? ");
double availableHours = double.Parse(Console.ReadLine());

Console.WriteLine();
Console.WriteLine($"Hello {name}!");
Console.WriteLine($"Your goal is {taskGoal} tasks.");
Console.WriteLine($"You have {availableHours} hours available.");
```

## Common C# data types

```csharp
string studentName = "Alex";
int taskCount = 5;
double studyHours = 2.5;
decimal budget = 100.50m;
bool isCompleted = false;
char grade = 'A';
```

Notice:

```csharp
"Hello"
```

is a `string`, while:

```csharp
'A'
```

is a `char`.

## Type inference with `var`

C# can infer the type if you assign a value to a variable:

```csharp
var courseName = ".NET Development";
var maxTasks = 10;
```

The variables still have fixed types. `var` does **not** mean that the variable can later become another type.

## Constants

Values that should never change can be declared using `const`.

Add at the beginning:

```csharp
const int MaxDailyTasks = 10;
const string AppName = "Student Task Manager";

Console.WriteLine($"Application: {AppName}");
Console.WriteLine($"Maximum recommended daily tasks: {MaxDailyTasks}");
```

## Safer numeric input

This code can crash:

```csharp
int number = int.Parse(Console.ReadLine());
```

A safer alternative is:

```csharp
Console.Write("Enter a number: ");
bool valid = int.TryParse(Console.ReadLine(), out int number);

Console.WriteLine($"Valid number: {valid}");
Console.WriteLine($"Number: {number}");
```

## Your turn

Add variables for:

- Student age
- Course name
- If the student already knows C# or not
- Maximum tasks allowed per day as a constant

Print all values using string interpolation.

### Investigate

Find out:

1. What happens if the user enters `"hello"` when using `int.Parse()`?
2. How does `int.TryParse()` prevent the program from crashing?
3. What is the difference between `double` and `decimal`?

---

[← Previous: Create the project](01-create-project.md) | [Next: Conditions and switch →](03-conditions-switch.md)
