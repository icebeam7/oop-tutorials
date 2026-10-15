# 02 - Create the Base Class (`Character.cs`)

We will create the core class that defines what every participant in the game can do. 

### First, define properties and state trackers
We need variables to hold tracking metrics for health (current and max), attack power, and a life check property flag. 

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public class Character
    {
        public string Name { get; set; }
        public int Health { get; set; }
        public int MaxHealth { get; set; }
        public int AttackPower { get; set; }

        public bool IsAlive => Health > 0;

```

### Add the base class constructor
We define an initialization method with a constructor:

```csharp
        protected Character(string name, int health, int attackPower)
        {
            Name = name;
            MaxHealth = health;
            Health = health;
            AttackPower = attackPower;
        }
```

### Add the virtual damage management method
We will implement standard health subtraction arithmetic. By using the `virtual` keyword, we allow subclasses to modify how damage calculation works if they have custom defense behavior.

```csharp
        public virtual void TakeDamage(int damage)
        {
            Health -= damage;
            
            if (Health < 0)
                Health = 0;
            
            Console.WriteLine($"{Name} takes {damage} damage! (HP: {Health}/{MaxHealth})");
        }
```

### Define the attack management method
We set another method for attacking that will be modified by subclasses.

```csharp
        public virtual void PerformAttack(Character target) { }
    }
}
```

[Previous: Project Setup](01-project-setup.md) | [Next: 03 - Warrior class](03-warrior.md)