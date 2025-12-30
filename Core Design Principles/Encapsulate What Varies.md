# Encapsulate What Varies

## Concept
Identifies the aspects of your application that vary and separate them from what stays the same.

> *"If you have code that changes frequently, isolate it."*

This is the foundational principle behind almost every Design Pattern.
-   **Strategy Pattern**: Encapsulates varying algorithms.
-   **Factory Pattern**: Encapsulates varying object creation logic.
-   **Observer Pattern**: Encapsulates varying responses to events.

## Benefits
1.  **Minimizes Impact of Change**: When the varying part changes, it doesn't break the stable parts.
2.  **Testability**: The isolated part is easier to test.

## Example

### Violation (Mixing Stable and Varying Code)
Here, the `OrderProcessor` (stable process) is mixed with tax calculation rules (which change every year/region).

```csharp
public class OrderProcessor
{
    public void Process(Order order, string country)
    {
        // 1. Validate (Stable)
        if (!order.IsValid) throw new Exception();

        // 2. Calculate Tax (Varies frequently!)
        decimal tax = 0;
        if (country == "US") tax = order.Total * 0.1m;
        else if (country == "EU") tax = order.Total * 0.2m;
        // If we add India, we modify this class again. Risk!

        // 3. Save (Stable)
        order.Save();
    }
}
```

### Solution (Encapsulation)
Move the tax logic into its own "capsule" (Class/Interface).

```csharp
// The "Variable" part
public interface ITaxCalculator
{
    decimal Calculate(decimal amount);
}

// The "Stable" part
public class OrderProcessor
{
    private readonly ITaxCalculator _taxCalculator;

    public OrderProcessor(ITaxCalculator taxCalculator)
    {
        _taxCalculator = taxCalculator;
    }

    public void Process(Order order)
    {
        // 1. Validate
        if (!order.IsValid) throw new Exception();

        // 2. Delegating to the encapsulated logic
        decimal tax = _taxCalculator.Calculate(order.Total);

        // 3. Save
        order.Save();
    }
}
```
Now, adding a new country specific tax calculator involves adding a new class, not touching `OrderProcessor`.
