# 06 - Static Members

[Previous: Properties and encapsulation](05-properties-encapsulation.md) | [Next: Lists of objects](07-lists-of-objects.md)

Sometimes a value should be shared by all objects of a class.

That is where `static` comes in.

## A static field

```csharp
class Student
{
    public string Name;
    public static int TotalStudents = 0;

    public Student(string name)
    {
        Name = name;
        TotalStudents++;
    }
}
```

Use it in `Program.cs`:

```csharp
Student student1 = new Student("Ana");
Student student2 = new Student("Luis");

Console.WriteLine(Student.TotalStudents);
```

The value is shared by every instance of the class.

## A static method

```csharp
class Student
{
    public static void ShowMessage()
    {
        Console.WriteLine("Welcome to the student system!");
    }
}
```

Use it in `Program.cs`:

```csharp
Student.ShowMessage();
```

Notice that static members are called through the class, not through an object.

## Your turn

Create a `Course` class with:

- `Name`
- `static int TotalCourses`

Every time a new course is created, increase the count.

Then print the total number of courses created.

### Investigate

Find out:

1. What does the `static` keyword do?
2. Can a static method access instance fields?
3. Why would you use static data for a counter?

---

[Previous: Properties and encapsulation](05-properties-encapsulation.md) | [Next: Lists of objects](07-lists-of-objects.md)
