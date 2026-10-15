# 01 - Abstract Character and abstract `PerformAttack`

In the previous lesson, the `Character` class was a normal class. That works, but it also allows us to create objects directly from the base class. In object-oriented programming, sometimes we do not want that, because a base class is only a template for its children.

For that reason, we can make the class **abstract**.

## Step 1: Make the base class and a specific method abstract

Mark both the `Character` class and the `PerformAttack` method as `abstract`.

```csharp
// ...
    public abstract class Character
    {
        // ...
        public abstract void PerformAttack(Character target);
    }
```

The `abstract` keyword means:

- You **CANNOT** create an instance from `Character` directly.
- A subclass **MUST** provide its own version of `PerformAttack`.

## Step 2: Try to instantiate the class

If we try to create a `Character` object in the main program, the compiler will reject it:

```csharp
using InheritanceMinigame.Classes;

Character hero = new Character("Hero", 100, 10);
```

Build the app using `dotnet build` in the terminal. This will produce a compiler error similar to:

```csharp
// Compiler error:
// 'Character' is abstract and does not provide an implementation for
// 'PerformAttack(Character)'
// Character hero = new Character("Hero", 100, 10);
```

This is exactly the point of an abstract class: it defines shared behavior, **but it does not allow incomplete objects to exist in the program**.

## Step 3: Use concrete subclasses

Only the real classes such as `Warrior` or `Mage` can be instantiated:

```csharp
Character player = new Warrior("Aragorn");
player.PerformAttack(new Mage("Merlin"));
```

This works because the runtime object is a child class that implements the abstract method.

Abstract classes are useful when:

- Several classes share the same base structure.
- The base class defines common properties and methods.
- Each derived class has a different implementation of one or more behaviors.

In our game, `Warrior` and `Mage` can both be `Character`, but each uses its own attack logic.

[Previous: Polymorphism README](README.md) | [Next: 02 - Sealed method in Warrior](02-sealed-method.md)
