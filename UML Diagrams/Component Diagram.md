# Component Diagram

## Type: Structural
Shows the organization and dependencies among a set of components.
A **Component** is a modular part of a system that encapsulates its contents and whose manifestation is replaceable within its environment (e.g., a DLL, a Web Service, a Database).

## Usage
-   High-Level Architecture views.
-   Showing how Microservices talk to each other.
-   Showing physical dependencies (e.g., this App needs this DLL).

## Example

```mermaid
classDiagram
    component [Web App]
    component [Auth Service]
    component [Payment Gateway]
    component [User DB]

    [Web App] ..> [Auth Service] : HTTP/REST
    [Web App] ..> [Payment Gateway] : gRPC
    [Auth Service] --> [User DB] : TCP/SQL
```

*(Note: Mermaid syntax for Component diagrams often reuses class diagram syntax or graph syntax depending on version, but the logic remains: high-level blocks).*
