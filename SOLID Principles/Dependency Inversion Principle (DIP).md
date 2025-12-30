# Dependency Inversion Principle (DIP)

## Definition
1.  High-level modules should not depend on low-level modules. Both should depend on abstractions.
2.  Abstractions should not depend on details. Details should depend on abstractions.

**Note**: Dependency Injection (DI) is the *pattern* we use to implement the Dependency Inversion *Principle*.

## Why?
-   **High-level modules** (Business Logic) are the core value.
-   **Low-level modules** (Database, FileSystem, Email) are implementation details/plumbing.
-   If High-level depends on Low-level, changing the database breaks the business logic. That's backward.

## Example

### Violation (High depends on Low)

```csharp
// Low Level (Detail)
public class FileLogger
{
    public void Log(string msg) { File.WriteAllText("log.txt", msg); }
}

// High Level (Business Logic)
public class TransferService
{
    private FileLogger _logger; // Dependency on concrete class!

    public TransferService()
    {
        _logger = new FileLogger(); 
    }

    public void Transfer()
    {
        // ...
        _logger.Log("Done");
    }
}
```

### Solution (Both depend on Abstraction)
We introduce an interface. `TransferService` depends on the interface. `FileLogger` implements the interface.

```csharp
// Abstraction
public interface ILogger 
{ 
    void Log(string msg); 
}

// High Level depends on Abstraction
public class TransferService
{
    private readonly ILogger _logger;

    public TransferService(ILogger logger)
    {
        _logger = logger;
    }
}

// Low Level depends on Abstraction (Validation)
public class FileLogger : ILogger
{
    public void Log(string msg) { /*...*/ }
}
```
Now `TransferService` doesn't care if it's a `FileLogger`, `DbLogger`, or `CloudLogger`.
