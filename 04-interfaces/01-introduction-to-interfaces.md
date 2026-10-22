# 01 - Introduction to interfaces

In this first step, we will build a small interface example from scratch.

An **interface** is a contract. It defines the members a class must provide, but it does not implement the behavior itself. This is useful because multiple unrelated classes can share the same behavior without needing to inherit from the same base class.

For example, a `Dog` and a `Cat` may both need to `Speak()`, but they do not have to inherit from the same class to follow the same rule.

## Step 0: Create a project

Project name: **InterfacesDemo**

- Create two folders: **Interfaces** and **Classes**

- All interfaces will be created inside the **Interfaces** folder

- All classes will be created inside the **Classes** folder

- Declare the corresponding namespaces for each interface or class. For example:

```csharp
namespace InterfacesDemo.Interfaces;
```

- In each class, include a `using` directive to reference the namespace that contains the interfaces, for example:

```csharp
using InterfacesDemo.Interfaces;
```

## Step 1: Create the interface file

Create a new file called `IAnimal.cs`.

Add this code:

```csharp
public interface IAnimal
{
    void Speak();
}
```

This tells C# that any class implementing `IAnimal` must provide a `Speak()` method.

## Step 2: Create the Dog class

Create a new file called `Dog.cs`.

Add the class below:

```csharp
public class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Woof!");
    }
}
```

## Step 3: Create the Cat class

Create another file called `Cat.cs`.

Add this class:

```csharp
public class Cat : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Meow!");
    }
}
```

## Step 4: Add the program file

In `Program.cs`, add a small example that creates both animals and calls the same method:

```csharp
IAnimal[] animals =
{
    new Dog(),
    new Cat()
};

foreach (var animal in animals)
{
    animal.Speak();
}
```

## What this shows

- the interface defines the contract
- each class implements the same method in its own way
- the same code can work with different classes

This is one of the main reasons interfaces are useful in OOP.

[Previous: README](README.md) | [Next: 02 - Properties and methods in an interface](02-interface-properties-and-methods.md)
