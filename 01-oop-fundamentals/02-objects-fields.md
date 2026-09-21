# 02 - Objects, Fields and State

[Previous: Introduction to classes](01-introduction-to-classes.md) | [Next: Constructors](03-constructors.md)

An object is an instance of a class.

The values stored inside an object are called its state.

## A class with fields

```csharp
class Student
{
    public string Name;
    public int Age;
    public string Career;
}
```

Each object has its own values. This is the code for `Program.cs`:

```csharp
Student student1 = new Student();
student1.Name = "Ana";
student1.Age = 19;
student1.Career = "Computer Science";

Student student2 = new Student();
student2.Name = "Carlos";
student2.Age = 22;
student2.Career = "Cybersecurity";

Console.WriteLine($"{student1.Name} is {student1.Age} years old.");
Console.WriteLine($"{student2.Name} is {student2.Age} years old.");
```

## Objects are independent

Even though both objects come from the same class, they do not share the same values.

This is one of the main ideas of OOP:

- same class
- different objects
- different state

## Example with a list

```csharp
List<Student> students = new List<Student>();

students.Add(student1);
students.Add(student2);

foreach (Student student in students)
{
    Console.WriteLine($"{student.Name} - {student.Career}");
}
```

## Your turn

Create a `Book` class with:

- `Title`
- `Author`
- `Pages`

Create 3 different books and print their information.

### Investigate

Find out:

1. What does it mean for an object to have state?
2. What happens when two objects are created from the same class?
3. What is a collection in C#?

---

[Previous: Introduction to classes](01-introduction-to-classes.md) | [Next: Constructors](03-constructors.md)
