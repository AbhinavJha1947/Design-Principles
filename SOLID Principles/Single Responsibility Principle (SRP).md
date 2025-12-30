# Single Responsibility Principle (SRP)

## Definition
> *"A class should have one, and only one, reason to change."* — *Robert C. Martin*

This is often misunderstood as "a class should only do one thing". While related, the core idea is about **people** providing requirements. If a change to the Database Schema requires you to change the `Employee` class, AND a change to the Payroll Calculation logic also requires you to change `Employee`, you have violated SRP.

## Logic vs. Responsibility
-   **Cohesion**: SRP leads to high cohesion.
-   **Coupling**: Violating SRP usually leads to high coupling between unrelated responsibilities.

## Example

### Violation (God Class)
The `Employee` class handles business logic, database persistence, and tax calculations. It has 3 reasons to change.

```csharp
public class Employee
{
    public string Name { get; set; }

    // Reason to change 1: Business Logic changes
    public bool Promote() 
    { 
        // ... logic
    }

    // Reason to change 2: Database Schema changes
    public void Save()
    {
        // ... SQL code
    }

    // Reason to change 3: Tax Law changes
    public decimal CalculateTax()
    {
        // ... tax math
    }
}
```

### Solution (Separated Responsibilities)
Split into focused classes.

```csharp
// 1. Domain Entity (Data Only or pure business logic)
public class Employee
{
    public string Name { get; set; }
}

// 2. Persistence Responsibility (Repository)
public class EmployeeRepository
{
    public void Save(Employee e) { /* SQL */ }
}

// 3. Tax Responsibility (Service)
public class TaxCalculator
{
    public decimal Calculate(Employee e) { /* Math */ }
}
```
Now, if the DBA changes the schema, only `EmployeeRepository` changes. `TaxCalculator` is untouched.
