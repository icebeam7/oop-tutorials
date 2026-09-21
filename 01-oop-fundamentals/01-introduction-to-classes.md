# 01 - Introduction to Classes

[Previous: Overview](README.md) | [Next: Objects, fields and state](02-objects-fields.md)

A class is a blueprint for creating objects.

Think of a class like a recipe or a template:

- A class defines what an object has.
- A class defines what an object can do.

For example, a `Student` class can represent a student with a name, age, and career.

## A simple class

Create a new console project `StudentApplication`.

Add a new file: `Student.cs`

Add this code to `Student.cs`:

```csharp
class Student
{
    public string Name;
    public int Age;
    public string Career;
}
```

Add this code to `Program.cs`:

```csharp
Student student1 = new Student();
student1.Name = "Ana";
student1.Age = 20;
student1.Career = "Computer Science";

Console.WriteLine($"Student: {student1.Name}");
Console.WriteLine($"Age: {student1.Age}");
Console.WriteLine($"Career: {student1.Career}");
```

## What is happening here?

`Student` is the class.

`student1` is an object created from that class.

The class defines the properties:

```csharp
public string Name;
public int Age;
public string Career;
```

Then we assign values to those fields:

```csharp
student1.Name = "Ana";
```

This means the object has its own internal state.

## Why use classes?

Classes help us organize code in a better way.

Instead of having many unrelated variables, we can group them in a single object.

Example:

```csharp
Student student2 = new Student();
student2.Name = "Luis";
student2.Age = 22;
student2.Career = "Software Engineering";
```

Now we can work with multiple students more easily.

## Your turn

Create a class named `Course` with:

- `Name`
- `Credits`
- `Teacher`

Then create an object for a course and print the information.

### Investigate

Find out:

1. What is the difference between a `class` and an `object`?
2. Why do we use the `new` keyword?
3. What is a field in C#?

---

[Previous: Overview](README.md) | [Next: Objects, fields and state](02-objects-fields.md)
