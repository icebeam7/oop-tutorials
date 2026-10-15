# 03 - Sealed class in `Mage`

A sealed class cannot be used as a base class for any other class. That means no child class can derive from it.

## Step 1: Seal the `Mage` class

```csharp
using System;

namespace InheritanceMinigame.Classes
{
    public sealed class Mage : Character
    {
        // ...
    }
}
```

Now `Mage` is sealed, so it is the final version of that class.

## Step 2: Try to create a subclass of `Mage`

```csharp
public class ArchMage : Mage
{
    public ArchMage(string name) : base(name) { }
}
```

Build the app. This will fail with a compiler error:

```csharp
// Compiler error:
// 'ArchMage': cannot derive from sealed type 'Mage'
```

This means the class is intentionally closed to inheritance.

## Step 3: Use `Mage` directly in the main program

Remove the `ArchMage` class (you can delete the file) and run the app. Play the game as a `Mage`. It is still perfectly valid to instantiate and use a sealed class, but you cannot extend it with another subclass.

A sealed class is useful when the implementation is meant to be final and no additional specialization is needed.

[Previous: 02 - Sealed method in Warrior](02-sealed-method.md) | [Next: 04 - Protected members and type checks with is/as](04-protected-members-and-is-as.md)
