# Liskov Substitution Principle (LSP)

## Definition
> *"Subtypes must be substitutable for their base types."*

If `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` without altering the correctness of the program.

## The Litmus Test
If you override a method and...
1.  **Throw `NotImplementedException`**: You violated LSP.
2.  **Do nothing (No-op)**: You likely violated LSP.
3.  **Weaken Preconditions** or **Strengthen Postconditions**: You violated LSP.

## Example

### Violation (The Square-Rectangle Problem)
A Square is mathematically a Rectangle, but behaviorally it is not. Setting width on a square must also set height, which violates the behavior expectation of a base Rectangle.

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
}

public class Square : Rectangle
{
    public override int Width 
    {
        set { base.Width = value; base.Height = value; }
    }
    public override int Height
    {
        set { base.Width = value; base.Height = value; }
    }
}

// The Consumer
public void MakeValidRectangle(Rectangle r)
{
    r.Width = 5;
    r.Height = 10;
    // If r is a Square, Area is 100 (10*10), not 50!
    // Assert.Equal(50, r.Area); // Fails for Square
}
```

### Violation (The Ostrich Problem)
Inheriting from a base class but forcing a throw on a method.

```csharp
public class Bird
{
    public virtual void Fly() { /* ... */ }
}

public class Ostrich : Bird
{
    public override void Fly()
    {
        throw new NotImplementedException("Ostriches can't fly!");
    }
}
```

### Solution (Segregate Hierarchy)
Do not inherit if the behavior doesn't match.

```csharp
// 1. Common Base (if any)
public class Bird { }

// 2. Capability Interfaces OR Sub-bases
public class FlyingBird : Bird
{
    public virtual void Fly() { }
}

public class Ostrich : Bird
{
    // Ostrich is a Bird, but not a FlyingBird.
    // No Fly() method to break.
}

public class Eagle : FlyingBird
{
    public override void Fly() { }
}
```
