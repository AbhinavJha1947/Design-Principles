# Separation of Concerns (SoC)

## Concept
**Separation of Concerns** is a design principle for separating a computer program into distinct sections such that each section addresses a separate concern.

> *"A concern is a set of information that affects the code of a computer program."*

This is the macro-level version of the Single Responsibility Principle.
-   **SRP** applies to Classes.
-   **SoC** applies to Modules, Layers, and Systems.

## Layers of Concern (Standard Architecture)
In a typical web application, we separate:
1.  **Presentation Layer (UI)**: Handling HTTP requests, HTML rendering.
2.  **Business Logic Layer (Domain)**: Business rules, validations, calculations.
3.  **Data Access Layer (Persistence)**: SQL queries, Database connections.

## Example

### Violation (Mixing Concerns)
Code that handles HTTP, validation, and SQL queries all in one Controller action.

```csharp
[HttpPost]
public IActionResult RegisterUser(string name, string email)
{
    // Concern 1: Validation
    if (string.IsNullOrEmpty(email)) return BadRequest();

    // Concern 2: Business Rule
    if (name == "Admin") return BadRequest("Reserved Name");

    // Concern 3: Data Access
    using (var conn = new SqlConnection("..."))
    {
        conn.Open();
        var cmd = new SqlCommand("INSERT INTO Users...", conn);
        cmd.ExecuteNonQuery();
    }

    // Concern 4: Notification
    var smtp = new SmtpClient();
    smtp.Send("Welcome!");

    return Ok();
}
```

### Solution (Separated)
Each layer handles its own concern.

```csharp
[HttpPost]
public IActionResult Register(UserDto dto)
{
    // Presentation Concern: Parsing inputs
    _userService.Register(dto.Name, dto.Email);
    return Ok();
}

public class UserService
{
    public void Register(string name, string email)
    {
        // Business Concern: Rules & Orchestration
        if (IsReserved(name)) throw new DomainException();
        
        _userRepo.Save(new User(name, email));
        _emailService.SendWelcome(email);
    }
}

public class UserRepository
{
    public void Save(User user)
    {
        // Data Access Concern: SQL
        _db.Users.Add(user);
        _db.SaveChanges();
    }
}
```
