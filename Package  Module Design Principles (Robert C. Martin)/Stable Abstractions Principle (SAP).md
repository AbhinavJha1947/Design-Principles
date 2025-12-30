# Stable Abstractions Principle (SAP)

## Definition
> *"A component should be as abstract as it is stable."*

## Concept
This principle links Stability (SDP) with Abstractness (OCP).

-   **Stable** packages (used by many) should be **Abstact** (Interfaces/Abstract classes), so they can be extended without modification.
-   **Unstable** packages (concrete implementations) should be concrete, as they are easy to change.

## The Main Sequence
We plot components on a graph:
-   X-Axis: Instability (I)
-   Y-Axis: Abstractness (A)

**The Zone of Pain**: (I=0, A=0). Highly Stable + Highly Concrete. Hard to change, but can't be extended. Example: Database Schema (often).
**The Zone of Uselessness**: (I=1, A=1). Highly Unstable + Highly Abstract. Useless abstract classes that nobody implements or uses.

**The Golden Path**: Components should fall on the diagonal line connecting (I=1, A=0) and (I=0, A=1).
