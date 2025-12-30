# Records vs Classes (C# 9+)

## Concept
A **Record** is a special kind of class (or struct) that provides built-in functionality for working with infinite data. It is "Reference Type looking like a Value Type".

## Key Features of Records
1.  **Value Equality**: Two records are equal if their properties are equal, even if they are different objects. (Classes use Reference Equality by default).
2.  **Immutability**: Designed to be immutable by default (using `init` setters).
3.  **Parsimony**: Less boilerplate code (`ToString`, `GetHashCode`, `Equals` are generated for you).
4.  **Destructuring**: Built-in support for deconstruction.

## Comparison

### Class (Traditional)
Use for:
-   Objects with identity (Entities).
-   Objects with mutable state.
-   Services / Logic holders.

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}

var u1 = new User { Id = 1, Name = "A" };
var u2 = new User { Id = 1, Name = "A" };
Console.WriteLine(u1 == u2); // FALSE (Different References)
```

### Record (Data Centric)
Use for:
-   DTOs (Data Transfer Objects).
-   API Responses.
-   Configuration objects.
-   Value Objects (DDD).

```csharp
// Concise Syntax
public record User(int Id, string Name);

var u1 = new User(1, "A");
var u2 = new User(1, "A");
Console.WriteLine(u1 == u2); // TRUE (Same Values)

// Non-destructive mutation
var u3 = u1 with { Name = "B" };
```
