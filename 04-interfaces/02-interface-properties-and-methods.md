# 02 - Properties and methods in an interface

Now we will create an interface that includes both properties and methods.

An interface can define more than just methods. It can also declare **properties** that any implementing class must provide. This gives us a shared structure for different classes while still allowing each class to define its own behavior.

In other words, the interface says: "Any class that implements me must have these properties and methods." The class then decides how each one works.

## Step 1: Create the interface file

Create a file called `ICombatant.cs`.

Add the following interface:

```csharp
public interface ICombatant
{
    string Name { get; set; }
    int Health { get; set; }
    int AttackPower { get; set; }

    void Attack();
    void Defend();
    void TakeDamage(int damage);
}
```

This interface includes:

- 3 properties: `Name`, `Health`, `AttackPower`
- 3 methods: `Attack()`, `Defend()`, `TakeDamage(int damage)`

## Step 2: Create the Warrior class

Create a new file called `Warrior.cs`.

Add this class and implement all required members:

```csharp
public class Warrior : ICombatant
{
    public string Name { get; set; }
    public int Health { get; set; }
    public int AttackPower { get; set; }

    public void Attack()
    {
        Console.WriteLine($"{Name} attacks with a sword for {AttackPower} damage.");
    }

    public void Defend()
    {
        Console.WriteLine($"{Name} raises a shield to defend.");
    }

    public void TakeDamage(int damage)
    {
        Health -= damage;
        Console.WriteLine($"{Name} takes {damage} damage. Current health: {Health}");
    }
}
```

## Step 3: Add a quick test in Program.cs

Update `Program.cs`.

Add this code:

```csharp
Warrior warrior = new Warrior
{
    Name = "Aragorn",
    Health = 100,
    AttackPower = 25
};

warrior.Attack();
warrior.Defend();
warrior.TakeDamage(10);
```

## Important idea

Any class that implements `ICombatant` **must** provide all the properties and methods declared in the interface.

This is the contract: the class agrees to the interface definition.

[Previous: 01 - Introduction to interfaces](01-introduction-to-interfaces.md) | [Next: 03 - Implementing interfaces to demonstrate polymorphism](03-polymorphism-with-interfaces.md)
