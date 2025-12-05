## Overview — goals of good exception handling

- Make failures visible and diagnosable.
    
- Keep code readable and maintainable.
    
- Provide meaningful context to callers and users.
    
- Avoid hiding errors or leaking sensitive details.
    
- Separate _exception handling policy_ from _business logic_.
    

---

## 1. Prefer explicit validation over exceptions for expected conditions

- Use argument checks (`if (arg == null) throw ...`) to fail fast.
    
- Don’t use exceptions for ordinary control flow (avoid try/catch as logic branch).
    
- Validate inputs early (public API boundary) so downstream code assumes valid values.
    

Example:

```java
public void setAge(int age) {
  if (age < 0) throw new IllegalArgumentException("age >= 0 required, got " + age);
}
```

---

## 2. Choose checked vs unchecked appropriately

- **Checked**: use when caller can reasonably recover (I/O, database).
    
- **Unchecked (RuntimeException)**: use for programming errors (invalid args, illegal state).
    
- Avoid overusing checked exceptions — they can clutter APIs. Prefer wrapping low-level checked exceptions into meaningful runtime exceptions at higher layers when appropriate.
    

---

## 3. Create a meaningful exception hierarchy for your domain

- Define application-specific exceptions (e.g., `DataAccessException`, `PaymentException`).
    
- Keep a small, clear hierarchy; provide both checked and runtime variants only when justified.
    
- Use exception types to express intent (recoverable vs fatal).
    

Example:

```java
public class DataAccessException extends RuntimeException {
  public DataAccessException(String msg, Throwable cause) { super(msg, cause); }
}
```

---

## 4. Always preserve the original cause (exception chaining)

- When wrapping, pass the original exception as the cause: `new MyException("msg", cause)`.
    
- This keeps stack traces and root cause info intact for debugging and monitoring.
    

Bad:

```java
catch (SQLException e) {
  throw new DataAccessException("db failed"); // lost original cause
}
```

Good:

```java
catch (SQLException e) {
  throw new DataAccessException("db failed for userId=" + id, e);
}
```

---

## 5. Use clear, contextual error messages

- Include contextual info: IDs, query text, filenames — but **never include secrets** (passwords, tokens).
    
- Prefer human- and machine-friendly messages (for logs and error responses).
    

Example:  
`"Failed to load user profile for userId=1234 from users table"`

---

## 6. Don’t swallow exceptions (avoid empty catch blocks)

- Swallowing hides bugs and causes subtle failures.  
    Bad:
    

```java
try { ... } catch (Exception e) { }
```

- If you must ignore, document why, and log at debug level with reason:
    

```java
catch (SpecificException e) {
  // Safe to ignore because X is optional; record for diagnostics
  logger.debug("Optional config missing, continuing", e);
}
```

---

## 7. Log exceptions responsibly

- Log at the appropriate level (error for unexpected, warn for recoverable, debug for noisy details).
    
- Log once at the boundary where you handle the error; avoid logging the same exception multiple times as it escalates.
    
- Include `exception` object in logger so stack trace is captured: `logger.error("msg", e)`.
    

---

## 8. Use try-with-resources for resource management

- Prefer try-with-resources to ensure proper closing and to surface suppressed exceptions automatically.
    

```java
try (Connection c = ds.getConnection();
     PreparedStatement ps = c.prepareStatement(sql)) {
   ...
}
```

- Check `Throwable.getSuppressed()` if you need suppressed exception details.
    

---

## 9. Handle InterruptedException correctly

- Do not swallow. Either rethrow or restore interrupted status:
    

```java
catch (InterruptedException e) {
  Thread.currentThread().interrupt(); // restore
  throw new MyAppException("interrupted", e); // or just return/propagate
}
```

---

## 10. Design API error contracts

- For public APIs (libraries, services), document exceptions thrown or map to standard error responses.
    
- For REST services, map exceptions to HTTP status codes with global handlers (e.g., `@ControllerAdvice` in Spring). Return meaningful error body (code, message, trace-id) but avoid stack traces in responses.
    

Example mapping:

- `ValidationException` → 400 Bad Request
    
- `NotFoundException` → 404 Not Found
    
- `AuthenticationException` → 401 Unauthorized
    
- Unexpected → 500 Internal Server Error (log details, return correlation id)
    

---

## 11. Exception translation at boundaries

- Translate low-level exceptions to higher-level ones at module boundaries (DAO → Service → Controller) to decouple layers.
    
- Keep low-level details in logs, expose generic message and error code to clients.
    

---

## 12. Retry and backoff strategies

- For transient errors (network, temporary DB locks), implement retries with exponential backoff and maximum attempts.
    
- Be idempotent or use idempotency keys to avoid duplicate side effects.
    
- Use libraries (Resilience4j, retry templates) rather than ad-hoc loops.
    

---

## 13. Fail fast, but degrade gracefully

- Prefer failing early when invariants broken (fail-fast).
    
- For user-facing systems, use graceful degradation and clear fallback behavior rather than systemic failure.
    

---

## 14. Avoid throwing generic Exception or Throwable

- Use the most specific exception type possible. `catch (Exception e)` only when rethrowing or logging at a high level; `catch (Throwable)` is almost always wrong.
    

---

## 15. Test exception paths

- Write unit tests that assert exceptions are thrown when expected.
    
- Test integration paths for error scenarios (connection refused, bad input). Use tools like Testcontainers to simulate failures.
    

---

## 16. Security and privacy considerations

- Don’t leak stack traces, SQL statements, or sensitive data in user-facing messages or logs accessible to untrusted parties.
    
- Redact data before logging if necessary.
    

---

## 17. Use centralized/global handlers for apps

- For server apps, centralize exception handling (e.g., a global exception handler) to ensure consistent logging, metrics, and user responses.
    
- Attach correlation IDs to logs and error responses to trace specific requests.
    

---

## 18. Measuring and alerting

- Track exception rates and types in monitoring (metrics, logs).
    
- Alert on spikes of unexpected exceptions or on specific severity classes.
    

---

## 19. Exception suppression: understand try-with-resources behavior

- When a resource close throws while another exception is being thrown, the close exception is **suppressed**. Use `getSuppressed()` to inspect them if debugging.
    

---

## 20. Practical patterns and examples

**A. Wrapping low-level exceptions**

```java
try {
  repo.save(entity);
} catch (SQLException e) {
  throw new DataAccessException("Failed to save order orderId=" + orderId, e);
}
```

**B. Global web handler (Spring-like pseudocode)**

```java
@ControllerAdvice
public class GlobalExceptionHandler {
  @ExceptionHandler(DataNotFoundException.class)
  ResponseEntity<ErrorDto> handleNotFound(DataNotFoundException ex) {
    return ResponseEntity.status(404).body(new ErrorDto("NOT_FOUND", ex.getMessage()));
  }
  @ExceptionHandler(Exception.class)
  ResponseEntity<ErrorDto> handleAny(Exception ex) {
    String id = traceId(); logger.error("unexpected", ex); // log full stack with id
    return ResponseEntity.status(500).body(new ErrorDto("INTERNAL","internal error, id=" + id));
  }
}
```

**C. Retrying transient failures with backoff (pseudo)**

```java
RetryPolicy policy = RetryPolicy.exponentialBackoff(100, 2000).withMaxAttempts(5);
Retryer.run(policy, () -> callRemoteService());
```

---

## Quick checklist for code reviews

- Are inputs validated early?
    
- Are exceptions meaningful and not generic?
    
- Are causes preserved when wrapping?
    
- Are resources closed (try-with-resources)?
    
- Is `InterruptedException` handled correctly?
    
- Are sensitive details redacted in messages?
    
- Is logging done once at boundary with full context?
    
- Are retry/backoff policies implemented for transient errors?
    
- Are exceptions exposed to users consistent and documented?
    

---

## Interview questions (short)

1. Checked vs unchecked — when to use each?
    
2. How and why to chain exceptions? (`getCause()`)
    
3. How to handle `InterruptedException`?
    
4. What is a suppressed exception and when does it occur?
    
5. How do you design REST error responses?
    
6. When should you wrap an exception? Give an example.
    
7. Why not catch `Throwable`?
    
8. How to implement retries safely (idempotency and backoff)?
    

---
> Next : [Global Exception Handling or Uncaught Exception Handlers](Global%20Exception%20Handling%20or%20Uncaught%20Exception%20Handlers.md)