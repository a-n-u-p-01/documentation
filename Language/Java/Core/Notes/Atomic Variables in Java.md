# **What Are Atomic Variables?**

Atomic variables are classes in `java.util.concurrent.atomic` package that allow you to **safely update shared variables in a multithreaded environment** without using `synchronized` or locks.

They provide **lock-free**, **non-blocking**, **thread-safe** operations using **CPU-level atomic instructions** (CAS — Compare-And-Swap).

---

# **Why Atomic Variables?**

When multiple threads update a shared variable:

```java
count = count + 1;
```

This looks simple but is **NOT atomic**. It actually has 3 steps:

1. Read value
    
2. Increment
    
3. Write value
    

If two threads do this at the same time → **race condition**.

Using `synchronized` fixes it but slows performance.

Atomic variables solve this by performing updates **atomically**, using CPU hardware instructions.

---

# **Key Atomic Classes**

### **AtomicInteger**

For atomic int operations.

### **AtomicLong**

For atomic long operations.

### **AtomicBoolean**

For atomic boolean operations.

### **AtomicReference**

For atomic updates on objects.

### **AtomicIntegerArray, AtomicLongArray**

For atomic array operations.

---

# **How CAS (Compare And Swap) Works**

Atomic classes use CAS internally:

```
If (currentValue == expectedValue)
    update to newValue
Else
    retry
```

This avoids locking, so performance is high.

Example internal flow:

```
Thread 1 reads value 10
Thread 2 reads value 10
Thread 1 CAS(10 → 11) succeeds
Thread 2 CAS(10 → 11) fails (because value changed)
Thread 2 retries with fresh value 11
```

---

# **AtomicInteger Example**

```java
AtomicInteger counter = new AtomicInteger(0);

counter.incrementAndGet(); // atomic ++
counter.getAndIncrement(); // returns old value
counter.addAndGet(5);      // atomic += 5
counter.getAndSet(100);    // atomically set new value
```

Everything is thread-safe **without locks**.

---

# **AtomicReference Example**

```java
AtomicReference<String> ref = new AtomicReference<>("A");

ref.compareAndSet("A", "B");   // safe update
```

Used when you want atomic updates on custom objects like User, Order, etc.

---

# **Atomic Variables vs Synchronized**

|Feature|Atomic Variables|synchronized|
|---|---|---|
|Locking|No|Yes|
|Speed|Faster|Slower|
|Blocking|Non-blocking|Blocking|
|CAS-based|Yes|No|
|Best For|High-performance applications|Simpler logic|

Atomic variables are widely used in **high-performance servers**, **counters**, **statistics**, **metrics**, **ID generation**, etc.

---

# **Common Methods (Important)**

### incrementing:

- `incrementAndGet()`
    
- `getAndIncrement()`
    

### updating:

- `addAndGet(x)`
    
- `getAndAdd(x)`
    

### comparing and setting:

- `compareAndSet(expected, newValue)`
    

### getting and setting:

- `get()`
    
- `set()`
    
- `lazySet()`
    

---

# **Where Atomic Variables Are Used in Real Projects**

- API rate limiters
    
- Counting active users
    
- Generating unique IDs
    
- Thread-safe caches
    
- Network message counters
    
- Logging counters
    
- Metrics collection (likes, views, hits)
    

---

# **Interview Questions + Answers**

### **1. What is an atomic operation?**

A thread-safe operation performed in one step, without interruption.

---

### **2. Why use AtomicInteger instead of int?**

Because operations like `increment` are _not atomic_ and cause race conditions.

---

### **3. How does CAS work internally?**

It compares current value with expected value; if equal, it updates; otherwise retry.

---

### **4. Difference between synchronized and atomic?**

Atomic is lock-free and faster; synchronized is blocking and slower.

---

### **5. When should we use AtomicReference?**

When you want to atomically update an entire object, not just a primitive.

---

### **6. What is ABA problem?**

A value changes from A → B → A before CAS checks, and CAS mistakenly succeeds.

(AtomicStampedReference solves this)

---
