# Dependency

## Concept
**Dependency** is the weakest relationship. It means "Uses-A".
Class A depends on Class B if changes to Class B *might* require changes to Class A.

-   **Relationship**: "Uses-A".
-   **Lifecycle**: Temporary (Call duration).

## Occurs When:
1.  A receives B as a parameter in a method.
2.  A instantiates B locally in a method.
3.  A returns B from a method.

## Code Example

```csharp
public class Printer
{
    // Dependency: Printer merely "uses" the Document to do its job.
    // It doesn't store it as a field (Association/Aggregation).
    public void Print(Document doc)
    {
        Console.WriteLine(doc.Text);
    }
}

public class Document
{
    public string Text { get; set; }
}
```

## Summary of Relationships Strength
`Dependency` < `Association` < `Aggregation` < `Composition`
(Weakest) ----------------------------------> (Strongest)
