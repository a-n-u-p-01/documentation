`Callable` and `Future` are part of the **java.util.concurrent** package and were introduced in **Java 5** to solve a limitation of threads:

> **A normal Runnable cannot return a result or throw a checked exception.**

`Callable` and `Future` are used with **ExecutorService** to run tasks in the background and get results later — similar to _async/await_ in other languages.

---

# **Callable Interface**

Callable represents a task that:

- returns a value
    
- can throw a checked exception
    
- is executed asynchronously
    

### Key Points

- It has a single method: `call()`
    
- The return type is generic: `Callable<T>`
    
- Used when you need a result from a background thread
    

### Example

```java
class MyTask implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        return 10 + 20;
    }
}
```

---

# **Future Interface**

`Future` represents the **result of an asynchronous computation**.  
It is returned when you submit a Callable to an ExecutorService.

### Future can:

- get the result
    
- check if the task is done
    
- cancel the task
    
- check if the task was cancelled
    

### Core Methods

|Method|Meaning|
|---|---|
|`get()`|Waits until result is available|
|`get(timeout)`|Waits for given time|
|`isDone()`|True if task completed|
|`cancel()`|Attempts to cancel the task|
|`isCancelled()`|True if cancelled|

---

# **Callable + Future Example**

```java
Callable<Integer> task = () -> {
    Thread.sleep(1000);
    return 42;
};

ExecutorService executor = Executors.newSingleThreadExecutor();

Future<Integer> future = executor.submit(task);

System.out.println("Task submitted");

// get() blocks until result is ready
Integer result = future.get();

System.out.println("Result = " + result);

executor.shutdown();
```

### Output

```
Task submitted
Result = 42
```

---

# **Why Callable is better than Runnable?**

|Feature|Runnable|Callable|
|---|---|---|
|Return value|No|Yes|
|Throws checked exceptions|No|Yes|
|Supports generics|No|Yes|
|Works with Future|No|Yes|

---

# **Non-blocking get() using isDone()**

```java
while (!future.isDone()) {
    System.out.println("Doing other work...");
    Thread.sleep(200);
}

System.out.println("Result: " + future.get());
```

---

# **Cancelling a Task**

```java
future.cancel(true);
```

If true → interrupt if running  
If false → cancel only if task not started

---

# **When to use Callable + Future**

Use when:

- You need the result of a background task
    
- You want exception handling from threads
    
- You want to run parallel computations
    
- You do CPU-intensive or I/O-intensive tasks
    
- You want to manage long-running tasks
    

Example use cases:

- Downloading data in the background
    
- Reading a file asynchronously
    
- Performing heavy calculations
    
- Calling APIs in parallel
    

---

# **Limitations of Future**

- Cannot chain tasks
    
- Cannot run callbacks after completion
    
- Cannot combine multiple futures
    
- No exception propagation control
    
- Hard to manage for complex async flows
    

These limitations led to:

### → **CompletableFuture** (Java 8)

More powerful, supports async chaining, callbacks, pipelines.

---

# **Interview Questions + Answers**

### 1. What is Callable?

Callable is an interface that represents a task that returns a result and may throw a checked exception.

---

### 2. What is Future?

Future is an object that stores the result of an asynchronous computation submitted to ExecutorService.

---

### 3. Difference between Runnable and Callable?

Runnable → no return, no exception  
Callable → returns result, can throw exception

---

### 4. Does Future get() block?

Yes. `get()` blocks until the result is ready unless a timeout is used.

---

### 5. How do you cancel a task?

Use `future.cancel(true)`.

---

### 6. Why do we use ExecutorService instead of creating threads manually?

It manages a pool of threads efficiently, reuses them, and improves performance.

---
