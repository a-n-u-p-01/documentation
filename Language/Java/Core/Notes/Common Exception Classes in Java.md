## Quick taxonomy

- `Throwable` is the root. Two main branches:
    
    - `Error` (subclass of `Throwable`): serious problems you usually do **not** catch (e.g., `OutOfMemoryError`, `StackOverflowError`).
        
    - `Exception` (subclass of `Throwable`): application-level problems. Two subcategories:
        
        - **Checked exceptions** (must be declared/handled) — e.g., `IOException`, `SQLException`.
            
        - **Unchecked exceptions** (runtime) — subclass of `RuntimeException` — e.g., `NullPointerException`, `IllegalArgumentException`.
            

---

## Runtime exceptions (common, unchecked)

### `NullPointerException` (NPE)

- **When:** Accessing a member (field/method) on a `null` reference.
    
- **Why care:** Most common bug; indicates missing null-check or invariant violation.
    
- **Handle:** Prevent with null-checks, `Objects.requireNonNull()`, Optional (where appropriate), defensive programming.
    
- **Example:**
    

```java
String s = null;
int len = s.length(); // NPE
```

- **Best practice:** Fail fast with meaningful messages: `Objects.requireNonNull(obj, "user must not be null");`
    

### `IllegalArgumentException`

- **When:** Method receives an argument in illegal or inappropriate state.
    
- **Use-case:** Validate public API inputs.
    
- **Example:**
    

```java
void setAge(int age) {
  if (age < 0) throw new IllegalArgumentException("age must be >= 0");
}
```

### `IllegalStateException`

- **When:** Method invoked at an illegal or inappropriate time (object not in required state).
    
- **Example:** Calling `Iterator.remove()` before `next()`; calling `start()` on already started service.
    

### `IndexOutOfBoundsException` / `ArrayIndexOutOfBoundsException` / `StringIndexOutOfBoundsException`

- **When:** Index is outside valid range for array, list, or string.
    
- **Handle:** Validate index; prefer `List.get()` checks.
    
- **Example:**
    

```java
int[] a = new int[3];
int x = a[3]; // ArrayIndexOutOfBoundsException
```

### `ClassCastException`

- **When:** Invalid cast at runtime (downcast fails).
    
- **Prevent:** Use generics, `instanceof` checks.
    
- **Example:**
    

```java
Object o = "hello";
Integer i = (Integer) o; // ClassCastException
```

### `NumberFormatException`

- **When:** Parsing a numeric value from a malformed string (subclass of `IllegalArgumentException`).
    
- **Example:** `Integer.parseInt("abc")`
    
- **Handle:** Validate input or catch and respond.
    

### `ArithmeticException`

- **When:** Arithmetic errors (e.g., divide by zero for integers).
    
- **Example:** `int r = 1 / 0;`
    

### `UnsupportedOperationException`

- **When:** Operation not supported (e.g., immutable collection `add()`).
    
- **Example:** `Collections.unmodifiableList(list).add(x);`
    

### `NoSuchElementException`

- **When:** Trying to access an element that doesn’t exist (e.g., `Iterator.next()` if none).
    
- **Handle:** Check `hasNext()` before `next()`.
    

### `ConcurrentModificationException`

- **When:** Detects concurrent modification while iterating (fail-fast collections).
    
- **Prevent:** Use `Iterator.remove()` correctly, use concurrent collections (`ConcurrentHashMap`, `CopyOnWriteArrayList`), or collect and operate.
    

---

## Checked exceptions (common)

### `IOException` (and common subclasses)

- **When:** I/O failure (file, stream, network).
    
- **Subclasses:**
    
    - `FileNotFoundException` — file missing or path wrong.
        
    - `EOFException` — unexpected end-of-stream.
        
    - `SocketException` — network socket errors (often thrown as `IOException`).
        
- **Handle:** Catch and either recover, retry, or wrap into an application-specific exception. Use try-with-resources to close streams automatically.
    
- **Example:**
    

```java
try (InputStream in = new FileInputStream("file.txt")) {
  // read
} catch (IOException e) {
  // handle
}
```

### `SQLException`

- **When:** Database access errors via JDBC.
    
- **Details:** Often contains vendor-specific SQL state and error code. Be careful to close resources (use try-with-resources).
    
- **Handling patterns:** Exception translation (wrap in custom DAO exception), retry transient failures.
    

### `ClassNotFoundException`

- **When:** Class loader fails to find a class (e.g., `Class.forName()`).
    
- **Handle:** Ensure class is on classpath; often thrown for dynamic loading.
    

### `InterruptedException`

- **When:** A thread is interrupted while waiting, sleeping, or blocking.
    
- **Important:** Restore interrupt status unless handling: `Thread.currentThread().interrupt();`
    
- **Example:** `Thread.sleep(1000)` throws `InterruptedException`.
    

### `ReflectiveOperationException` (umbrella)

- **Includes:** `ClassNotFoundException`, `IllegalAccessException`, `InvocationTargetException`, `NoSuchMethodException`, `InstantiationException`.
    
- **When:** Problems during reflection operations.
    

### `TimeoutException`

- **When:** Operations exceed allowed time (concurrency API, futures, network libs).
    
- **Handle:** Provide fallback or retry, fail fast.
    

---

## Errors (serious, usually not caught)

### `OutOfMemoryError`

- **When:** JVM cannot allocate memory (heap exhausted).
    
- **Not recoverable** in general; diagnose with heap dumps, reduce memory footprint, tuning GC, increase heap size.
    

### `StackOverflowError`

- **When:** Deep recursion or runaway stack allocations.
    
- **Fix:** Correct recursion, reduce stack depth.
    

### `LinkageError` / `NoClassDefFoundError`

- **When:** Binary incompatibilities, class-loading issues, or missing definitions at runtime.
    
- **Note:** `NoClassDefFoundError` often follows `ClassNotFoundException` in static init failures.
    

---

## Reflection & wrapper exceptions

### `InvocationTargetException`

- **When:** Reflection `Method.invoke()` throws an exception; the underlying exception is wrapped and can be retrieved via `getTargetException()` or `getCause()`.
    

### `ExceptionInInitializerError`

- **When:** Exception occurs during static initialization block or static field initialization. Indicates startup failure.
    

---

## Concurrency-related exceptions

### `ExecutionException`

- **When:** Thrown by `Future.get()` to wrap underlying exception thrown in task; `getCause()` contains real exception.
    

### `IllegalMonitorStateException`

- **When:** Thread calls `wait()`/`notify()`/`notifyAll()` without owning the monitor (synchronized block).
    

### `RejectedExecutionException`

- **When:** ThreadPoolExecutor rejects task submission (queue full, policy).
    

---

## Best practices: handling, wrapping, and translation

### Checked vs unchecked: design guideline

- Use checked exceptions when caller can reasonably recover (I/O, DB).
    
- Use unchecked for programming errors (invalid arguments, nulls).
    

### Exception wrapping and chaining

- Wrap low-level checked exceptions into higher-level ones for abstraction (exception translation).
    
- Preserve cause: `throw new DataAccessException("db failed", sqlEx);`
    
- Always use `initCause()` or constructor that accepts cause; inspect with `getCause()`.
    

### Rethrowing and preserving stack trace

- When rethrowing, prefer: `throw new MyException("msg", e);` rather than `throw e;` if wrapping.
    
- If rethrowing same exception after logging, just `throw e;` — avoid double-logging.
    

### Suppressed exceptions (try-with-resources)

- Resources closed in `try-with-resources` may suppress exceptions; access via `Throwable.getSuppressed()`.
    

### Use meaningful messages

- Include context (IDs, file paths, parameters) but avoid leaking sensitive info (credentials) in messages.
    

### Logging best practices

- Log exceptions at appropriate level (error for unexpected, warn for recoverable).
    
- Avoid logging stack trace multiple times along propagation chain.
    

### Avoid catch-all

- Avoid `catch (Exception e)` unless you rethrow or wrap appropriately and handle rare cases. Catching `Throwable` is almost always wrong.
    

---

## Creating custom exceptions

- Extend `RuntimeException` for unchecked custom exceptions.
    
- Extend `Exception` for checked custom exceptions.
    
- Provide constructors:
    

```java
public class MyAppException extends RuntimeException {
  public MyAppException(String msg) { super(msg); }
  public MyAppException(String msg, Throwable cause) { super(msg, cause); }
}
```

- Use custom exceptions to encapsulate domain errors, preserve abstraction.
    

---

## Debugging tips: reading stack traces

- Top of stack trace = point where exception was created/first thrown.
    
- Follow the chain: `Caused by:` sections reveal root cause.
    
- Use line numbers and class names to navigate to offending code.
    

---

## Small patterns & examples

### Exception translation (DAO example)

```java
try {
  // JDBC code
} catch (SQLException e) {
  throw new DataAccessException("Failed to fetch user id=" + id, e);
}
```

### Cleaning up with try-with-resources

```java
try (Connection c = ds.getConnection();
     PreparedStatement ps = c.prepareStatement(sql)) {
  // ...
} catch (SQLException e) {
  // handle
}
```

### InterruptedException handling

```java
try {
  Thread.sleep(1000);
} catch (InterruptedException e) {
  Thread.currentThread().interrupt(); // restore
  // handle or return
}
```

---

## Common interview questions (short answers)

1. **Checked vs Unchecked exceptions?**  
    Checked must be declared/handled; unchecked (RuntimeException) need not be.
    
2. **When to use custom checked vs unchecked?**  
    Checked if caller can act on exception; unchecked for programming errors.
    
3. **How to preserve original exception?**  
    Pass it as the cause: `new Exception("msg", cause)`.
    
4. **What is `Suppressed` exception?**  
    Exceptions suppressed when closing resources in try-with-resources; accessible with `getSuppressed()`.
    
5. **How to handle `InterruptedException`?**  
    Restore interrupt status or propagate; do not swallow silently.
    
6. **Why avoid catching `Throwable`?**  
    It includes `Error` (serious JVM errors) which are not meant to be handled and can mask failures.
    

---
>next : [Best Practices in Exception Handling](Best%20Practices%20in%20Exception%20Handling.md)