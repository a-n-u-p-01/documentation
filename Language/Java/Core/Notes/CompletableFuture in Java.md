`CompletableFuture` (Java 8+) is an advanced concurrency API that makes **asynchronous programming**, **parallel execution**, and **non-blocking pipelines** easy.

It is used heavily in **real projects**, microservices, and high-performance systems.

---

# **1. Why CompletableFuture?**

Before Java 8:

- `Future` could not be completed manually
    
- No callback support
    
- No chaining
    
- No combining multiple futures
    

`CompletableFuture` solves all these limitations.

It supports:

- Async execution
    
- Callbacks (thenApply, thenAccept…)
    
- Chaining tasks
    
- Combining multiple futures
    
- Exception handling
    
- Manual completion
    

---

# **2. Creating a CompletableFuture**

### A. Run a task asynchronously (no return value)

```java
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Running task in background...");
});
```

### B. Run a task asynchronously (returns value)

```java
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    return 10;
});
```

`supplyAsync()` is used when the task **returns** something.

---

# **3. Getting the Result**

```java
int result = future.get();      // waits and returns result
int result2 = future.join();    // similar but unchecked exception
```

`join()` is preferred because it avoids checked exceptions.

---

# **4. Chaining Tasks (Very Important)**

You can create a **pipeline** of tasks.

### thenApply (transform value)

```java
CompletableFuture<Integer> future =
    CompletableFuture.supplyAsync(() -> 10)
                     .thenApply(n -> n * 2);  // Result = 20
```

### thenAccept (consume value)

```java
CompletableFuture.supplyAsync(() -> "Data")
                 .thenAccept(s -> System.out.println("Received: " + s));
```

### thenRun (no input, no output)

```java
future.thenRun(() -> System.out.println("Task finished!"));
```

---

# **5. Combining Two CompletableFutures**

### thenCombine()

```java
CompletableFuture<Integer> f1 = CompletableFuture.supplyAsync(() -> 10);
CompletableFuture<Integer> f2 = CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> result =
    f1.thenCombine(f2, (a, b) -> a + b);  // 30
```

### allOf() – Wait for all futures

```java
CompletableFuture<Void> all =
    CompletableFuture.allOf(f1, f2);
```

### anyOf() – Returns first completed future

```java
CompletableFuture<Object> any =
    CompletableFuture.anyOf(f1, f2);
```

---

# **6. Exception Handling**

### exceptionally()

```java
CompletableFuture<Integer> future =
    CompletableFuture.supplyAsync(() -> {
        if (true) throw new RuntimeException("Error!");
        return 10;
    }).exceptionally(ex -> {
        System.out.println(ex.getMessage());
        return -1;  // fallback
    });
```

### handle()

Called whether exception occurs or not.

```java
future.handle((result, ex) -> {
    if (ex != null) return -1;
    return result;
});
```

---

# **7. Manually Completing a CompletableFuture**

```java
CompletableFuture<String> cf = new CompletableFuture<>();

cf.complete("Done!");  // manually completed
```

Useful when the result comes from **an external event**, like:

- Network callback
    
- WebSocket message
    
- Timer
    

---

# **8. Using Custom Executor (Optional but Important)**

By default, CompletableFuture uses **ForkJoinPool.commonPool()**.

But we can provide our own thread pool:

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

CompletableFuture.supplyAsync(() -> {
    return "Task done";
}, executor);
```

This is used in production systems to avoid blocking the default pool.

---

# **9. What Makes CompletableFuture Powerful?**

- Non-blocking operations
    
- Callback-based pipelines
    
- Parallel execution
    
- Easy combination of results
    
- Automatic error handling
    
- Supports functional programming style
    
- Used in scalable, reactive systems
    

It brings Java close to **JavaScript promises** and **reactive programming**.

---

# **10. Interview Questions (with answers)**

### 1. What is CompletableFuture?

It is an asynchronous computation API that allows running tasks without blocking the main thread and supports chaining, combining, and callbacks.

---

### 2. Difference between Future and CompletableFuture?

|Future|CompletableFuture|
|---|---|
|Cannot complete manually|Can complete manually|
|Blocking get()|Non-blocking callbacks|
|No chaining|Chaining supported|
|No combining|Supports thenCombine, allOf|
|No exception handling|Built-in exception handling|

---

### 3. What is thenApply() vs thenAccept()?

- `thenApply()` → transforms value
    
- `thenAccept()` → consumes value but returns nothing
    

---

### 4. What does allOf() do?

Waits for **all** futures to complete.

---

### 5. What is exceptionally()?

Handles errors and provides fallback values.

---

### 6. Why use custom executors?

To avoid overloading the default common thread pool and to control thread count.

---
