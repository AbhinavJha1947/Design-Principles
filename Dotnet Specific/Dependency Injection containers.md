# Dependency Injection Containers (IoC Containers)

## Concept
A **DI Container** manages the creation and lifetime of dependencies. In .NET Core, this is built-in (`IServiceProvider`).

## Lifetimes
Choosing the wrong lifetime is a common source of bugs (e.g., memory leaks or stale data).

### 1. Transient (`AddTransient`)
-   **Created**: Every time it is requested.
-   **Use Case**: Lightweight, stateless services.
-   **Warning**: Don't inject expensive objects here.

### 2. Scoped (`AddScoped`)
-   **Created**: Once per Client Request (HTTP Request).
-   **Use Case**: DbContext, UserSession.
-   **Warning**: Do NOT capture Scoped services inside Singleton services (Captive Dependency).

### 3. Singleton (`AddSingleton`)
-   **Created**: First time requested, lives until app shuts down.
-   **Use Case**: Caches, Configuration, connection pools.
-   **Warning**: Must be thread-safe!

## The Captive Dependency Problem

### Violation
Injecting a `Scoped` service (DbContext) into a `Singleton` service.

```csharp
// Registered as Singleton
public class MyCache
{
    private readonly AppDbContext _db; // Registered as Scoped

    public MyCache(AppDbContext db)
    {
        _db = db;
        // ERROR: This _db instance will live forever (Singleton),
        // but it was meant to die after the request.
        // It will accumulate tracked entities and eventually crash (memory leak).
    }
}
```

### Solution (`IServiceScopeFactory`)
If a Singleton needs a Scoped service, create a scope manually.

```csharp
public class MyCache
{
    private readonly IServiceScopeFactory _scopeFactory;

    public MyCache(IServiceScopeFactory scopeFactory)
    {
        _scopeFactory = scopeFactory;
    }

    public void Refresh()
    {
        using (var scope = _scopeFactory.CreateScope())
        {
             var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
             // Do work with db...
             // db is disposed here.
        }
    }
}
```
