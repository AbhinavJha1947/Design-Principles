# CQRS (Command Query Responsibility Segregation)

## Concept
**CQRS** is an architectural pattern that separates the models for reading data (**Queries**) from the models for updating data (**Commands**).

> *"Asking a question should not change the answer."* — *Bertrand Meyer*

## Why split them?
In traditional architectures (CRUD), the same model is used for both. This leads to issues:
1.  **Mismatch**: Read models often need specialized DTOs (joins, aggregation), while Write models need complex validation and domain logic.
2.  **Performance**: Scaling reads is different from scaling writes (e.g., Read Replicas).

## The Pattern

### 1. Commands (Write Side)
-   **Intent**: "Do something".
-   **Naming**: Imperative (`RegisterUser`, `PlaceOrder`).
-   **Return**: Void (or ID/Result). No data.
-   **Stack**: Domain Entities -> Validation -> Repository -> DB (Normalized).

### 2. Queries (Read Side)
-   **Intent**: "Get something".
-   **Naming**: `GetUserById`, `SearchProducts`.
-   **Return**: DTOs (ViewModels).
-   **Stack**: Direct SQL / Dapper -> DB (or specific Read DB). Bypasses the Domain Model overhead.

## Mediator Pattern (MediatR)
Often paired with CQRS in .NET.
-   Controller sends a `Command` object to Mediator.
-   Mediator finds the matching `CommandHandler`.
-   Handler executes logic.
-   **Benefit**: Controllers become thin and decoupled.

```csharp
// Controller
public async Task<IActionResult> Register(RegisterUserCommand cmd)
{
    await _mediator.Send(cmd);
    return Ok();
}
```
