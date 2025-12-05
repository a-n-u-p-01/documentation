These three methods are used for **thread communication**, also known as **inter-thread signaling**.

They allow threads to **coordinate** and **wait for a specific condition** instead of continuously checking.

These methods belong to **Object class**, not Thread, because every object has a **monitor lock**.

---

# **Core Idea**

When multiple threads are working on the same object:

- `wait()` → a thread **pauses** and **releases the lock**
    
- `notify()` → wakes **one** waiting thread
    
- `notifyAll()` → wakes **all** waiting threads
    

This is the foundation of thread communication.

---

# **Why do we need wait/notify?**

Imagine a real-world scenario:

- Thread A: Produces a value
    
- Thread B: Consumes that value
    

Consumer must wait until Producer produces something.

Using loops like:

```java
while(valueNotReady) {
   // check again and again
}
```

is **CPU expensive**.

Instead, Consumer calls `wait()` → sleeps until Producer signals using `notify()`.

---

# **Rules (Very Important)**

1. Must be inside a `synchronized` block or method
    
2. Must hold the object monitor before calling these methods
    
3. `wait()` releases the lock
    
4. `notify()` does NOT release lock immediately; lock is released when synchronized block ends
    
5. `notifyAll()` is safer when multiple threads wait
    

---

# **wait()**

Used when a thread wants to pause until some condition becomes true.

- Releases the lock
    
- Thread goes into WAITING or TIMED_WAITING state
    
- Cannot continue until notified
    

Example:

```java
synchronized(obj) {
    obj.wait(); // thread pauses here
}
```

---

# **notify()**

Wakes up **one** random thread waiting on the same object.

- Does NOT release lock immediately
    
- Only releases lock when synchronized block finishes
    

Example:

```java
synchronized(obj) {
    obj.notify();
}
```

---

# **notifyAll()**

Wakes up **all** threads waiting on the same object.

Useful when:

- You do not know which thread must continue
    
- Multiple waiting conditions exist
    

Example:

```java
synchronized(obj) {
    obj.notifyAll();
}
```

---

# **Simple Example (Very Easy to Understand)**

### Shared object

```java
class Shared {
    int value = 0;
    boolean hasValue = false;
}
```

### Producer

```java
synchronized(shared) {
    while(shared.hasValue) {
        shared.wait();
    }

    shared.value = 10;
    shared.hasValue = true;
    shared.notify();
}
```

### Consumer

```java
synchronized(shared) {
    while(!shared.hasValue) {
        shared.wait();
    }

    System.out.println(shared.value);
    shared.hasValue = false;
    shared.notify();
}
```

---

# **What actually happens?**

1. Consumer comes → no value → calls `wait()`
    
2. Producer comes → produces value → calls `notify()`
    
3. Consumer wakes up and reads value
    

This avoids infinite checking loops.

---

# **Understanding Lock Release (Very Important)**

### `wait()`

Thread releases the lock and sleeps.

### `notify()`

Thread does NOT release lock immediately;  
it releases lock **after synchronized block ends**.

This is a common interview catch.

---

# **Common Mistakes**

1. Calling wait() without synchronized → IllegalMonitorStateException
    
2. Using if instead of while
    
    - Should always be:
        

```java
while(conditionNotMet) {
    wait();
}
```

3. Assuming notify wakes the "correct" thread
    
    - It wakes a random one
        
4. Forgetting lock release behavior
    

---

# **State transitions**

Here is a very clear diagram:

```
                +----------------------+
                |       New            |
                +----------+-----------+
                           |
                           v
                  +--------+---------+
                  |     Runnable     |
                  +--------+---------+
                           |
                           v
                       Running
                           |
        +------------------+-------------------+
        |                                      |
        v                                      v
    Blocked                              Waiting / Timed Waiting
        |                                      |
        +------------------+-------------------+
                           |
                           v
                      Runnable (again)
                           |
                           v
                     Terminated
```

---

# **When a thread calls wait()**

```
Running → Waiting  
Lock Released
```

# **When notify() is called**

```
Waiting → Runnable  
Lock NOT immediately released
```

# **When synchronized block exits**

```
Runnable → Running
```

---

# **Interview Questions with Answers**

---

### 1. Why do wait/notify belong to Object class, not Thread?

Because every object has a **monitor lock**, and threads wait/notify on that object.

---

### 2. Does wait() release the lock?

Yes.  
This is the most important point.

---

### 3. Does notify() release the lock?

No.  
It only releases lock **after** synchronized block finishes.

---

### 4. Why use while instead of if?

To avoid issues from **spurious wakeups**.

---

### 5. Difference between notify() and notifyAll()?

- notify(): wakes one random waiting thread
    
- notifyAll(): wakes all waiting threads
    

---

### 6. Can wait(), notify(), notifyAll() be called outside synchronized block?

No → throws IllegalMonitorStateException.

---

### 7. What happens if notify() is called but no thread is waiting?

Nothing happens. No error.

---

### 8. Which is preferred, notify or notifyAll?

notifyAll → safer for complex systems.  
notify → acceptable for simple producer–consumer.

---
