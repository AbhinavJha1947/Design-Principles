# Repository Pattern & Unit of Work

## The Repository Pattern
Mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects.

-   **Goal**: Decouple business logic from Data Access specifics (EF Core, SQL, Files).
-   **Contract**: `GetById`, `Add`, `Remove`, `Find(predicate)`.

### The Controversy: IQueryable in Repo?
**Bad Practice**: Returning `IQueryable<T>` from a Repository.
-   *Why?* It leaks database concerns. The caller (Service layer) can add `.Where()` clauses that might not translate to SQL or rely on DB specifics.
-   *Fix*: Return `IEnumerable<T>`, `List<T>`, or `Specification` results. **Repository should determine the query, not the caller.**

## Unit of Work (UoW)
Maintains a list of objects affected by a business transaction and coordinates the writing out of changes.

-   **Goal**: Transaction management. Ensure multiple repositories share the **same database context** so changes are committed atomically.

### Implementation Example

```csharp
public interface IUnitOfWork : IDisposable
{
    IOrderRepository Orders { get; }
    ICustomerRepository Customers { get; }
    int Complete(); // Calls SaveChanges()
}

// Usage in Service
public void PlaceOrder(Order o)
{
    // Transaction starts implicit scope
    _uow.Orders.Add(o);
    var cust = _uow.Customers.Get(o.CustomerId);
    cust.LastOrderDate = DateTime.Now;
    
    // One interaction with DB
    _uow.Complete(); 
}
```
