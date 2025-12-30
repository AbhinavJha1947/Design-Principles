# DRY (Don’t Repeat Yourself)

## Concept
**DRY** stands for **"Don't Repeat Yourself"**. It is a principle of software development aimed at reducing repetition of software patterns, replacing it with abstractions or adhering to data normalization to avoid redundancy.

> *"Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."* — *The Pragmatic Programmer*

## Misconception: Code Duplication vs. Knowledge Duplication
Many developers think DRY means "never type the same code twice". This is wrong.
-   **Code Duplication**: Two lines of code look the same. This is often fine (accidental duplication).
-   **Knowledge Duplication**: Two pieces of code represent the same business logic or rule. If the rule changes, you have to fix it in two places. **This is what DRY forbids.**

## Example

### Violation (Repeating Knowledge)
Here, the logic for calculating a discount "10% off for Gold members" is repeated. If marketing changes it to 15%, we might miss one spot.

```csharp
public class OrderService
{
    public decimal CalculateOrderTotal(Order order, User user)
    {
        decimal total = order.Items.Sum(i => i.Price);
        
        // Replicating business logic 1
        if (user.Type == UserType.Gold)
        {
            total *= 0.9m;
        }
        
        return total;
    }
}

public class InvoiceService
{
    public decimal GenerateInvoiceAmount(Order order, User user)
    {
        decimal total = order.Items.Sum(i => i.Price);

        // Replicating business logic 2
        if (user.Type == UserType.Gold)
        {
            total *= 0.9m;
        }

        return total;
    }
}
```

### Solution (Single Source of Truth)
Centralize the "Knowledge" of how discounts work.

```csharp
public class DiscountStrategy
{
    public decimal ApplyDiscount(decimal total, User user)
    {
        if (user.Type == UserType.Gold)
        {
            return total * 0.9m;
        }
        return total;
    }
}

// Usage in both services:
// total = _discountStrategy.ApplyDiscount(total, user);
```

## Warning against Over-DRYing
Trying to DRY up code that *looks* similar but *changes for different reasons* leads to coupling.
-   **Rule of Three**: Wait until you see a pattern used three times before you refactor it into an abstraction.
