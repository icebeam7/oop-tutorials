# 05 - Properties and Encapsulation

[Previous: Methods and behavior](04-methods-and-behavior.md) | [Next: Static members](06-static-members.md)

Properties allow us to control how data is accessed from outside the class.

They are often safer than exposing fields directly.

## A class with public fields

```csharp
class Student
{
    public string Name;
    public int Age;
}
```

This works, but external code (for example in `Program.cs`) can set invalid values:

```csharp
student.Age = -50;
```

## A safer version with properties

```csharp
class Student
{
    private string name;
    private int age;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age
    {
        get { return age; }
        set
        {
            if (value >= 0)
            {
                age = value;
            }
        }
    }
}
```

Now the object validates the data. Add this in `Program.cs`:

```csharp
Student student = new Student();
student.Name = "Ana";
student.Age = 20;

Console.WriteLine($"{student.Name} - {student.Age}");
```

## Encapsulation

Encapsulation means hiding the internal details of the class and only exposing what is necessary.

This helps:

- protect data
- reduce errors
- make the code easier to maintain

## Your turn

Create a `Person` class with:

- `Name`
- `Age`

Use private fields and public properties.

Validate that age cannot be negative.

### Investigate

Find out:

1. What is the difference between a field and a property?
2. Why is encapsulation useful?
3. What does `private` mean?

---

[Previous: Methods and behavior](04-methods-and-behavior.md) | [Next: Static members](06-static-members.md)
