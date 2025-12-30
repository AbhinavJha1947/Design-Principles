# Reuse Release Equivalence Principle (REP)

## Definition
> *"The granule of reuse is the granule of release."*

## Concept
You cannot reuse a component (package) unless it is managed by a release system (Versioning).

-   **Reuse**: Using code without copying it (Dependency).
-   **Release**: Assigning a version number (v1.0.0) and tracking changes.

## Implication
-   If you want to reuse a library, it MUST have a release number.
-   You cannot expect developers to link to your "live" source code. They need a stable version.
-   Classes that are grouped together in a package must share the same Version Number and Release Cycle.

## Summary of Cohesion Principles
-   **REP**: Group for reuse/release.
-   **CCP**: Group for maintenance (change together).
-   **CRP**: Split to avoid unneeded deps.
*Note: These pull in different directions. REP+CCP make components larger. CRP makes them smaller. Architecture is balancing these tensions.*
