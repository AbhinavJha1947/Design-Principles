# High Cohesion & Low Coupling

These are the twin pillars of good architectural design. You virtually always want **High Cohesion** and **Low Coupling**.

## 1. High Cohesion
**Cohesion** refers to how closely related and focused the responsibilities of a single module (class, package, function) are.

-   **High Cohesion (Good)**: A class does one thing and does it well (follows SRP). Elements inside the class belong together.
    -   *Example*: `OrderValidator` class only contains methods related to validating an order.
-   **Low Cohesion (Bad)**: A "God Class" that does everything.
    -   *Example*: `Utilities` class containing `CalculateTax()`, `SendEmail()`, `ResizeImage()`, and `ConnectToDatabase()`.

## 2. Low Coupling
**Coupling** refers to the degree of direct knowledge that one element has of another.

-   **Low Coupling (Good)**: Classes are independent. Changing one class doesn't break other classes.
    -   *Achieved by*: Using Interfaces, Dependency Injection, and Event-driven architecture.
-   **High Coupling (Bad)**: Spaghetti code. Changing a database column requires changing the UI code.
    -   *Example*: Classes instantiating concrete dependencies directly (`new SqlServerRepository()`).

## Example

### Violation: Low Cohesion & High Coupling

```csharp
public class OrderProcessor
{
    public void Process(Order order)
    {
        // Low Cohesion: Why is OrderProcessor validating?
        if (order.Total <= 0) throw new Exception("Invalid");

        // High Coupling: Directly depending on concrete SqlRepository
        var repo = new SqlRepository(); 
        repo.Save(order);

        // Low Cohesion: Why is OrderProcessor sending emails?
        var emailer = new SmtpClient();
        emailer.Send("Confirmed!");
    }
}
```

### Solution: High Cohesion & Low Coupling

```csharp
public class OrderProcessor
{
    private readonly IRepository _repo; // Low Coupling (Interface)
    private readonly INotificationService _nots;

    public OrderProcessor(IRepository repo, INotificationService nots)
    {
        _repo = repo;
        _nots = nots;
    }

    public void Process(Order order)
    {
        // Validation logic moved to high-cohesion Validator or Domain Entity
        order.Validate(); 

        _repo.Save(order);
        _nots.NotifySuccess(order);
    }
}
```
