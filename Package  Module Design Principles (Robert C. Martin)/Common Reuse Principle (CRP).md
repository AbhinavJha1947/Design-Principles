# Common Reuse Principle (CRP)

## Definition
> *"Don't force users of a component to depend on things they don't need."*

## Concept
This is essentially the **Interface Segregation Principle (ISP)** applied at the Component/Package level.

-   **Goal**: Decoupling.
-   If Package A depends on Package B, then A depends on **every** class in B (even the ones it doesn't use).
-   If a class in B changes (even one that A doesn't care about), A might still need to be recompiled/redeployed.

## Application
Split "Fat" packages.
If you have a `Utils` package that contains `StringHelpers` and `DatabaseHelpers`, split them.
-   A UI Layer might need `StringHelpers` but shouldn't depend on `DatabaseHelpers`.
