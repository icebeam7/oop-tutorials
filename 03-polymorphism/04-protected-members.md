# 04 - Protected members

The `protected` modifier is used when we want a member to be accessible only by the class itself and by classes that inherit from it.

## Step 1: Add a protected member to `Character`

Let's add a `BattleCry` protected property with a default value. 

```csharp
public abstract class Character
{
    // ...

    protected string BattleCry { get; set; } = "Charge!";

    // ...
}
```

This allows derived classes to use `BattleCry`, but code outside the inheritance hierarchy cannot access it directly.

## Step 2: Use the protected member in a subclass

In `Warrior` class, replace the default value of `BattleCry` and display it when the Warrior attacks.

```csharp
public class Warrior : Character
{
    // ... 
    public Warrior(string name) : base(name, 120, 15)
    {
        BattleCry = "For the kingdom!";
    }

    public sealed override void PerformAttack(Character target)
    {
        Console.WriteLine($"{Name} shouts '{BattleCry}' and swings a heavy greatsword at {target.Name}!");
        target.TakeDamage(AttackPower);
    }
}
```

The class `Warrior` can access `BattleCry` because it is a derived class. However, code outside the class cannot do this:

```csharp
// This is not allowed in Program.cs:
// player.BattleCry = "Attaaaaack!";
```

Run the program to demonstrate above functionality.

## Step 3. Use the protected member in the other subclasses

Repeat the previous step for your other classes: `Mage`, `Dragon`, and `Goblin`. Then, play the game and observe how the protected `BattleCry` value is displayed.

[Previous: 03 - Sealed class in Mage](03-sealed-class.md) | [Next: 05 - Type checks with is/as](05-type-checks-is-as.md)