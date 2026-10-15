# 02 - Sealed method in `Warrior`

A `sealed` method prevents a subclass from overriding it. This is useful when the base implementation is final and should not change in child classes.

## Step 1: Seal the `PerformAttack` method inside `Warrior`

```csharp
// ...
    public class Warrior : Character
    {
        // ...
        public sealed override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} swings a heavy greatsword at {target.Name}!");
            target.TakeDamage(AttackPower);
        }
        // ...
    }
```

The `sealed` keyword tells C#:

- `Warrior` can still be inherited from.
- But the method `PerformAttack` cannot be overridden again in derived classes.

## Step 2: Create another class that inherits from `Warrior`

```csharp
public class EliteWarrior : Warrior
{
    public EliteWarrior(string name) : base($"Elite Member {name}") 
    { 
        AttackPower *= 2;
        ShieldBlock *= 2;
    }

    // This would be invalid:
    // public override void PerformAttack(Character target)
    // {
    //     Console.WriteLine("This warrior attacks in a different way!");
    // }
}
```

If we try to override the sealed method, the compiler will throw an error when you use it. Build the app if you don't see the error:

```csharp
// Compiler error:
// 'EliteWarrior.PerformAttack(Character)': cannot override inherited member
// 'Warrior.PerformAttack(Character)' because it is sealed.
```

## Step 3: Use the derived class in the main program

Even though `PerformAttack` is sealed, the subclass can still be used normally because it inherits the behavior:

Modify the code in `Program.cs` to implement the following:

- If the user chooses to be a Warrior, ask them if they are Elite Warrior or not.
- If they choose Elite Warrior, create the corresponding instance and play the game, which should work.

In this example, `EliteWarrior` inherits the already sealed version of `PerformAttack` from `Warrior`.

Sealing methods is useful when you want to protect the logic of a base behavior while still allowing the class to be extended for other purposes, *unless you completely seal the class* (which is our next topic).

[Previous: 01 - Abstract members in Character](01-abstract-members.md) | [Next: 03 - Sealed class in Mage](03-sealed-class.md)
