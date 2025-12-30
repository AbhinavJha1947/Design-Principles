# Fluent Interfaces

## Concept
A **Fluent Interface** is an API design that relies extensively on method chaining to make more readable code. The goal is to make the code read like a sentence (Domain Specific Language).

## Example: Building a Query

### Standard API
```csharp
var query = new Query();
query.SetTable("Users");
query.AddSelect("Name");
query.AddWhere("Age > 18");
query.SetLimit(10);
```

### Fluent API
```csharp
var query = new Query()
    .From("Users")
    .Select("Name")
    .Where("Age > 18")
    .Limit(10);
```

## How to Implement
Each method must return the context (usually `this`) to allow the chain to continue.

```csharp
public class EmailBuilder
{
    private string _to;
    private string _subject;

    public EmailBuilder To(string address)
    {
        _to = address;
        return this; // Key check-in
    }

    public EmailBuilder Subject(string subject)
    {
        _subject = subject;
        return this;
    }

    public void Send() { /*...*/ }
}

// Usage
new EmailBuilder()
    .To("admin@test.com")
    .Subject("Alert")
    .Send();
```

## Pros & Cons
-   **Pros**: Highly readable, guides the developer through a "flow".
-   **Cons**: Harder to debug (stack traces are on one line). Harder to maintain the builder logic.

## Best Use Cases
-   **Builders**: Constructing complex objects (`ConfigurationBuilder`, `HostBuilder`).
-   **linq**: `Where(...).Select(...)`.
-   **Testing Libraries**: `Assert.That(x).Is.Not.Null`.
