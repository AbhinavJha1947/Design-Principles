# YAGNI (You Aren’t Gonna Need It)

## Concept
**YAGNI** stands for **"You Aren't Gonna Need It"**. It is a principle of Extreme Programming (XP) that states a programmer should not add functionality until deemed necessary.

> *"Always implement things when you actually need them, never when you just foresee that you need them."* — Ron Jeffries

## The Trap of Anticipation
Developers often try to predict future requirements:
- "What if we need to support multiple database types later?"
- "Let's make this class generic just in case."
- "I'll add this extra verification method now so we don't have to later."

**The Reality**:
1.  **Wrong Predictions**: Requirements change. The "future case" you built for often never happens, or happens differently than you expected.
2.  **Cost of Carry**: Unused code must be maintained, tested, and understood by other developers. It adds noise and complexity to the codebase.
3.  **Opportunity Cost**: Time spent building "what if" features is time *not* spent building features the users actually need right now.

## Benefits of YAGNI
1.  **Simpler Code**: Less code means fewer bugs and easier understanding.
2.  **Faster Development**: You focus strictly on the current requirements.
3.  **Easier Refactoring**: It's easier to change a simple system than a complex one laden with speculative abstraction hooks.

## YAGNI vs. Scalability
YAGNI does **not** mean writing bad code or ignoring architectural best practices.
-   **Good**: Writing clean, modular code that is *easy to change* later if requirements grow.
-   **Bad/YAGNI Violation**: Building a complex plugin infrastructure for a system that currently only needs one hardcoded behavior.

## Example
**Scenario**: You are writing a logging service.
-   **YAGNI Approach**: Write a simple logger that writes to a file, because that's what is required today.
-   **Violation**: Create an `ILogger` interface, an `AbstractLogFactory`, and concrete implementations for `FileLogger`, `DatabaseLogger`, and `EmailLogger`, "just in case" we need them later.

**Rule of Thumb**: Wait until the second or third time you need to do something similar before you generalize it (The Rule of Three).
