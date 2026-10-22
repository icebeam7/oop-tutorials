# 05 - Explicit interface members

Sometimes two interfaces define the same method name. In that case, we need to be explicit about which implementation belongs to which interface.

This is where **explicit interface implementation** matters. When two different interfaces declare a method with the same name, C# needs a way to tell them apart. By implementing the method through the interface name, we can resolve the conflict clearly and safely.

## Step 1: Create the first interface

Create a file called `IAttack.cs`.

Add this code:

```csharp
public interface IAttack
{
    void Attack();
}
```

## Step 2: Create the second interface

Create a file called `IDefend.cs`.

Add this code:

```csharp
public interface IDefend
{
    void Attack();
}
```

Both interfaces have a method called `Attack()`. That is where explicit interface members become useful.

## Step 3: Create the Knight class

Create `Knight.cs`.

Add this implementation:

```csharp
public class Knight : IAttack, IDefend
{
    void IAttack.Attack()
    {
        Console.WriteLine("Knight attacks with a sword.");
    }

    void IDefend.Attack()
    {
        Console.WriteLine("Knight raises a shield to defend.");
    }
}
```

## Step 4: Add usage in Program.cs

Update `Program.cs` and add:

```csharp
Knight knight = new Knight();

IAttack attack = knight;
attack.Attack();

IDefend defend = knight;
defend.Attack();
```

## What this shows

- two interfaces can contain the same method name
- explicit implementation removes the ambiguity
- we choose which interface method we are calling by casting to the interface type
- MAIN DISADVANTAGE: You can't directly invoke the `Attack` method. You need to explicitly cast the object to the interface.

[Previous: 04 - Multiple interfaces and interface inheritance](04-multiple-interfaces-and-inheritance.md)
