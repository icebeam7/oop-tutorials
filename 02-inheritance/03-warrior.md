# 03. Implement the a Hero Class (`Warrior.cs`)

We will create our first playable hero subclass using inheritance. A Warrior relies on its defense to stay alive against their opponents.

### First, declare the subclass inheritance connection
Use the colon operator to state that the `Warrior` class derives directly from the `Character` class. Moreover, the `Warrior` class includes a unique shield property to reduce the damage.

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public class Warrior : Character
    {
        public int ShieldBlock { get; set; } = 4;
```

### Add the constructor that invokes the parent's constructor
Create a constructor that calls the constructor defined in `Character` class passing initial values (120 HP, 15 Attack) using the `: base(...)` keyword.

```csharp
        public Warrior(string name) : base(name, 120, 15) { }
```

### Implement the specific attack logic
Implement the attack method using the `override` keyword to describe exactly how the Warrior uses a sword during a standard offensive action phase.

```csharp
        public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} swings a heavy greatsword at {target.Name}!");
            target.TakeDamage(AttackPower);
        }
```

### Implement the damage reception logic
Implement the take damage method using an `override`. Apply numerical subtraction checking to reduce damage via `ShieldBlock` before passing the final value to `base.TakeDamage()`.

```csharp
        public override void TakeDamage(int damage)
        {
            int finalDamage = Math.Max(1, damage - ShieldBlock);
            Console.WriteLine($"{Name} blocks {ShieldBlock} damage with their shield!");
            base.TakeDamage(finalDamage);
        }
    }
}
```

[Previous: 02 - Character class](02-character.md) | [Next: 04 - Mage class](04-mage.md)