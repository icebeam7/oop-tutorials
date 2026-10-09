# 05. Build the Boss Enemy Subclass (`Dragon.cs`)

We will build an enemy class. This is a strong heavy hitter.

### Use inheritance
Derive from `Character` class:

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public class Dragon : Character
    {
```

### Set high boss parameters in the constructor
Initialize high parameters (200 HP, 20 Attack) to reflect a powerful opponent.

```csharp
        public Dragon(string name) : base(name, 200, 20) { }
```

### Override the attack execution code
Implement a heavy attack pattern that overrides the default implementation, simulating a fire breathing action stage.

```csharp
        public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} breathes devastating fire across the battlefield!");
            target.TakeDamage(AttackPower);
        }
    }
}
```

[Previous: 04 - Mage class](04-mage.md) | [Next: 06 - Goblin class](06-goblin.md)