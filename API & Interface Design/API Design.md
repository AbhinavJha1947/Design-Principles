# API Design

## 1. Method Signatures
Method signatures are the contract between your API and its consumers. A well-designed signature is self-documenting and easy to use.

-   **Explicit Intent**: Naming should clearly indicate the action (e.g., `RegisterUser` vs `ProcessData`).
-   **Limited Arguments**: Ideally, keep arguments under 3. If more are needed, group them into a parameter object or DTO.
-   **Strong Typing**: Use specific types (e.g., `UserId` struct/class) instead of primitives (`int`, `guid`) to prevent swapping parameters like `(userId, orderId)`.
-   **Async by Default**: For I/O bound operations, always use `async/await` (Task-based Asynchronous Pattern in C#) to ensure scalability.

## 2. Request/Response Models
Keep your API contract stable and decoupled from your internal domain model/database entities.

-   **DTOs (Data Transfer Objects)**: Always use DTOs for API input and output. Never expose Entity Framework entities directly.
    -   *Security*: Prevents Mass Assignment vulnerabilities.
    -   *Versioning*: Allows the internal domain to evolve without breaking external clients.
-   **Serialization**: Use standard naming conventions (e.g., `camelCase` for JSON responses) regardless of the server-side language casing (`PascalCase` in C#).

## 3. Validation Logic Placement
Validation ensures that the system only processes valid data.

-   **Syntactic Validation (Format)**: Place this at the **DTO/Entry level** (e.g., FluentValidation or Data Annotations).
    -   *Examples*: "Email is required", "Age must be > 0".
    -   *Goal*: Fail fast. Reject invalid requests before they reach the domain logic.
-   **Semantic Validation (Business Rules)**: Place this in the **Domain Model or Domain Service**.
    -   *Examples*: "User cannot register if email already exists", "Cannot withdraw more than balance".
    -   *Goal*: Maintain data consistency and invariants.

## 4. Error Handling Design
Consistent error responses build trust and make debugging easier for clients.

-   **Standard Format**: Use **Problem Details (RFC 7807)** structure.
    ```json
    {
      "type": "https://example.com/probs/out-of-credit",
      "title": "You do not have enough credit.",
      "status": 403,
      "detail": "Your current balance is 30, but that costs 50."
    }
    ```
-   **HTTP Status Codes**: Use generic codes correctly.
    -   `200 OK`: Success.
    -   `201 Created`: Resource created successfully.
    -   `400 Bad Request`: Client sent invalid data (Validation error).
    -   `401 Unauthorized`: Authentication required.
    -   `403 Forbidden`: Authenticated, but not allowed (Permissions).
    -   `404 Not Found`: Resource doesn't exist.
    -   `500 Internal Server Error`: Server blew up (generic fallback).

## 5. Idempotency
Idempotency means making multiple identical requests has the same effect as making a single request.

-   **Safe Methods**: `GET`, `HEAD`, `OPTIONS` should never change state.
-   **Idempotent Methods**: `PUT`, `DELETE` should result in the same state if called once or 10 times.
    -   *Example*: `DELETE /users/123`. If user exists, delete and return 204 or 200. If user is already deleted, return 200 or 204 (don't return 404/500 on the second call ideally, or at least handle it gracefully).
-   **Non-Idempotent Methods**: `POST` is usually not idempotent (creating a resource).
    -   *Strategy*: To make `POST` idempotent (e.g., for payment processing), require the client to send a unique `Idempotency-Key` header. The server tracks this key; if seen before, it returns the cached response instead of re-processing the payment.