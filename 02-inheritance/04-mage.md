# 04. Implement the Hero Class (`Mage.cs`)

We will build another hero subclass.

### Use inheritance and define unique resources
Use the colon operator to state that the `Mage` class derives directly from the `Character` class. Moreover, the `Mage` class includes a unique Mana property for standard magic usage.

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public class Mage : Character
    {
        public int Mana { get; private set; } = 50;
```

### Add the constructor that invokes the parent's constructor
Create a constructor that calls the constructor defined in `Character` class passing initial values (80 HP, 25 Attack) using the `: base(...)` keyword.

```csharp
        public Mage(string name) : base(name, 80, 25) { }
```

### Implement the specific attack logic
Implement the attack method using the `override` keyword to describe exactly how the Mage uses magic for a standard offensive action phase.

```csharp
        public override void PerformAttack(Character target)
        {
            if (Mana >= 20)
            {
                Mana -= 20;
                Console.WriteLine($"{Name} casts a massive Fireball at {target.Name}! (Mana left: {Mana})");
                target.TakeDamage(AttackPower);
            }
            else
            {
                Console.WriteLine($"{Name} is out of mana! They whack {target.Name} weakly with a staff.");
                target.TakeDamage(5); 
            }
        }
    }
}
```

[Previous: 03 - Warrior class](03-warrior.md) | [Next: 05 - Dragon class](05-dragon.md)