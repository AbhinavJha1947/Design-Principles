# Inversion of Control (IoC)

## Concept
**Inversion of Control** is a design principle where the control of the program's execution flow is inverted. instead of the custom code calling the library, the library/framework calls the custom code.

> **"Don't call us, we'll call you."** — *The Hollywood Principle*

## Traditional vs Inverted Control

### Traditional (Procedural)
Your code is the boss. It instantiates dependencies, calls methods, and controls the timeline.
```csharp
// Program controls the flow
var data = ReadInput();
var processed = Process(data);
Save(processed);
```

### Inverted (IoC)
A framework (like ASP.NET Core or a UI framework) controls the flow. It calls your methods (event handlers, controllers) at specific times.

## Types of IoC

### 1. Dependency Injection (DI)
The most common form. Instead of a class creating its dependencies, they are "injected" from the outside.
-   **Control Inverted**: The class loses control over *creation*.
-   **Benefit**: Decoupling and Testability.

### 2. Events & Delegates
In a UI app, you don't write a loop checking for mouse clicks. You register an event handler. The Framework calls *you* when the click happens.
-   **Control Inverted**: The handler loses control over *when* it runs.

### 3. Template Method Pattern
A base class defines the skeleton of an algorithm but lets subclasses override specific steps.
-   **Control Inverted**: The interaction flow is fixed in the base class; the subclass just fills in the blanks.

## Summary
IoC is the broader principle. DI is one specific way to achieve it.
-   **IoC Container**: A tool (like `IServiceCollection` in .NET) that helps manage DI.
