# 03 - Implementing interfaces to demonstrate polymorphism

Now we will build a second class that implements the same interface, so we can see polymorphism in action.

**Polymorphism** means different objects can be treated through the same type. In C#, an interface is perfect for this. We can create several classes that all implement `ICombatant`, and then work with them as `ICombatant` objects even though each one behaves differently.

This is one of the main advantages of interfaces: one common contract, many different implementations.

## Step 1: Create the Mage class

Create a new file called `Mage.cs`.

Add this class:

```csharp
public class Mage : ICombatant
{
    public string Name { get; set; }
    public int Health { get; set; }
    public int AttackPower { get; set; }

    public void Attack()
    {
        Console.WriteLine($"{Name} casts a fireball for {AttackPower} damage.");
    }

    public void Defend()
    {
        Console.WriteLine($"{Name} creates a magical barrier.");
    }

    public void TakeDamage(int damage)
    {
        Health -= damage;
        Console.WriteLine($"{Name} takes {damage} damage.");
    }
}
```

## Step 2: Create the program logic

Open `Program.cs` and add this array:

```csharp
ICombatant[] fighters =
{
    new Warrior { Name = "Aragorn", Health = 100, AttackPower = 25 },
    new Mage { Name = "Merlin", Health = 80, AttackPower = 40 }
};
```

Then add this loop:

```csharp
foreach (var fighter in fighters)
{
    fighter.Attack();
    fighter.Defend();
}
```

## What this demonstrates

Even though the objects are different, both are stored as `ICombatant`.

This is the heart of **polymorphism**:

- one interface
- multiple classes
- same method calls, different behavior

[Previous: 02 - Properties and methods in an interface](02-interface-properties-and-methods.md) | [Next: 04 - Multiple interfaces and inheritance](04-multiple-interfaces-and-inheritance.md)
