# **What is Synchronization?**

Synchronization is a mechanism that ensures **only one thread at a time** can access a shared resource (variable, method, object).  
It prevents problems like:

- inconsistent data
    
- corrupted state
    
- race conditions
    

Without synchronization, multiple threads may modify the same variable at the same time → leading to unpredictable results.

---

# **Why Synchronization Is Needed**

Example problem (Race Condition):

```java
class Counter {
    int count = 0;

    public void increment() {
        count++;  
    }
}
```

If **two threads** call increment() simultaneously:

- both read count = 0
    
- both increment to 1
    
- final output = 1 (WRONG)
    

Expected: 2

Synchronization fixes this by allowing **only one thread** inside increment() at a time.

---

# **Types of Synchronization**

1. **Synchronized Method**
    
2. **Synchronized Block**
    
3. **Static Synchronization**
    
4. **Lock API (ReentrantLock)**
    

---

# **1. Synchronized Method**

```java
public synchronized void increment() {
    count++;
}
```

- Locks the **object**
    
- No other thread can enter _any_ synchronized method on the same object
    
- Simple but less flexible
    

---

# **2. Synchronized Block**

```java
public void increment() {
    synchronized (this) {
        count++;
    }
}
```

- More efficient
    
- Only the code inside the block is locked
    
- Preferred in high-performance scenarios
    

You can lock on any object:

```java
synchronized (lockObject) {
    // critical section
}
```

---

# **3. Static Synchronization**

Used when threads share **class-level data**.

```java
public static synchronized void process() {}
```

Locks on **Class object**, not instance.

---

# **4. Lock API (ReentrantLock)**

More powerful than `synchronized`:

```java
Lock lock = new ReentrantLock();

lock.lock();
try {
    count++;
} finally {
    lock.unlock();
}
```

Advantages:

- fair locking
    
- tryLock() (non-blocking attempt)
    
- interruptible lock waiting
    

Used in complex multithreaded applications.

---

# **Monitor (Intrinsic Lock)**

Every object in Java has a **monitor lock**.

When a thread enters a synchronized block/method:

- it acquires the lock
    
- no other thread can enter synchronized code using same lock
    
- when exiting, it releases the lock
    

---

# **Deadlock (Important)**

Occurs when two threads hold locks that the other needs.

Example:

Thread A locks Resource 1 → waits for Resource 2  
Thread B locks Resource 2 → waits for Resource 1

Both wait forever.  
Avoid by locking resources in a fixed order.

---

# **Thread Interference Example**

```
count = 0
Thread A reads → 0
Thread B reads → 0
Thread A writes → 1
Thread B writes → 1
Final result = 1
```

Synchronization prevents this.

---

# **Memory Consistency Errors**

Without synchronization:

- one thread may not see the latest value of a variable
    
- caches may not update immediately
    

`synchronized` forces **happens-before** relationship.

---

# **Best Practices**

- Use synchronized blocks instead of whole methods when possible
    
- Minimize time spent inside synchronized block
    
- Never perform long tasks inside a lock
    
- Avoid nested locks (risk of deadlock)
    
- Prefer `java.util.concurrent` packages for better performance:
    
    - Atomic variables
        
    - ConcurrentHashMap
        
    - ExecutorService
        

---

# **Interview Questions + Answers**

---

### **1. What is synchronization?**

It ensures only one thread accesses a shared resource at a time, preventing race conditions.

---

### **2. Difference between synchronized method and block?**

Method locks the _whole method_.  
Block locks only a _small section_, making it more efficient.

---

### **3. What is intrinsic lock?**

Every object has a built-in lock.  
`synchronized` uses this lock.

---

### **4. What is ReentrantLock?**

A flexible lock with features like fairness, ability to try locking without waiting, and interruptible locking.

---

### **5. What is deadlock?**

Two or more threads waiting on each other forever, preventing execution.

---

### **6. How to avoid deadlock?**

- Acquire locks in a fixed order
    
- Use tryLock()
    
- Minimize synchronized usage
    

---

### **7. What is thread interference?**

When multiple threads modify shared data incorrectly due to unsynchronized access.

---

### **8. What are memory consistency errors?**

One thread sees stale data due to lack of synchronization.

---