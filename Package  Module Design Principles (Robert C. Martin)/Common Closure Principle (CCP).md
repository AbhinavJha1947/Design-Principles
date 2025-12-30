# Common Closure Principle (CCP)

## Definition
> *"The classes in a component should be closed together against the same kinds of changes. A change that affects a component affects all the classes in that component and no other components."*

## Concept
This is essentially the **Single Responsibility Principle (SRP)** applied at the Component/Package level.

-   **Goal**: Maintainability.
-   If you need to change code (e.g., changes to DB Schema), you ideally want to change just **one** package (dll/jar), recompile it, and redeploy it.
-   You do **not** want to hunt through 10 different packages to make one logical change.

## Rule of Thumb
Group classes together that are tightly coupled and likely to change together.

-   If two classes are always modified in the same commit, they belong in the same package.
