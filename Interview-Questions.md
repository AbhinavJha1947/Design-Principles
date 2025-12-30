# Software Design & Architecture Interview Questions

A comprehensive list of interview questions based on the principles and patterns in this repository.

## Table of Contents

### Core Design Principles
1.  [Difference between Cohesion and Coupling](#1-difference-between-cohesion-and-coupling)
2.  [Explain Composition over Inheritance](#2-explain-composition-over-inheritance)
3.  [What is the Law of Demeter?](#3-what-is-the-law-of-demeter)
4.  [Explain DRY vs YAGNI](#4-explain-dry-vs-yagni)

### SOLID Principles
5.  [How does OCP help in maintenance?](#5-how-does-ocp-help-in-maintenance)
6.  [Give a real-world violation of LSP](#6-give-a-real-world-violation-of-lsp)
7.  [Difference between DIP and Dependency Injection](#7-difference-between-dip-and-dependency-injection)

### .NET Specific
8.  [Async/Await Best Practices & Pitfalls](#8-asyncawait-best-practices--pitfalls)
9.  [Transient vs Scoped vs Singleton Lifetimes](#9-transient-vs-scoped-vs-singleton-lifetimes)
10. [What is the Captive Dependency problem?](#10-what-is-the-captive-dependency-problem)
11. [Struct vs Class: When to use which?](#11-struct-vs-class-when-to-use-which)
12. [Middleware vs Filters in ASP.NET Core](#12-middleware-vs-filters-in-aspnet-core)

### Data & Persistence
13. [What is the N+1 Problem and how to fix it?](#13-what-is-the-n1-problem-and-how-to-fix-it)
14. [Lazy Loading vs Eager Loading](#14-lazy-loading-vs-eager-loading)
15. [Why shouldn't you return IQueryable from a Repository?](#15-why-shouldnt-you-return-iqueryable-from-a-repository)
16. [Explain CQRS](#16-explain-cqrs)

### Architecture & Design
17. [Common Closure Principle (CCP) vs Common Reuse Principle (CRP)](#17-common-closure-principle-ccp-vs-common-reuse-principle-crp)
18. [Service Locator vs Dependency Injection](#18-service-locator-vs-dependency-injection)
19. [API Versioning Strategies](#19-api-versioning-strategies)

---

## Answers

### 1. Difference between Cohesion and Coupling
**Cohesion** refers to how arguably related the responsibilities of a single module/class are. We want **High Cohesion** (Doing one thing well).
**Coupling** refers to how dependent modules are on each other. We want **Low Coupling** (Independence).
*High Cohesion usually leads to Low Coupling.*

[Back to Top](#table-of-contents)

### 2. Explain Composition over Inheritance
Inheritance ("Is-A") creates valid tight coupling. Changes in the base class affect all children. It is rigid.
Composition ("Has-A") assembles behavior using smaller components. It is flexible and allows changing behavior at runtime.
*Example: Instead of `Robot` inheriting `Worker`, `Robot` should have a `WorkModule` component.*

[Back to Top](#table-of-contents)

### 3. What is the Law of Demeter?
Also known as the "Principle of Least Knowledge". A unit should only talk to its immediate friends and not talk to strangers.
*Violation*: `order.Customer.Address.City` (Train Wreck).
*Fix*: `order.GetCustomerCity()` or `customer.GetCity()`.

[Back to Top](#table-of-contents)

### 4. Explain DRY vs YAGNI
-   **DRY (Don't Repeat Yourself)**: Avoid duplication of knowledge/logic.
-   **YAGNI (You Aren't Gonna Need It)**: Don't implement features you "might" need in the future. Just implement what is needed now.

[Back to Top](#table-of-contents)

### 5. How does OCP help in maintenance?
**Open-Closed Principle**: Open for extension, Closed for modification.
It reduces the risk of breaking existing bugs. Instead of modifying a huge `switch` statement to add a new case (risk), you create a new class that implements an interface (safe).

[Back to Top](#table-of-contents)

### 6. Give a real-world violation of LSP
**Liskov Substitution Principle**: Subtypes must be substitutable for base types.
*Violation*: The `Square` inheriting from `Rectangle` problem. Setting `Width` on a Square unexpectedly changes `Height`, breaking the assumption of the Rectangle user.
*Violation 2*: Throwing `NotImplementedException` in an overridden method.

[Back to Top](#table-of-contents)

### 7. Difference between DIP and Dependency Injection
-   **DIP (Principle)**: High-level modules should not depend on low-level modules; both should depend on abstractions.
-   **DI (Pattern)**: A technique to implement DIP by injecting dependencies (usually via Constructor) rather than creating them inside.

[Back to Top](#table-of-contents)

### 8. Async/Await Best Practices & Pitfalls
-   **Pitfall**: `Async Void` (Crashes process on error, effectively fire-and-forget). Only use for Event Handlers.
-   **Pitfall**: `.Result` or `.Wait()` (Causes Deadlocks).
-   **Best Practice**: Async "Turtle" (All the way down). Return `Task`.

[Back to Top](#table-of-contents)

### 9. Transient vs Scoped vs Singleton Lifetimes
-   **Transient**: New instance every time. (Lightweight, stateless).
-   **Scoped**: New instance per HTTP Request. (DbContext, unit of work).
-   **Singleton**: Single instance for app life. (Cache, Config).

[Back to Top](#table-of-contents)

### 10. What is the Captive Dependency problem?
When a service with a longer lifetime (e.g., Singleton) holds a reference to a service with a shorter lifetime (e.g., Scoped).
The Scoped service becomes "trapped" and lives as long as the Singleton, potentially causing memory leaks or using stale data (like a closed DbContext).

[Back to Top](#table-of-contents)

### 11. Struct vs Class: When to use which?
-   **Struct (Value Type)**: Stack allocated. Copy by value. Use for small, immutable data like `Point`, `Coordinate`.
-   **Class (Reference Type)**: Heap allocated. Copy by reference. Use for almost everything else (Entities, Services).
*Warning*: Mutable structs are evil.

[Back to Top](#table-of-contents)

### 12. Middleware vs Filters in ASP.NET Core
-   **Middleware**: Runs on the global pipeline. Good for cross-cutting concerns like Headers, Logging, Auth. No MVC context.
-   **Filters**: Runs within the MVC pipeline (Controller/Action). Good for ModelState validation, Caching specific views.

[Back to Top](#table-of-contents)

### 13. What is the N+1 Problem and how to fix it?
Executing 1 query to get N items, and then N loop queries to get related data for each item.
*Fix*: Eager Loading (Using `.Include()` in EF Core) to fetch everything in 1 JOIN query.

[Back to Top](#table-of-contents)

### 14. Lazy Loading vs Eager Loading
-   **Lazy**: Load data on-demand when property is accessed. Convenient but risky (Performance).
-   **Eager**: Load data upfront. Efficient (fewer roundtrips) but can fetch too much data.

[Back to Top](#table-of-contents)

### 15. Why shouldn't you return IQueryable from a Repository?
It leaks database concerns to the calling layer. The caller might add `.Where(x => x.CustomMethod())` which forces local evaluation or runtime errors if SQL cannot translate it. Repositories should return specific results (`List`, `IEnumerable`, `Specification`).

[Back to Top](#table-of-contents)

### 16. Explain CQRS
**Command Query Responsibility Segregation**.
separating the Read model (Queries, DTOs) from the Write model (Commands, Domain Entities). Allows optimizing them independently (e.g., fast reads via Dapper, complex writes via DDD/EF Core).

[Back to Top](#table-of-contents)

### 17. Common Closure Principle (CCP) vs Common Reuse Principle (CRP)
-   **CCP**: Group classes that change together. (Tendency to make packages larger).
-   **CRP**: Split classes so users don't depend on what they don't use. (Tendency to make packages smaller).
*They are in tension with each other.*

[Back to Top](#table-of-contents)

### 18. Service Locator vs Dependency Injection
-   **Service Locator**: Asking a global static object for dependencies. Hides dependencies, hard to test. (Anti-pattern).
-   **DI**: Declaring dependencies in constructor. Explicit, easy to test.

[Back to Top](#table-of-contents)

### 19. API Versioning Strategies
-   **URI Path**: `/v1/users` (Simplest, widely supported).
-   **Header**: `X-Version: 1` (Clean URL, harder to test in browser).
-   **Query String**: `?v=1`.

[Back to Top](#table-of-contents)

### 20. How does the Stable Dependencies Principle affect CI/CD?
If high-level modules (Business Logic) depend on unstable low-level modules (UI/DB), any change to the unstable module forces a rebuild/retest of the stable module.
In CI/CD, this means your build times increase because cache invalidation cascades upwards. By adhering to SDP (Stable packages depend only on more stable packages), you ensure that changes to volatile components (UI) don't trigger rebuilding the entire Core domain.

[Back to Top](#table-of-contents)

### 21. Design a resilient system using Circuit Breakers and Retry patterns
-   **Retry**: Use for transient failures (Network blip). Key: Use **Exponential Backoff** to avoid hammering a struggling service.
-   **Circuit Breaker**: Use for persistent failures. If a service fails X times, "Open" the circuit and fail fast immediately without calling the service. After a timeout, "Half-Open" to test if it's back.
-   **Trade-off**: Retry increases latency. Circuit Breaker reduces latency during outages but requires fallback logic.

[Back to Top](#table-of-contents)

### 22. Value Objects vs Entities in High-Performance Scenarios
-   **Entities** (Classes) allow identity tracking but cause Heap Allocations and GC pressure.
-   **Value Objects** (Structs/Records) are immutable and can be Stack Allocated (if Structs).
-   *Scenario*: Processing 1M GPS points/sec. Using `class Point` creates 1M objects for GC to collect. Using `struct Point` (Value Object) keeps data on stack/inline, zero GC pressure.
-   *Caveat*: Large structs (>16 bytes) are expensive to copy. Use `ref` passing.

[Back to Top](#table-of-contents)

### 23. Handling Distributed Transactions (Saga Pattern)
In Microservices, you cannot use ACID transactions across databases (Two-Phase Commit is slow/locking).
**Saga Pattern**: A sequence of local transactions.
-   *Service A* commits -> publishes event -> *Service B* listens and commits.
-   **Compensation**: If Service B fails, it publishes a failure event. Service A listens and executes a "Compensation" (Undo) transaction to revert its change.
-   *Trade-off*: Eventual Consistency vs Strong Consistency.

[Back to Top](#table-of-contents)

### 24. Critique the Repository Pattern with EF Core
**Argument For**: Decoupling, Testability (Mocking).
**Argument Against**: EF Core `DbContext` *is* already a Repository (Unit of Work). Creating `GenericRepository<T>` on top of `DbSet<T>` is often an unnecessary abstraction layer that hides features (like `.Include`, `AsSplitQuery`) and leads to "Leaky Abstractions" (returning IQueryable).
*Senior View*: Use Repositories for specific Complex Aggregates (Root), but don't wrap every table in a Generic Repo.

[Back to Top](#table-of-contents)
