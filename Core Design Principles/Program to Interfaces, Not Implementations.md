# Program to Interfaces, Not Implementations

## Concept
This principle states that clients should depend on abstract interfaces (like `interface` or `abstract class` in C#) rather than concrete implementations (classes).

> *"Dependency Injection is built entirely on this principle."*

## Why?
1.  **Loose Coupling**: The client doesn't need to know *how* something is done, just *what* can be done.
2.  **Testability**: You can easily swap a real `DatabaseService` with a `MockDatabaseService` for unit testing if you program to an interface.
3.  **Flexibility**: You can switch underlying technologies (e.g., from SQL Server to Oracle) without changing the client code.

## Example

### Violation (Dependent on Concrete Class)
The `CustomerService` is tightly coupled to `SqlRepository`. It is impossible to unit test this without a real database.

```csharp
public class SqlRepository
{
    public void Save(string data) { /* Writes to SQL DB */ }
}

public class CustomerService
{
    private SqlRepository _repo;

    public CustomerService()
    {
        // Hard dependency!
        _repo = new SqlRepository(); 
    }

    public void Register()
    {
        _repo.Save("Customer");
    }
}
```

### Solution (Dependent on Interface)
Now `CustomerService` can work with *any* repository.

```csharp
// 1. Define the Interface (Contract)
public interface IRepository
{
    void Save(string data);
}

// 2. Implement it
public class SqlRepository : IRepository { /*...*/ }
public class FileRepository : IRepository { /*...*/ }
public class MockRepository : IRepository { /*...*/ } // For Tests!

// 3. Inject it
public class CustomerService
{
    private readonly IRepository _repo;

    // Constructor Injection
    public CustomerService(IRepository repo)
    {
        _repo = repo;
    }

    public void Register()
    {
        _repo.Save("Customer");
    }
}
```
