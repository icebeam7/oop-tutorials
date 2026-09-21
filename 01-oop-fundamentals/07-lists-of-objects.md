# 07 - Lists of Objects

[Previous: Static members](06-static-members.md) | [Next: Challenges](08-challenges.md)

In real applications, we usually work with more than one object.

To manage many students, we can use a list.

## Example

```csharp
class Student
{
    public string Name;
    public int Age;

    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

Add this code in `Program.cs`:

```csharp
List<Student> students = new List<Student>();

students.Add(new Student("Ana", 20));
students.Add(new Student("Luis", 22));
students.Add(new Student("Sara", 19));

foreach (Student student in students)
{
    Console.WriteLine($"{student.Name} - {student.Age}");
}
```

The list stores many objects of the same type.

This is very common in C# programs.

## More useful example

Modify the code in `Program.cs`:

```csharp
List<Student> students = new List<Student>();

students.Add(new Student("Ana", 20));
students.Add(new Student("Luis", 22));

foreach (Student student in students)
{
    if (student.Age >= 21)
    {
        Console.WriteLine($"Adult student: {student.Name}");
    }
}
```

## Your turn

Create a `Course` class and a list of courses.

Add 3 courses and print all of them.

### Investigate

Find out:

1. What is a `List<T>`?
2. Why is it useful to store objects in a list?
3. What is a `foreach` loop?

---

[Previous: Static members](06-static-members.md) | [Next: Challenges](08-challenges.md)
