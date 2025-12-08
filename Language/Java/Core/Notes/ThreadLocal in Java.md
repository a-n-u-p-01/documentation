`ThreadLocal` is a special Java class that allows you to store data separately for each thread.
Each thread gets **its own copy** of a variable, and no other thread can access it.

This is extremely useful when:

- Multiple threads use the same object but must not share data
    
- You want to avoid synchronization
    
- You want thread-specific state
    

---

# **1. What Problem Does ThreadLocal Solve?**

Normally, if multiple threads use the same object:

```java
class UserContext {
    public static String name;
}
```

Threads overwrite each other’s data:

```
Thread A → name = "A"
Thread B → name = "B"
Thread A → now sees "B"
```

This causes **race conditions**.

`ThreadLocal` solves this by giving each thread its own isolated copy.

---

# **2. How ThreadLocal Works**

Each thread has an internal map called:

```
ThreadLocalMap
```

When you write:

```java
ThreadLocal<String> tl = new ThreadLocal<>();
tl.set("Hello");
```

The value **does NOT go inside the ThreadLocal object**.

Instead, it goes into:

```
Thread → ThreadLocalMap → (ThreadLocal instance, value)
```

So:

- Thread A stores its own value
    
- Thread B stores its own value
    
- No thread sees values of others
    

---

# **3. Basic Usage**

### Create a ThreadLocal variable

```java
ThreadLocal<String> tl = new ThreadLocal<>();
```

### Set value for this thread

```java
tl.set("Anupam");
```

### Get value for this thread

```java
String v = tl.get();
```

### Remove value (important!)

```java
tl.remove();
```

---

# **4. Full Example**

```java
public class Demo {

    private static ThreadLocal<Integer> threadId = new ThreadLocal<>();

    public static void main(String[] args) {
        
        Runnable task = () -> {
            threadId.set((int) (Math.random() * 100));
            System.out.println(Thread.currentThread().getName() + " → " + threadId.get());
        };

        new Thread(task, "Thread-A").start();
        new Thread(task, "Thread-B").start();
        new Thread(task, "Thread-C").start();
    }
}
```

### Output (example)

```
Thread-A → 42
Thread-B → 88
Thread-C → 13
```

Each thread gets its **own copy**, even though all use the same `ThreadLocal` object.

---

# **5. ThreadLocal vs Synchronization**

### Without ThreadLocal

Shared data → needs synchronization  
→ slower performance  
→ complex code

### With ThreadLocal

Each thread has its own data  
→ no synchronization needed  
→ faster and simpler

---

# **6. ThreadLocal.withInitial() (Recommended)**

Instead of manually setting default values, you can do:

```java
ThreadLocal<Integer> count = ThreadLocal.withInitial(() -> 0);
```

---

# **7. Real Project Uses**

### ✔ Storing logged-in user info (per request)

```java
UserContext.set(currentUser);
```

### ✔ Keeping DB connection per thread

In frameworks like Hibernate.

### ✔ Keeping request ID for logs

To trace a single request across a thread.

### ✔ Maintaining formatters (date/number)

Because `SimpleDateFormat` is **not thread-safe**.

---

# **8. Memory Leak Warning**

ThreadLocal can cause memory leaks if not removed.

Why?

Because:

- Threads in thread pools live for a long time
    
- ThreadLocalMap entries stay forever
    
- Even after your task is completed
    

Fix:

Always call `remove()` in finally:

```java
try {
    threadLocal.set(value);
} finally {
    threadLocal.remove();
}
```

---

# **9. Internals (Easy Explanation)**

`Thread` object contains:

```
ThreadLocalMap table;
```

Each entry in the map:

```
Key = WeakReference<ThreadLocal>
Value = actual data for that thread
```

If you forget to remove() and ThreadLocal object is garbage collected,  
the value may remain stuck → leak.

---

# **10. Interview Questions (with Answers)**

---

### **1. What is ThreadLocal?**

**Answer:**  
ThreadLocal provides thread-specific variables. Each thread gets its own independent copy of the variable.

---

### **2. Why do we use ThreadLocal?**

**Answer:**  
To avoid sharing data between threads, avoid synchronization, and maintain thread-safe state.

---

### **3. Where is ThreadLocal data stored?**

**Answer:**  
Inside each Thread’s internal `ThreadLocalMap`.

---

### **4. Why is remove() important?**

**Answer:**  
To prevent memory leaks, especially in thread pools.

---

### **5. How is ThreadLocal used in real applications?**

- Request context storage
    
- Thread-safe date formatting
    
- Storing DB session per thread
    
- Logging with request IDs
    

---

### **6. How is ThreadLocal different from synchronization?**

**Answer:**  
Synchronization shares one variable across threads.  
ThreadLocal gives each thread its own variable → no locking needed.

---
