# 03 - Constructors

[Previous: Objects, fields and state](02-objects-fields.md) | [Next: Methods and behavior](04-methods-and-behavior.md)

A constructor is a special method used to initialize an object when it is created.

It runs automatically when we use `new`.

## A class with a constructor

```csharp
class Student
{
    public string Name;
    public int Age;
    public string Career;

    public Student(string name, int age, string career)
    {
        Name = name;
        Age = age;
        Career = career;
    }
}
```

Now we can create objects like this in `Program.cs`:

```csharp
Student student = new Student("Ana", 20, "Computer Science");
Console.WriteLine($"{student.Name} - {student.Career}");
```

## Why constructors are useful

Without a constructor, we would need to assign every field manually.

Constructors make the object creation cleaner and easier to understand.

## A default constructor

This constructor takes no parameters:

```csharp
class Student
{
    public string Name;
    public int Age;

    public Student()
    {
        Name = "Unknown";
        Age = 0;
    }
}
```

We have assigned default values to the fields. Use it like this in `Program.cs`:

```csharp
Student student = new Student();
Console.WriteLine(student.Name);
```

## Your turn

Create a `Product` class with:

- `Name`
- `Price`
- `Stock`

Include a constructor that receives these values.

Then create two products and print their values.

### Investigate

Find out:

1. Why is a constructor called automatically?
2. What is the difference between a normal method and a constructor?
3. Can a class have more than one constructor?

---

[Previous: Objects, fields and state](02-objects-fields.md) | [Next: Methods and behavior](04-methods-and-behavior.md)
