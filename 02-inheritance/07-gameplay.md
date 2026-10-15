# 07. Main Gameplay (`Program.cs`)

We will tie everything together in our main program using polymorphism by storing various distinct subclasses within a unified `Character` variable type or `Collection`.

* Request input from the user to dynamically instantiate either a Warrior or Mage.
* Load instances of `Goblin` and `Dragon` into a `Queue`.
* Write nested while loops to maintain active combat rounds until victory or defeat.

### Setup Workspace and Capture Player Selection
We will gather player preferences from the console line input.
 
* Prompt the user for a custom hero name and provide a default value if left blank.
* Display a choice menu listing your available hero classes.

```csharp
using System;
using System.Collections.Generic;
using InheritanceMinigame.Classes;

namespace InheritanceMinigame
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("WELCOME TO THE INHERITANCE ARENA");
            Console.Write("Enter your hero's name: ");
            string heroName = Console.ReadLine();
            if (string.IsNullOrWhiteSpace(heroName))
                heroName = "Unknown Hero";

            Console.WriteLine("\nChoose your class:");
            Console.WriteLine("1. Warrior (High Health, Blocks Damage)");
            Console.WriteLine("2. Mage (Fragile, Deals Massive Fireball Damage)");
            Console.Write("Selection (1 or 2): ");
            string choice = Console.ReadLine();
```

---

### Use Polymorphism to Instantiate Player
We will initialize the player variable. By declaring the variable type as the base class `Character`, it can dynamically hold either a `Warrior` or a `Mage` object.

* Declare a `Character player;` reference.
* Evaluate the user choice string to assign the correct subclass object tracking.

```csharp
            Character player;

            if (choice == "2")
                player = new Mage(heroName);
            else
                player = new Warrior(heroName);
```

---

### Queue Up Combat Opponents
We will now pool different monsters together. Because both `Goblin` and `Dragon` inherit from `Character`, they can live together inside a single collection.

* Create a generic `Queue<Character> dungeonEnemies`.
* Enqueue two `Goblin` instances and one final `Dragon` boss object.

```csharp
            Queue<Character> dungeonEnemies = new Queue<Character>();
            dungeonEnemies.Enqueue(new Goblin("Gorg the Sneaky"));
            dungeonEnemies.Enqueue(new Goblin("Zib the Vile"));
            dungeonEnemies.Enqueue(new Dragon("Ignis the Flame-Bringer"));

            Console.WriteLine($"{player.Name} enters the dungeon!");
```

---

### Process the Main Combat States
We need a loop to execute real-time alternating combat rounds until either the player falls or all enemies are eliminated.

* Write a main loop that extracts the front enemy from the queue via `.Peek()` without removing them yet.
* Implement an inner loop checking if both the `player` is alive and there are still enemies in the `dungeonEnemies` queue.
* Execute player attacks, clear dead enemies out of the queue using `.Dequeue()`, and process corresponding enemy counter-attacks.

```csharp
            while (player.IsAlive && dungeonEnemies.Count > 0)
            {
                Character currentEnemy = dungeonEnemies.Peek();
                Console.WriteLine($"\n--------------------------------------------");
                Console.WriteLine($"A wild {currentEnemy.GetType().Name} appears: {currentEnemy.Name}!");
                Console.WriteLine($"--------------------------------------------");

                while (player.IsAlive && currentEnemy.IsAlive)
                {
                    Console.WriteLine("\nPress ENTER to attack!");
                    Console.ReadLine();
                    player.PerformAttack(currentEnemy);

                    if (!currentEnemy.IsAlive)
                    {
                        Console.WriteLine($"You defeated {currentEnemy.Name}!");
                        dungeonEnemies.Dequeue();
                        break;
                    }

                    currentEnemy.PerformAttack(player);
                }
            }
```

---

### Output Resolution Flags and Win Conditions
Once the loops break, we evaluate why they ended to display either a victory or a defeat prompt.

* Check the current state of `player.IsAlive`.
* Display your final narrative win or lose text tracking to complete the game loop block.

```csharp
            Console.WriteLine("\n--- END OF GAME ---");
            if (player.IsAlive)
                Console.WriteLine("CONGRATULATIONS! You cleared the dungeon and won the game!");
            else
                Console.WriteLine("YOU DID NOT SURVIVE! GAME OVER.");
            Console.WriteLine("========================================");
        }
    }
}
```


[Previous: 06 - Goblin class](06-goblin.md)