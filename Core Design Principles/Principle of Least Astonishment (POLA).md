# Principle of Least Astonishment (POLA)

## Concept
The **Principle of Least Astonishment** (also known as the Principle of Least Surprise) states that a component of a system should behave in a way that most users will expect it to behave. The design should match the user's mental model.

In the context of API and code design:
> *"If a necessary feature has a high astonishment factor, it may be necessary to redesign the feature."*

## Why it Matters
When code behaves surprisingly:
1.  **Bugs**: Developers make assumptions based on common patterns. If your code breaks those patterns, bugs occur.
2.  **Learning Curve**: It takes longer for new developers to understand the system.
3.  **Misuse**: APIs will be used incorrectly, leading to data corruption or performance issues.

## Common Violations & Fixes

### 1. Getters with Side Effects
**Violation**: A property getter (e.g., `user.Name`) that triggers a database call or modifies state.
**Expectation**: Getters are cheap, fast, and safe to call repeatedly.
**Fix**: If it's expensive, make it a method like `user.FetchNameFromDb()`.

### 2. Misleading Method Names
**Violation**: A method called `ValidateUser()` that also sends a welcome email and saves the user to the DB.
**Expectation**: Validation should only check validity and return a result without side effects (commands vs queries).
**Fix**: Rename to `RegisterUser()` or separate the concerns.

### 3. Inconsistent Return Values
**Violation**: A method returns `null` in some error cases, throws an `Exception` in others, and returns an empty list in others.
**Expectation**: Consistency.
**Fix**:
-   Return empty collections (not null) for lists.
-   Throw exceptions for exceptional/unexpected failures.
-   Use `Option`/`Result` types for expected failures.

### 4. Magic Casting
**Violation**: Defining an operator overload that silently casts a complex object to a boolean based on an obscure internal flag.
**Fix**: Be explicit. Use `.IsValid()` instead of implicit boolean evaluation.

## Summary
Design your interfaces so that the **obvious** way to use them is also the **correct** way. If a developer guesses how your API works, they should guess right.
