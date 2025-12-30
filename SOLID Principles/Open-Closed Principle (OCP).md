# Open-Closed Principle (OCP)

## Definition
> *"Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."*

-   **Open for Extension**: You can add new behavior (e.g., a new payment method).
-   **Closed for Modification**: You don't have to change the source code of the existing class to add that behavior.

## Why?
Every time you modify existing, tested code, you risk introducing bugs. OCP encourages adding new code (new classes) instead of changing old code.

## Example

### Violation (Modification Required)
We have a `Shape` calculator. If we want to add `Triangle`, we have to *modify* the switch statement.

```csharp
public class Rectangle { public double Width; public double Height; }
public class Circle { public double Radius; }

public class AreaCalculator
{
    public double ComputeArea(object shape)
    {
        if (shape is Rectangle r)
            return r.Width * r.Height;
        else if (shape is Circle c)
            return c.Radius * c.Radius * Math.PI;
        
        // If we add Triangle, we MUST modify this file!
        // Violation of OCP.
        
        return 0;
    }
}
```

### Solution (Extension via Polymorphism)
We rely on an interface/base class. To add `Triangle`, we create a new class. We never touch `AreaCalculator` again.

```csharp
// The Contract
public abstract class Shape
{
    public abstract double Area();
}

// Extension 1
public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    public override double Area() => Width * Height;
}

// Extension 2
public class Circle : Shape
{
    public double Radius { get; set; }
    public override double Area() => Radius * Radius * Math.PI;
}

// Closed for Modification
public class AreaCalculator
{
    public double ComputeTotalArea(List<Shape> shapes)
    {
        double total = 0;
        foreach (var shape in shapes)
        {
            // Abstraction layer - works for ANY future shape
            total += shape.Area(); 
        }
        return total;
    }
}
```
