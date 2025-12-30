# Composition Over Inheritance

## Concept
**Composition Over Inheritance** suggests that it is better to build objects by composing them from other objects (has-a relationship) rather than by inheriting from them (is-a relationship).

> *"Favor object composition over class inheritance."* — *Design Patterns (GoF)*

## The Problem with Inheritance
Inheritance is the tightest coupling available in OO languages.
1.  **Fragile Base Class Problem**: Changes in the parent class can break child classes in unexpected ways.
2.  **Inflexibility**: Inheritance hierarchies are static. You cannot change a parent class at runtime.
3.  **Exploding Hierarchies**: If you have `Animal` -> `Bird` -> `Sparrow`, what happens when you need a `PlasticBird` (Toy)? It's a bird, but it can't eat or fly. You end up with empty overrides or exceptions.

## Example

### Violation (Inheritance Abuse)
We want to share code, so we use inheritance. But now `Robot` is forced to have `Eat()`.

```csharp
public class Worker
{
    public void Work() { /*...*/ }
    public void Eat() { /*...*/ }
}

// A Robot works, but it doesn't Eat!
// We are forced to inherit behavior we don't need.
public class Robot : Worker
{
    // ...
}
```

### Solution (Composition)
Isolate behaviors into their own components/interfaces and compose them.

```csharp
public interface IWorkable { void Work(); }
public interface IFeedable { void Eat(); }

public class Human : IWorkable, IFeedable
{
    public void Work() { /*...*/ }
    public void Eat() { /*...*/ }
}

public class Robot : IWorkable
{
    public void Work() { /*...*/ }
    // No Eat() method here. Clean.
}
```

## When to use Inheritance?
Inheritance is still useful for **"is-a"** relationships where the subclass is a *true subtype* (Liskov Substitution Principle).
-   Use Inheritance for **Type Hierarchy** (e.g., `Dog` is an `Animal`).
-   Use Composition for **Behavior Sharing** (e.g., `Dog` has a `Runner` behavior).
