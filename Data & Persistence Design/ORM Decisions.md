# ORM Decisions (EF Core)

## 1. Lazy vs Eager Loading

### Lazy Loading
Data is loaded automatically *when you access the property*.
-   **Pros**: Convenient. You don't need to plan queries ahead.
-   **Cons**: **N+1 Problem**. Performance disaster in loops.
-   **Usage**: Requires `virtual` properties and a proxy.

### Eager Loading (`.Include()`)
Data is loaded *in the initial query* using JOINs.
-   **Pros**: One roundtrip to DB. Efficient.
-   **Cons**: Can fetch too much data (Cartesian explosion if too many `.Include`s).

### Explicit Loading (`.Entry(...).Load()`)
You manually tell EF to load a relationship later. Balance between Lazy and Eager.

## 2. The N+1 Problem
The most common performance issue in ORMs.

### Scenario
You want to print 100 Orders and their Customer Name.

```csharp
// 1 Query to get Orders
var orders = db.Orders.ToList(); 

foreach(var order in orders)
{
    // 1 Query PER ORDER to get Customer (Lazy Load)
    // 100 Orders + 1 initial query = 101 Queries!
    Console.WriteLine(order.Customer.Name); 
}
```

### Fix (Eager Loading)
```csharp
// 1 Query Total (JOIN)
var orders = db.Orders.Include(o => o.Customer).ToList(); 
```

## 3. Tracking vs NoTracking
-   **Tracking (Default)**: EF keeps a snapshot of entities. detecting changes for `.SaveChanges()`. Good for **read-write** scenarios.
-   **NoTracking (`.AsNoTracking()`)**: EF reads data returns objects, and forgets them. Faster, less memory. Good for **read-only** scenarios (API GET requests).