# Struct vs Class

## Key Difference: Value Type vs Reference Type

### Class (Reference Type)
-   **Allocation**: Heap.
-   **Passing**: Passed by reference (pointer copy).
-   **Equality**: Reference equality (are they the exact same object?) by default.
-   **Lifetime**: Managed by Garbage Collector (GC).

### Struct (Value Type)
-   **Allocation**: Stack (mostly) or inline in other types.
-   **Passing**: Passed by value (full copy).
-   **Equality**: Value equality (are all fields equal?).
-   **Lifetime**: Released immediately when out of scope.

## When to use Struct?
**"Is it a small data holder that represents a single value?"**

Use a struct **ONLY** if:
1.  It logically represents a single value, similar to a primitive (`int`, `double`).
2.  It is **Immutable** (readonly).
3.  It has a small instance size (< 16 bytes is the general rule of thumb).
4.  It will not be boxed frequently.

### Examples of Good Structs
-   `DateTime`
-   `Guid`
-   `Point(x, y)`
-   `ComplexNumber`

## The Mutable Struct Evil
**NEVER** create a mutable struct. It leads to extremely confusing bugs because copies effectively "lose" their connection to the original.

```csharp
// BAD
public struct MutablePoint
{
    public int X;
    public int Y;
}

// ...
var list = new List<MutablePoint> { new MutablePoint { X = 1, Y = 1 } };
// This does NOTHING because list[0] returns a COPY.
list[0].X = 100; 
// list[0].X is still 1.
```
