# Design Principles & Architecture Patterns for C#/.NET

This repository contains a comprehensive collection of software design principles, architectural patterns, and best practices, tailored for .NET developers.

## 📚 Table of Contents

### 1. Core Design Principles
Foundational rules for writing clean, maintainable code.
- **[DRY (Don’t Repeat Yourself)](./Core%20Design%20Principles/DRY%20(Don’t%20Repeat%20Yourself).md)**: Single source of truth.
- **[KISS (Keep It Simple, Stupid)](./Core%20Design%20Principles/KISS%20(Keep%20It%20Simple,%20Stupid).md)**: Avoid unnecessary complexity.
- **[High Cohesion & Low Coupling](./Core%20Design%20Principles/High%20Cohesion%20&%20Low%20Coupling.md)**: The goal of modular design.
- **[Composition Over Inheritance](./Core%20Design%20Principles/Composition%20Over%20Inheritance.md)**: Flexible "Has-A" vs Rigid "Is-A".
- **[Encapsulate What Varies](./Core%20Design%20Principles/Encapsulate%20What%20Varies.md)**: Isolate change.
- **[Program to Interfaces, Not Implementations](./Core%20Design%20Principles/Program%20to%20Interfaces,%20Not%20Implementations.md)**: Decoupling via abstraction.
- **[Separation of Concerns (SoC)](./Core%20Design%20Principles/Separation%20of%20Concerns%20(SoC).md)**: Organization by responsibility.
- **[Law of Demeter (Least Knowledge)](./Core%20Design%20Principles/Law%20of%20Demeter%20(Least%20Knowledge).md)**: "Don't talk to strangers".
- **[Principle of Least Astonishment (POLA)](./Core%20Design%20Principles/Principle%20of%20Least%20Astonishment%20(POLA).md)**: Be intuitive.
- **[YAGNI (You Aren’t Gonna Need It)](./Core%20Design%20Principles/YAGNI%20(You%20Aren’t%20Gonna%20Need%20It).md)**: Avoid premature optimization/features.

### 2. SOLID Principles
The five pillars of OOD (Object-Oriented Design).
- **[S - Single Responsibility Principle (SRP)](./SOLID%20Principles/Single%20Responsibility%20Principle%20(SRP).md)**
- **[O - Open-Closed Principle (OCP)](./SOLID%20Principles/Open-Closed%20Principle%20(OCP).md)**
- **[L - Liskov Substitution Principle (LSP)](./SOLID%20Principles/Liskov%20Substitution%20Principle%20(LSP).md)**
- **[I - Interface Segregation Principle (ISP)](./SOLID%20Principles/Interface%20Segregation%20Principle%20(ISP).md)**
- **[D - Dependency Inversion Principle (DIP)](./SOLID%20Principles/Dependency%20Inversion%20Principle%20(DIP).md)**

### 3. Dotnet Specific
Patterns and idioms specific to the .NET ecosystem.
- **[Async-await design](./Dotnet%20Specific/Async-await%20design.md)**: Best practices for TPL.
- **[Dependency Injection containers](./Dotnet%20Specific/Dependency%20Injection%20containers.md)**: Lifetimes (Transient, Scoped, Singleton) & Pitfalls.
- **[Middleware vs Filters](./Dotnet%20Specific/Middleware%20vs%20Filters.md)**: ASP.NET Core pipeline components.
- **[Struct vs Class](./Dotnet%20Specific/Struct%20vs%20Class.md)**: Memory allocation (Stack vs Heap).
- **[Records vs Classes](./Dotnet%20Specific/Records%20vs%20Classes.md)**: Immutability and Value Semantics.
- **[Interfaces vs Abstract Classes](./Dotnet%20Specific/Interfaces%20vs%20abstract%20classes%20in%20C%23.md)**: When to use which.

### 4. Data & Persistence Design
- **[Data Modeling](./Data%20&%20Persistence%20Design/Data%20Modeling.md)**: Entities, Value Objects, Aggregates.
- **[ORM Decisions](./Data%20&%20Persistence%20Design/ORM%20Decisions.md)**: Lazy/Eager loading, N+1 Problem.
- **[Repository Pattern](./Data%20&%20Persistence%20Design/Repository%20Pattern.md)**: Repository & Unit of Work.
- **[CQRS](./Data%20&%20Persistence%20Design/CQRS.md)**: Command Query Responsibility Segregation.
- **[Soft Deletes](./Data%20&%20Persistence%20Design/Soft%20Deletes.md)**: Handling logical deletion.

### 5. API & Interface Design
- **[API Design](./API%20&%20Interface%20Design/API%20Design.md)**: REST, Idempotency, Request/Response models.
- **[Interface Design](./API%20&%20Interface%20Design/Interface%20Design.md)**: Role Interfaces, Explicit Implementation.
- **[API Versioning Strategies](./API%20&%20Interface%20Design/API%20Versioning%20Strategies.md)**: Methods to handle breaking changes.
- **[Fluent Interfaces](./API%20&%20Interface%20Design/Fluent%20Interfaces.md)**: Method chaining patterns.

### 6. Control & Dependency Management
- **[Inversion of Control (IoC)](./Control%20&%20Dependency%20Management/Inversion%20of%20Control%20(IoC).md)**: The Hollywood Principle.
- **[DI vs Service Locator](./Control%20&%20Dependency%20Management/DI%20vs%20Service%20Locator.md)**: Why DI is preferred.

### 7. Object Relationships
- **[Association](./Object%20Relationships/Association.md)** ("Knows-A")
- **[Aggregation](./Object%20Relationships/Aggregation.md)** ("Has-A")
- **[Composition](./Object%20Relationships/Composition.md)** ("Part-Of")
- **[Dependency](./Object%20Relationships/Dependency.md)** ("Uses-A")
- **[Cardinality](./Object%20Relationships/Cardinality%20(1-1,%201-many,%20many-many).md)** (1:1, 1:N, N:N)

### 8. Package / Module Principles
Based on Robert C. Martin's package principles.
- **Cohesion**: [CCP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Common%20Closure%20Principle%20(CCP).md), [CRP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Common%20Reuse%20Principle%20(CRP).md), [REP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Reuse%20Release%20Equivalence%20Principle%20(REP).md).
- **Coupling**: [ADP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Acyclic%20Dependencies%20Principle%20(ADP).md), [SDP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Stable%20Dependencies%20Principle%20(SDP).md), [SAP](./Package%20%20Module%20Design%20Principles%20(Robert%20C.%20Martin)/Stable%20Abstractions%20Principle%20(SAP).md).

### 9. UML Diagrams
Guides with Mermaid integration.
- **Behavioral**: [Sequence](./UML%20Diagrams/Sequence%20Diagram.md), [Activity](./UML%20Diagrams/Activity%20Diagram.md), [State](./UML%20Diagrams/State%20Diagram.md).
- **Structural**: [Class](./UML%20Diagrams/Class%20Diagram.md), [Object](./UML%20Diagrams/Object%20Diagram.md), [Component](./UML%20Diagrams/Component%20Diagram.md).

---
*Created by Abhinav Jha*
