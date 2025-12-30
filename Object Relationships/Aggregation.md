# Aggregation

## Concept
**Aggregation** is a specific type of Association representing a "Has-A" relationship, but with **Weak Ownership**.

-   **Relationship**: "Has-A" (Whole-Part).
-   **Lifecycle**: Independent. Using the "Whole" does not destroy the "Part". The "Part" can exist without the "Whole".
-   **Direction**: Unidirectional (usually).

## Analogy
**Department and Professor**.
A Department *has* Professors. But if the Department closes (is destroyed), the Professors don't die. They still exist and can join another department.

## Code Example

In C#, Aggregation is usually modeled by passing the child object into the parent's constructor or method, rather than creating it inside.

```csharp
public class Professor
{
    public string Name { get; set; }
}

public class Department
{
    private List<Professor> _professors = new List<Professor>();

    // Aggregation: Professors are created OUTSIDE and passed IN.
    // The Department does not own their lifecycle.
    public void AddProfessor(Professor p)
    {
        _professors.Add(p);
    }
}

// Usage
var profSmith = new Professor { Name = "Smith" }; // Created independently

var mathDept = new Department();
mathDept.AddProfessor(profSmith); 

// If mathDept is destroyed here, profSmith still exists in memory.
mathDept = null; 
Console.WriteLine(profSmith.Name); // Works!
```
