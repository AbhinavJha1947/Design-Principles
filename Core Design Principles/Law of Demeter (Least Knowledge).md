# Law of Demeter (Least Knowledge)

## Concept
The **Law of Demeter (LoD)** implies that a module should not know about the inner details of the objects it manipulates.

> *"Don't talk to strangers."* — *Pragmatic Programmer*

Formally, a method `M` of object `O` should only invoke methods of:
1.  `O` itself.
2.  `M`'s parameters.
3.  Any objects created/instantiated within `M`.
4.  `O`'s direct component objects (fields).

**It should NOT invoke methods on objects returned by any of the above function calls.**

## The "Train Wreck"
The classic violation is a long chain of accessors:
`order.Customer.Address.City.ZipCode`
This means the `Order` object knows about `ZipCode` structure, which is 4 hops away.

## Example

### Violation (Talking to Strangers)

```csharp
public class Paperboy
{
    public void CollectPayment(Customer customer)
    {
        // Violation!
        // Paperboy is reaching into Customer's pockets (Wallet)
        // to get Money. Paperboy shouldn't know Customer has a Wallet.
        decimal payment = 2.00m;
        Wallet wallet = customer.GetWallet();
        if (wallet.TotalCash >= payment)
        {
            wallet.Subtract(payment);
        }
    }
}
```

### Solution (Tell, Don't Ask)
Ask the object to do the work.

```csharp
public class Paperboy
{
    public void CollectPayment(Customer customer)
    {
        // Respectful: Hey Customer, pay me 2.00.
        // I don't care if you use a wallet, pocket, or bank account.
        customer.Pay(2.00m);
    }
}

public class Customer
{
    private Wallet _wallet;
    
    public void Pay(decimal amount)
    {
        _wallet.Subtract(amount);
    }
}
```

## Exceptions
Fluent interfaces (Builders, LINQ) are exceptions to this rule because they return the *same* type or context, not a *stranger's* internal structure.
-   `list.Where(...).Select(...).ToList()` is **OK**.
