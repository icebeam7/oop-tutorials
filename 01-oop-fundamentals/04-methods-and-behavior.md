# 04 - Methods and Behavior

[Previous: Constructors](03-constructors.md) | [Next: Properties and encapsulation](05-properties-encapsulation.md)

Classes are not only about data. They also contain behavior.

Behavior is represented by methods.

## A class with methods

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

    public void PrintSummary()
    {
        Console.WriteLine($"Student: {Name} - Age: {Age}");
    }
}
```

Use it in `Program.cs`:

```csharp
Student student = new Student("Ana", 20);
student.PrintSummary();
```

## Methods can also return values

Add this method in `Student.cs`:

```csharp
public int GetYearsToGraduate()
{
    return 4;
}
```

Use it in `Program.cs`:

```csharp
int years = student.GetYearsToGraduate();
Console.WriteLine($"Years to graduate: {years}");
```

## Methods with parameters

Add this method in `Student.cs`

```csharp
public void CelebrateBirthday(int years)
{
    Age += years;
    Console.WriteLine($"Happy birthday {Name}! You are now {Age}.");
}
```

Use it in `Program.cs`:

```csharp
student.CelebrateBirthday(1);
```

## Why methods matter

Methods allow us to keep logic inside the class.

This makes code easier to understand and reuse.

## Your turn

Create a `BankAccount` class with:

- `Owner`
- `Balance`

Add methods:

- `Deposit(double amount)`
- `Withdraw(double amount)`
- `ShowBalance()`

Try creating an account and making a few deposits/withdrawals.

### Investigate

Find out:

1. What is the difference between a field and a method?
2. What is a method with a return value?
3. Why is it useful to keep behavior inside a class?

---

[Previous: Constructors](03-constructors.md) | [Next: Properties and encapsulation](05-properties-encapsulation.md)
