# Interfaces vs Abstract Classes

## Quick Comparison

| Feature | Interface | Abstract Class |
| :--- | :--- | :--- |
| **Members** | Only signatures (mostly). No fields. | Can have fields, constructors, full methods. |
| **Inheritance** | Multiple Implementation allowed. | Single Inheritance only. |
| **Access Modifiers** | Public by default. | Can have protected/private members. |
| **Purpose** | "Can-Do" (Behavior / Contract). | "Is-A" (Hierarchy / Base). |

## When to use which?

### Use Abstract Class when:
1.  **Code Sharing**: You have logic that is identical across multiple derived classes (DRY).
2.  **Tight Relationship**: The derived classes are essentially the "same kind of thing" (e.g., `Bird` -> `Eagle`, `Penguin`).
3.  **Versioning**: Adding a method to an Abstract Class (with a default impl) allows you to upgrade it without breaking children. Adding a method to an interface breaks all implementers (unless you use Default Interface Methods).

### Use Interface when:
1.  **Unrelated Classes**: You want to provide common functionality to unrelated classes. (e.g., `IDisposable`, `IComparable` are implemented by totally different things).
2.  **Multiple Inheritance**: You need a class to implement multiple behaviors (e.g., `class SmartPhone : IPhone, ICamera, IGps`).
3.  **Mocking/Testing**: Interfaces are easier to mock in unit tests than concrete classes.

## Example

```csharp
// Abstract Class: Shared Logic
public abstract class Animal
{
    public void Sleep() { Console.WriteLine("Zzz..."); } // Shared
    public abstract void MakeSound(); // Forced override
}

// Interface: Capability
public interface IFlyable
{
    void Fly();
}

public class Duck : Animal, IFlyable
{
    public override void MakeSound() => Console.WriteLine("Quack");
    public void Fly() => Console.WriteLine("Flap");
}
```
