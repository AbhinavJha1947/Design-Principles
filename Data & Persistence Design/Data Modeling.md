# Data Modeling

## 1. Entities
An **Entity** is an object defined primarily by its **identity**, not its attributes. Even if all its properties change, it remains the same object if its ID is the same.

-   **Identity**: Must have a unique identifier (ID), such as a GUID, Integer, or natural key (e.g., SSN, Email - though mutable natural keys are risky).
-   **Mutability**: Entities are typically mutable. Their state can change over time (e.g., a `User` entity changes their address or name).
-   **Equality**: Two entities are equal if their IDs are equal, even if they have different data.

*Example*: A `Customer` is an entity. Two customers with the name "John Doe" are different people if they have different CustomerIDs.

## 2. Value Objects
A **Value Object** is defined by its **attributes** (values), not an identity. It describes a characteristic of a thing.

-   **No Identity**: It doesn't have an ID.
-   **Immutability**: Once created, it cannot be changed. To "change" a value, you replace the entire object with a new one.
-   **Structural Equality**: Two value objects are equal if all their properties are equal.

*Example*: `Address` or `Money`. 
-   If you have a $5 bill and I have a $5 bill, they are effectively the same value ($5). We don't care about the serial number of the bill in most contexts.
-   `new Money(50, "USD")` should be equal to another `new Money(50, "USD")`.

## 3. Aggregate Roots (DDD-lite)
An **Aggregate** is a cluster of associated objects (Entities and Value Objects) that are treated as a unit for the purpose of data changes. The **Aggregate Root** is the main entity that holds the cluster together.

-   **Consistency Boundary**: The Aggregate Root is responsible for enforcing invariants (business rules) for the entire group. You never modify the internals of an aggregate directly; you go through the Root.
-   **Single Facade**: External objects can only hold references to the Aggregate Root, not to internal entities.
-   **Transactional Unit**: An aggregate should be saved or loaded from the database as a whole.

*Example*: `Order` is an Aggregate Root. `OrderItem` is an entity inside it.
-   *Rule*: You cannot add an `OrderItem` directly to the database. You must call `Properties` on the `Order` object (`order.AddItem(...)`), which ensures the total price is recalculated and rules are met.
-   *Persistence*: You save the `Order`, and the repository handles saving the `OrderItems` transactionally.
