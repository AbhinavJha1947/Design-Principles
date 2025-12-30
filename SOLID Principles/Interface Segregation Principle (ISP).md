# Interface Segregation Principle (ISP)

## Definition
> *"Clients should not be forced to depend on interfaces they do not use."*

In simpler terms: **Many client-specific interfaces are better than one general-purpose interface.**

## The Problem: "Fat" Interfaces
When you create a large, "do-it-all" interface, you force implementing classes to define methods they don't need. This leads to:
1.  **Throwing NotSupportedException**: Implementing classes stub out methods with exceptions.
2.  **Unnecessary Coupling**: Changes to a method in the interface force *all* implementers to recompile/deploy, even those that don't use that method.

## Example: The "Fat" Interface (Violation)

Imagine a multifunction printer interface.

```csharp
// Violation: A simple printer is forced to implement Scan and Fax
public interface IMachine
{
    void Print(Document d);
    void Scan(Document d);
    void Fax(Document d);
}

public class OldFashionedPrinter : IMachine
{
    public void Print(Document d) 
    {
        // Actual printing logic
        Console.WriteLine("Printing...");
    }

    public void Scan(Document d)
    {
        throw new NotImplementedException("I can't scan!");
    }

    public void Fax(Document d)
    {
        throw new NotImplementedException("I can't fax!");
    }
}
```

## The Solution: Segregation

Break the interface down into smaller, role-specific interfaces.

```csharp
// 1. Break down capabilities
public interface IPrinter
{
    void Print(Document d);
}

public interface IScanner
{
    void Scan(Document d);
}

public interface IFax
{
    void Fax(Document d);
}

// 2. Compose them as needed
public class OldFashionedPrinter : IPrinter
{
    public void Print(Document d)
    {
        Console.WriteLine("Printing...");
    }
}

public class MultiFunctionMachine : IPrinter, IScanner, IFax
{
    public void Print(Document d) { /* ... */ }
    public void Scan(Document d)  { /* ... */ }
    public void Fax(Document d)   { /* ... */ }
}
```

## Benefits
-   **Decoupling**: Classes only know about methods that are relevant to them.
-   **Flexibility**: You can mix and match behavior by implementing different combinations of interfaces.
-   **Safety**: Compile-time safety prevents calling `Fax()` on a printer that doesn't support it.
