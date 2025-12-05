# **1. What is Global Exception Handling?**

Global Exception Handling refers to a **centralized mechanism** that catches and handles exceptions that occur anywhere in an application — especially the ones not handled locally.

This prevents:

- Application crash due to uncaught exceptions
    
- Scattered try-catch blocks
    
- Duplicate error-handling code
    
- Poor logging and debugging
    

It ensures **consistent error responses, logging, debugging, and recovery strategies**.

---

# **2. Why Global Exception Handling Is Needed**

Without a global handler:

- Exceptions thrown in background threads may terminate silently
    
- Errors may be logged inconsistently
    
- The application state may become unstable
    
- Users may see raw error messages or crashes
    

A global handler ensures:

- Safe fallback behavior
    
- Clean logging with root cause
    
- Standardized response for errors
    
- Graceful shutdown if needed
    

---

# **3. Uncaught Exception Handling in Java (Core Java)**

In Java, if a thread throws an exception and **no catch block** handles it, the JVM forwards it to the **Thread’s UncaughtExceptionHandler**.

### **Default Behavior**

If no handler is set, JVM prints stack trace and terminates that thread.

---

# **4. Setting a Global Uncaught Exception Handler (Thread-level)**

You can register a handler for **all threads**:

```java
Thread.setDefaultUncaughtExceptionHandler((thread, exception) -> {
    System.err.println("Uncaught exception in thread: " + thread.getName());
    exception.printStackTrace();
});
```

This handler executes whenever any uncaught exception appears.

---

### **Key Responsibilities of a Global Uncaught Handler**

- Log detailed exception info
    
- Clean up resources (files, DB connections, executors)
    
- Possibly restart failed services
    
- Avoid application corruption
    
- Send alert/notification for critical failures
    

---

# **5. Per-Thread Uncaught Exception Handler**

You can set a handler for one specific thread:

```java
Thread thread = new Thread(() -> {
    throw new RuntimeException("Error in worker thread");
});

thread.setUncaughtExceptionHandler((t, e) -> {
    System.out.println("Caught exception from: " + t.getName());
});
```

---

# **6. Uncaught Exception Handling in ThreadPool (Executors)**

Thread pools **do not** automatically use `Thread.setDefaultUncaughtExceptionHandler()`.

### Why?

Because pooled threads swallow exceptions unless tasks propagate them.

### Best Practice:

Wrap Runnable tasks manually:

```java
ExecutorService exec = Executors.newFixedThreadPool(2);

exec.execute(() -> {
    try {
        throw new RuntimeException("thread pool failure");
    } catch (Exception e) {
        System.err.println("Caught in wrapper: " + e);
    }
});
```

Or use `ThreadFactory`:

```java
ThreadFactory factory = r -> {
    Thread t = new Thread(r);
    t.setUncaughtExceptionHandler((thr, ex) -> {
        System.err.println("Error in thread pool: " + thr.getName());
    });
    return t;
};
```

---

# **7. Global Exception Handling in Web Applications (Servlet/JSP)**

In Java Web Applications (without Spring), global exception mapping is done using **web.xml**.

### **web.xml Example**

```xml
<error-page>
    <exception-type>java.lang.Exception</exception-type>
    <location>/globalError.jsp</location>
</error-page>
```

This catches **all uncaught exceptions** in Servlets/JSP and forwards users to an error page.

---

### **HTTP Error Code Mapping**

```xml
<error-page>
    <error-code>404</error-code>
    <location>/notFound.jsp</location>
</error-page>
```

---

# **8. Global Exception Handling in Spring Boot (ControllerAdvice)**

_(Since you're learning Spring also, this is valuable.)_

You can define a **single class** to handle all exceptions across controllers:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleException(Exception ex) {
        return ResponseEntity.status(500).body("Internal error: " + ex.getMessage());
    }
}
```

Benefits:

- Clean controllers
    
- One place for all common error responses
    
- Control HTTP status codes and messages
    
- Can log exceptions consistently
    

---

# **9. Difference Between UncaughtExceptionHandler and Global Web Handler**

|Feature|UncaughtExceptionHandler|Web Global Handler|
|---|---|---|
|**Level**|JVM/Thread-level|Web request-level|
|**Handles**|Exceptions in threads|Exceptions in controllers|
|**Use Case**|Background workers, schedulers, executors|REST APIs, Servlets|
|**Output**|Logs, restarts thread|Returns HTTP response|

---

# **10. Best Practices for Global Exception Handling**

1. **Never hide exceptions** → always log with cause
    
2. **Use a consistent format** for error messages/responses
    
3. **Capture root cause** (stack trace)
    
4. **Avoid infinite loops** when restarting threads
    
5. **Protect against sensitive data exposure** in messages
    
6. **In API applications** → return proper HTTP codes (400, 404, 500)
    
7. **In services** → notify DevOps/Monitoring tools
    
8. **In background threads** → ensure handler is always set
    

---

# **11. Simple Example Showing Exception Flow**

```java
public class App {
    public static void main(String[] args) {

        Thread.setDefaultUncaughtExceptionHandler((t, e) -> {
            System.out.println("Handled globally: " + e.getMessage());
        });

        Thread t = new Thread(() -> {
            throw new RuntimeException("Boom!");
        });

        t.start();
    }
}
```

**Output:**

```
Handled globally: Boom!
```

---

# **12. Common Interview Questions**

### 1️⃣ What is an UncaughtExceptionHandler?

A callback that handles exceptions thrown by a thread when no catch block handles them.

---

### 2️⃣ How to set a global exception handler for all threads?

Using:

```java
Thread.setDefaultUncaughtExceptionHandler();
```

---

### 3️⃣ Difference between catching exceptions and global exception handler?

- Normal catching handles known failures locally.
    
- Global handler catches unexpected failures anywhere in the thread system.
    

---

### 4️⃣ Does UncaughtExceptionHandler work for thread pools?

Not by default — you must use a custom ThreadFactory.

---

### 5️⃣ How to handle global exceptions in a web app?

Using:

```xml
<error-page>
```

or  
`@ControllerAdvice` (Spring Boot).

---
> Next : [File Handling (File Path Files API)](File%20Handling%20(File%20Path%20Files%20API).md)