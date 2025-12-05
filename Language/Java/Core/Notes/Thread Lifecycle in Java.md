A thread in Java does not run continuously from start to finish.  
It moves through different states during its lifetime.  
These states describe what the thread is **currently doing** internally.

Java defines **six official thread states**.

---

## **New**

A thread object is created but not yet started.

```java
Thread t = new Thread(() -> {});
```

Important points:

- Memory is allocated
    
- Thread is NOT scheduled yet
    
- Only after calling `start()` it can run
    

---

## **Runnable**

Thread is **ready to run** and waiting for CPU time.

It may be:

- Running actively
    
- Waiting in the runnable queue
    

Java groups both together as **RUNNABLE** even if the thread is not currently executing.

---

## **Running**

Although Java does not show "Running" as a separate state,  
conceptually a thread is running when:

- CPU has assigned it time
    
- It is executing the `run()` method
    

So **Running = Runnable + actually executing**.

---

## **Blocked**

Thread wants to enter a synchronized block/method  
but another thread is currently holding the lock.

Example:

```java
synchronized(lock) {
    // Thread A has entered
}
```

Thread B trying to enter the same lock becomes **Blocked**.

Important:

- Cannot proceed
    
- Waiting for another thread to release the monitor
    
- Not related to I/O or sleep
    

---

## **Waiting**

Thread is waiting **indefinitely** for another thread to signal it.

Common causes:

- `wait()`
    
- `join()` without timeout
    
- `park()`
    

Example:

```java
obj.wait();  // thread enters WAITING state
```

Key point:

- Thread is paused
    
- It needs another thread to notify or complete
    

---

## **Timed Waiting**

Thread waits, but only for a **specific time**.

Examples:

```java
Thread.sleep(2000);
obj.wait(1000);
thread.join(500);
```

Differences from WAITING:

- Thread automatically continues after timeout
    
- Does not need a notification
    

---

## **Terminated**

Thread has finished execution or stopped due to an error.

Example:

```java
public void run() {
    System.out.println("done");
}
// run() finishes → Terminated
```

Once a thread reaches this state:

- It **cannot be restarted**
    
- Calling `start()` again throws `IllegalThreadStateException`
    

---

## **Complete Lifecycle Summary**

```
New → Runnable → Running → Terminated
          ↑          ↓
     Waiting    Timed Waiting
          ↑
      Blocked
```

Explanation:

- New → created
    
- Runnable → waiting for CPU
    
- Running → CPU executing
    
- Blocked → waiting for monitor lock
    
- Waiting → waiting without timeout
    
- Timed waiting → waiting with timeout
    
- Terminated → finished
    

---

## **Practical Example Showing States**

```java
class DemoThread extends Thread {
    public void run() {
        try {
            Thread.sleep(500);
            synchronized (this) {
                wait(200);
            }
        } catch (Exception e) {}
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        DemoThread t = new DemoThread();
        System.out.println(t.getState()); // NEW

        t.start();
        System.out.println(t.getState()); // RUNNABLE

        Thread.sleep(50);
        System.out.println(t.getState()); // TIMED_WAITING (sleep)

        Thread.sleep(600);
        System.out.println(t.getState()); // TIMED_WAITING (wait)

        Thread.sleep(500);
        System.out.println(t.getState()); // TERMINATED
    }
}
```

---

## **Interview Questions with Answers**

### 1. What are the thread states in Java?

New, Runnable, Blocked, Waiting, Timed Waiting, Terminated.

---

### 2. What causes a thread to enter Blocked state?

When it tries to enter a synchronized block/method but another thread holds the lock.

---

### 3. Difference between Waiting and Timed Waiting?

Waiting → no timeout; needs notification  
Timed Waiting → auto-continues after timeout

---

### 4. Can a thread be restarted once terminated?

No. Calling `start()` again throws an exception.

---

### 5. When does a thread enter Runnable state?

After calling `start()` and before the CPU chooses to run it.

---

### 6. Is Running a real state in Java?

Not officially. Java merges **Running** into **Runnable**.  
Runnable = running + ready to run.

---

### 7. What triggers Terminated state?

- run() completes
    
- Unhandled exception in the thread
    

---