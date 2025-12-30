# Association

## Concept
**Association** is the most general relationship. It simply means "Knows-A" or "Works-With". It defines a link between two classes where one instance interacts with another instance.

-   **Relationship**: "Knows-A".
-   **Lifecycle**: Completely Independent.
-   **Cardinality**: Can be 1-to-1, 1-to-Many, Many-to-Many.

## Analogy
**Teacher and Student**.
A Teacher teaches a Student. A Student learns from a Teacher. They interact, but they don't own each other.

## Code Example

```csharp
public class Student
{
    public string Name { get; set; }
}

public class Teacher
{
    public string Name { get; set; }

    // Association: The Teacher "knows" about the Student temporarily 
    // or maintains a reference, but there is no ownership.
    public void Teach(Student s)
    {
        Console.WriteLine($"{this.Name} is teaching {s.Name}");
    }
}
```

## Difference from Dependency
-   In **Dependency**, the link is usually just a method parameter (very temporary).
-   In **Association**, the link *could* be a field reference (longer lived), but implies no ownership.
