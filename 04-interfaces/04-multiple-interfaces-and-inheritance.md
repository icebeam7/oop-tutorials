# 04 - Multiple interfaces and interface inheritance

Now we will create a more realistic design using interface inheritance and multiple interfaces.

Interfaces can also be organized in a hierarchy. One interface can inherit from another, which means the child interface includes the parent interface members. A class can also implement multiple interfaces at the same time, allowing it to participate in different contracts.

This is useful when a class needs to combine responsibilities such as "combat" and "healing" without forcing it to inherit from multiple classes.

## Step 1: Create the base interface

Create a file called `ICharacter.cs`.

Add this interface:

```csharp
public interface ICharacter
{
    string Name { get; set; }
    int Health { get; set; }

    void TakeDamage(int damage);
}
```

## Step 2: Create the child interface

Modify the `ICombatant` interface so it inherits from `ICharacter` and adjust its code:

```csharp
public interface ICombatant : ICharacter
{
    int AttackPower { get; set; }

    void Attack();
    void Defend();
}
```

This means `ICombatant` includes all the members from `ICharacter` plus its own ones.

## Step 3: Create the healer interface

Create a file called `IHealer.cs`.

Add this interface:

```csharp
public interface IHealer
{
    void Heal(int hp);
}
```

## Step 4: Modify the Mage class

Modify `Mage.cs` so it implements two interfaces now:

```csharp
public class Mage : ICombatant, IHealer
{
    // ...
}
```

Because the class now has two contracts, it also **has to** implement the rules defined in `IHealer`, so you need to add the `Heal` method in the class:

```csharp
public class Mage : ICombatant, IHealer
{
    // ...

    public void Heal(int hp)
    {
        Health += hp;
        Console.WriteLine($"{Name} recovered health.");
    }
}
```

## Step 5: Add usage in Program.cs

Modify `Program.cs`:

```csharp
// ...
Mage mage = fighters[1] as Mage;
mage.Heal(30);
```

## Why this matters

A class can:

- implement multiple interfaces
- inherit from an interface
- keep all behavior organized and reusable

This is a common pattern in larger applications.

[Previous: 03 - Polymorphism with interfaces](03-polymorphism-with-interfaces.md) | [Next: 05 - Explicit interface members](05-explicit-interface-members.md)
