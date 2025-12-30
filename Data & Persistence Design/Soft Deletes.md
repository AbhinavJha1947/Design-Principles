# Soft Deletes

## Concept
**Soft Delete** means marking a record as "deleted" (e.g., `IsDeleted = true` or `DeletedAt = DateTime.Now`) rather than physically removing it from the database (`DELETE FROM...`).

## Pros
1.  **History**: You can see what was deleted and when.
2.  **Undo**: You can restore data easily.
3.  **Audit**: Essential for financial or legal apps.

## Cons
1.  **Complexity**: **EVERY** query must include `WHERE IsDeleted = false`. If you forget one, you leak deleted data.
2.  **Referential Integrity**: Foreign Keys might break logic. (e.g., "Unique Email" constraint - if I delete a user, can I register again with the same email? DB says "Duplicate Key" unless you filter uniqueness).
3.  **Storage**: The DB grows indefinitely.

## Implementation in EF Core
Use **Global Query Filters** to handle the complexity automatically.

```csharp
public class MyContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder builder)
    {
        // Automatically apply this to all queries for User
        builder.Entity<User>().HasQueryFilter(u => !u.IsDeleted);
    }
}
```
Now `db.Users.ToList()` returns only active users.
To see deleted ones: `db.Users.IgnoreQueryFilters().ToList()`.
