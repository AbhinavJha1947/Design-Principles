# Dependency Injection vs Service Locator

Both are patterns to achieve **Inversion of Control**, but one is generally considered an **Anti-Pattern**.

## The Goal
We want `OrderService` to send emails, but strictly decouple it from the concrete `SmtpEmailSender`.

## 1. Dependency Injection (The Good)
Dependencies are explicitly declared in the constructor.

```csharp
public class OrderService
{
    private readonly IEmailSender _emailSender;

    // "I need an IEmailSender to work. I don't care where it comes from."
    public OrderService(IEmailSender emailSender)
    {
        _emailSender = emailSender;
    }
}
```
### Pros
-   **Explicit**: You look at the constructor and know exactly what this class needs.
-   **Testable**: Easy to pass `new MockEmailSender()` in tests.
-   **Loose Coupling**: The class knows nothing about the container.

## 2. Service Locator (The Bad)
The class asks a global "Locator" for its dependencies.

```csharp
public class OrderService
{
    private readonly IEmailSender _emailSender;

    public OrderService()
    {
        // "Hey global static helper, give me an IEmailSender."
        _emailSender = ServiceLocator.Current.GetInstance<IEmailSender>();
    }
}
```
### Cons
-   **Hidden Dependencies**: The constructor is empty. You have to read the code to know it needs an `IEmailSender`.
-   **Fragile Tests**: You must set up the global `ServiceLocator` state before running tests, or they crash. Parallel testing becomes hard (shared static state).
-   **Coupling**: The class now depends on `ServiceLocator` AND `IEmailSender`.

## Conclusion
**Always prefer Dependency Injection.**
Only use Service Locator in legacy systems or rare edge cases where Constructor Injection is impossible (e.g., inside an attribute or static context).
