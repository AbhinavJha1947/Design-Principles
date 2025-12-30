# Acyclic Dependencies Principle (ADP)

## Definition
> *"Allow no cycles in the component dependency graph."*

## The Problem (Cycle)
Package A -> Package B -> Package C -> Package A.

-   **Build Hell**: You can't build A until B is built. You can't build B until C is built. You can't build C until A is built.
-   **Testing**: You can't test A in isolation. You have to test A+B+C together.
-   **Morning After Syndrome**: You go home, your code works. You come back, it's broken because someone changed C, which affected A.

## The Solution

### 1. Dependency Inversion (DIP)
Create an interface in Package A (or B) to invert the arrow.

### 2. Escalation
Create a new Package D. Move the classes that both A and C depend on into D.
A -> D
C -> D
Cycle broken.
