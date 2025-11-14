#tech-notes 

## What happens when an error occurs?
- Execution stops immediately at the throw point (or auto-thrown bug like a TypeError).
- JS walks **up the call stack** looking for the nearest `try/catch`.
- If none is found, the program (or NestJS request) terminates → usually becomes a 500.
- The **first** catch it finds handles the error.

Example:
```
function a() { b(); }
function b() { c(); }
function c() { throw new Error("Boom"); }

try {
  a();
} catch (err) {
  console.log("Handled:", err.message);
}
```

#### When should you use throw?

- Use `throw` when your function cannot continue because a required condition is violated.
- The function stops right there.
- The _caller_ is responsible for deciding how to handle the failure.    
- This keeps your code layered and predictable.

**Functions validate and throw and Callers decide how to handle.**

## What is a Technical Error vs a Domain Error?

**Technical Error**
- Comes from libraries, DB, runtime, or low-level operations.
- Examples: Prisma P2002, Axios ECONNREFUSED, TypeError, JSON.parse() error.
- Too low-level to show to the client.

**Domain Error**
- Expresses a business rule failure in your system.
- Examples: DuplicateBookmarkError, UserAlreadyExistsError, InvalidInputError.
- Easy for controllers to convert into proper HTTP responses.

## Why rethrow errors?
Use rethrow when:
- You want to **add context** (which operation failed).
- You want to **translate** a technical error into a domain error.
- You want to let the **caller decide** the final action.

If a function cannot fully handle or fix the problem → rethrow.

## Error bubbling flow in layered architecture

Repository (technical) → Service (domain) → Controller (HTTP)

- Repository throws *technical* errors.
- Service may catch + translate to *domain* errors.
- Controller maps to `HttpException` (400, 403, 409, 500, etc).

## When NOT to use try/catch in service
- When you’re not translating anything.
- When you want the controller/global filter to handle all errors.
- When a technical error already has enough context in the stack trace.

## When service SHOULD catch
- To detect known DB errors (e.g., Prisma P2002).
- To throw domain errors like `DuplicateBookmarkError`.
- To add operation-specific details (userId, entityId).

## NestJS flow for controller errors
- Controller throws `HttpException`.
- Nest catches it via global exception filter.
- Response sent to the client.
- Optional: Sentry/global filter logs the error.

## Key mental model
- **Throw when your function can’t continue.**
- **Bubble up until the closest layer that can make a meaningful decision.**
- **Only controller (or global filter) decides HTTP status codes.**
