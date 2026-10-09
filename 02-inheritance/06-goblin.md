# 05. Build another Enemy Subclass (`Goblin.cs`)

We will design a basic monster type that utilizes randomized behavior. 

### Use inheritance
Derive from `Character` class:

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public class Goblin : Character
    {
```

### Set base stat parameters in the constructor
Initialize small health and attack parameters (60 HP, 10 Attack) to reflect a weak opponent.

```csharp
        public Goblin(string name) : base(name, 60, 10) { }
```

### Override the default attack with randomized triggers
Code an attack sequence that executes a primary slash, then uses `Random.Shared.Next` to run a percentage check. If triggered, it processes an immediate secondary bonus damage strike.

```csharp
        public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} slashes with a rusty dagger!");
            target.TakeDamage(AttackPower);
            
            if (Random.Shared.Next(0, 100) < 30)
            {
                Console.WriteLine($"{Name} strikes a second time out of nowhere!");
                target.TakeDamage(AttackPower / 2);
            }
        }
    }
}
```

[Previous: 05 - Dragon class](05-dragon.md) | [Next: 07 - Main gameplay](07-gameplay.md)