# Interface Design Patterns

## 1. Role Interfaces (ISP)
Interfaces should represent a single "role" or capability, not a class identity.
-   **Good**: `ICustomerValidator`, `IEmailSender`
-   **Bad**: `ICustomerServices` (Contains validation, emailing, and database logic).

## 2. Header Interfaces (Anti-Pattern)
Extracting an interface for *every* method in a class just to say "I have an interface".
-   **Symptom**: `CustomerService` has `ICustomerService`, and `ICustomerService` has 20 methods that exactly match the class.
-   **Fix**: Only define interfaces for things that need decoupling (for testing or multiple implementations).

## 3. Explicit vs Implicit Implementation (C#)

### Implicit (Normal)
The method is part of the class's public contract.
```csharp
public class Logger : ILog
{
    public void Log() { } // Accessible via Logger instance
}
```

### Explicit
The method is *hidden* unless cast to the Interface.
-   **Use Case**: When implementing two interfaces that have the same method name, or when you want to "hide" dangerous/advanced methods from the default class usage.

```csharp
public class DisposableClass : IDisposable
{
    // Hidden from "new DisposableClass().Dispose()"
    // Only visible via "((IDisposable)obj).Dispose()"
    void IDisposable.Dispose() { } 
}
```

## 4. Default Interface Methods (C# 8+)
You can provide a default implementation in the interface itself.
-   **Pros**: Allows adding new methods to published interfaces without breaking existing implementers.
-   **Cons**: Can lead to "Multiple Inheritance" confusion. Use sparingly for versioning.