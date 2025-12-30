# API Versioning Strategies

When your API changes (breaking changes), you need versioning.

## 1. URI Path Versioning (Most Common)
Include the version in the URL.
-   **Format**: `GET /api/v1/products`
-   **Pros**: Explicit, easy to see in logs/browser, easy to route.
-   **Cons**: "Pollutes" the URI resource (technically `v1/product` and `v2/product` are the same resource identity).

## 2. Header Versioning (RESTful)
Use a custom header.
-   **Format**: `X-Api-Version: 1`
-   **Pros**: URIs stay clean (`GET /api/products`).
-   **Cons**: Harder to test in browser (need a tool like Postman to set headers).

## 3. Query String Versioning
-   **Format**: `GET /api/products?v=1`
-   **Pros**: Easy to test.
-   **Cons**: Can interfere with caching keys if not handled correctly.

## 4. Media Type Versioning (Content Negotiation)
The most "Pure REST" way. The client asks for a specific representation.
-   **Format**: `Accept: application/vnd.mycompany.v1+json`
-   **Pros**: Elegant. Supports different versions for Request vs Response.
-   **Cons**: Complex to implement and document.

## Recommendation
For most Web APIs (Internal or Public): **URI Path Versioning (`/v1/`)**.
-   It is the most pragmatic. Developers understand it immediately.
-   The theoretical "REST violation" is rarely a practical issue.
