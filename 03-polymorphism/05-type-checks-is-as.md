# 05 - Type checks with `is` and `as`

## Step 1: Use `is` in the main program

The `is` keyword lets us check the runtime type of an object.

In your `Program.cs`, check the class type after the player object is created using `new` and the selected class:

```csharp

// ...

Character player;

// ...

if (player is Mage)
{
    Console.WriteLine("This character is a mage.");
}
else
{
    Console.WriteLine("This character is not a mage.");
}

if (player is Warrior warrior)
{
    Console.WriteLine($"{warrior.Name} has a shield block of {warrior.ShieldBlock}.");
}

// ...

```

Run the program to demonstrate above functionality.

## Step 2: Use `as` in the main program

The `as` keyword is used to convert an object to a specific type when the object may be compatible:

Add this code below the previous code that you just added.

```csharp
Mage? mage = player as Mage;

if (mage != null)
{
    Console.WriteLine($"{mage.Name} is a mage with {mage.Mana} mana.");
}
else
{
    Console.WriteLine("The player is not a Mage.");
}
```

The `?` after a data type (or class) means that the variable (or object) accepts `null` values.

If the object is not compatible, `as` returns `null` instead of throwing an exception because of the `?`.

Run the program to demonstrate above functionality.

[Previous: 04 - Protected members](04-protected-members.md) | [Next: 02 - Inheritance](../02-inheritance/README.md)
