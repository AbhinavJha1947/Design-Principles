# Composition

## Concept
**Composition** is a strong form of Aggregation representing a "Part-Of" relationship with **Strong Ownership**.

-   **Relationship**: "Part-Of" (Whole-Part).
-   **Lifecycle**: Dependent. If the "Whole" is destroyed, the "Part" is also destroyed. The Part cannot exist without the Whole.

## Analogy
**House and Room**.
A House is composed of Rooms. You cannot have a Room floating in space without a House. If check demolish the House, the Rooms are destroyed too.

## Code Example

In C#, Composition is modeled by ensuring the Parent creates the Child, or at least manages its disposal strictly.

```csharp
public class Room
{
    public string Name { get; set; }
}

public class House
{
    private List<Room> _rooms = new List<Room>();

    public House()
    {
        // Composition: The House CREATES the Rooms.
        // They are born with the House.
        _rooms.Add(new Room { Name = "Bedroom" });
        _rooms.Add(new Room { Name = "Kitchen" });
    }

    public void Demolish()
    {
        _rooms.Clear(); // Destruction of House implies destruction of Rooms
        _rooms = null;
    }
}
```
*Note: In Database terms, this is a Cascade Delete.*
