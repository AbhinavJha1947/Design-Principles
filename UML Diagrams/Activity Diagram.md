# Activity Diagram

## Type: Behavioral
Shows the flow of control or data from activity to activity. Similar to a **Flowchart**.

## Usage
-   Modeling complex logic (if/else loops).
-   Modeling business workflows (e.g., Order Processing).
-   Supports **concurrency** (fork/join).

## Example (Login Flow)

```mermaid
graph TD
    Start((Start)) --> Input[Input Credentials]
    Input --> Validate{Valid?}
    
    Validate -->|No| Error[Show Error]
    Error --> Input
    
    Validate -->|Yes| Login[Log User In]
    Login --> Load[Load Dashboard]
    Load --> Stop((Stop))
```
