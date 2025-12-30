# Middleware vs Filters (ASP.NET Core)

Both intercept requests, but they operate at different levels of the pipeline.

## Middleware
-   **Scope**: Global. Runs on **every** request (even for static files, 404s).
-   **Context**: Has access to `HttpContext`, but **not** MVC context (Action, Route Data, ModelState).
-   **Use Cases**:
    -   Global Exception Handling
    -   Authentication (checking tokens before MVC kicks in)
    -   Logging Request/Response (Serilog)
    -   CORS, Compression

## Filters
-   **Scope**: MVC Specific. Runs only for Controller Actions.
-   **Context**: Has access to `ActionContext`, `ModelState`, Arguments.
-   **Use Cases**:
    -   Validation (`ModelState.IsValid`)
    -   Caching (Response Caching)
    -   Authorization (Granular policies per Controller)

## Analogy
-   **Middleware**: The Security Guard at the building entrance. He checks everyone. He doesn't know which office you are going to.
-   **Filter**: The Receptionist on the 5th floor. She knows specifically who you are meeting and can check if you have an appointment (ModelState).

## Example Pipeline
1.  **Middleware**: `ExceptionMiddleware` (Catches everything)
2.  **Middleware**: `AuthenticationMiddleware` (Who are you?)
3.  **Middleware**: `RoutingMiddleware` (Where are you going?)
4.  -- MVC Starts --
5.  **Filter**: `ResourceFilter` (Cache hit?)
6.  **Filter**: `ActionFilter` (Before: Validation) -> **Action Method** -> (After: Modify Result)
