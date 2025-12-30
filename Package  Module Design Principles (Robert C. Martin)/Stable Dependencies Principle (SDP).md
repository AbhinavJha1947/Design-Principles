# Stable Dependencies Principle (SDP)

## Definition
> *"Depend in the direction of stability."*

## Concept
A module should not depend on a module that is *less* stable than itself.

-   **Stability**: Hard to change. (Not "working well", but "rigid").
-   **Instability (I)**: Metric from 0 to 1.
    -   `I = FanOut / (FanIn + FanOut)`
    -   `I = 0`: Maximally Stable (Many depend on it, it depends on nothing). Example: `System.String`.
    -   `I = 1`: Maximally Unstable (It depends on everything, nobody depends on it). Example: High-Level UI Logic.

## Rule
If **Package A** depends on **Package B**, then **B** must be more stable than **A**.
`I(B) < I(A)`

## Why?
If A is stable (hard to change) and it depends on B (unstable/volatile), then B's frequent changes will force difficult changes in A. You are making A unstable.
