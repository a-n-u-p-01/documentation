# **Thread Class and Runnable Interface**

Java is a multithreaded programming language, meaning a program can run multiple tasks at the same time. A thread is the smallest independent unit of execution. To create threads in Java, you mainly use either the Thread class or the Runnable interface. Both allow concurrent execution but differ in flexibility and design.

---

## **What a Thread Is**

A thread is a path of execution inside a program. Each thread has its own stack, program counter, and execution flow. Multiple threads run inside the same process and share memory, which makes communication easy but requires proper synchronization.

---

## **Creating Threads: Two Approaches**

### **1. Extending the Thread Class**

You create a subclass of Thread and override the run() method.

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
    }
}
```

Explanation:

- run() contains the code that executes inside the new thread
    
- start() creates a new thread and calls run() internally
    
- You cannot extend another class because Java supports single inheritance
    

---

### **2. Implementing the Runnable Interface**

A more flexible and widely used way.

```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task running");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyTask());
        t.start();
    }
}
```

Explanation:

- Runnable allows your class to extend another class
    
- Runnable is preferred for real-world applications and thread pools
    
- Thread object still manages execution, Runnable only provides the task
    

---

## **Why Runnable Is Better**

Runnable offers flexibility because the class does not need to extend Thread. It also works with ExecutorService, which is the modern way of handling threads. Runnable is considered lightweight and promotes better object-oriented design.

---

## **Understanding start() vs run()**

Calling run() manually does not start a new thread. It behaves like a normal method call.

```java
t.run();  // runs in main thread
t.start();  // runs in a new thread
```

start() → creates new thread  
run() → executes inside current thread

This is a common interview question.

---

## **Useful Methods of Thread Class**

- start() → begins execution in a new thread
    
- run() → task definition
    
- sleep(milliseconds) → pauses current thread
    
- join() → waits for another thread to finish
    
- interrupt() → signals thread to stop sleeping/waiting
    
- isAlive() → checks if thread is still running
    
- yield() → hints the scheduler to temporarily pause
    

These methods help manage thread behavior.

---

## **Example With Two Threads Running Together**

```java
class PrintTask implements Runnable {
    private String msg;

    PrintTask(String msg) { this.msg = msg; }

    public void run() {
        for (int i = 1; i <= 5; i++)
            System.out.println(msg + " : " + i);
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new PrintTask("Task A"));
        Thread t2 = new Thread(new PrintTask("Task B"));

        t1.start();
        t2.start();
    }
}
```

Both threads run at the same time, and output interleaves depending on CPU scheduling.

---

## **Creating Threads Using Lambda (Modern Java)**

```java
Thread t = new Thread(() -> System.out.println("Running via lambda"));
t.start();
```

Short and widely used in modern Java applications.

---

## **When to Use Each Approach**

Use Thread class when:

- You need to override thread-specific methods
    
- You want full control of thread behavior
    

Use Runnable when:

- Your class already extends another class
    
- You want clean architecture
    
- You use ExecutorService or thread pools (best practice)
    

Most developers use Runnable or Callable, not Thread subclassing.

---

## **Common Mistakes**

- Calling run() instead of start() (does not create a new thread)
    
- Starting the same thread twice (throws IllegalThreadStateException)
    
- Not handling exceptions inside run()
    
- Performing heavy work on the main thread
    

---

## **Interview Questions with Answers**

**1. What is the difference between Thread and Runnable?**  
Thread is a class; Runnable is an interface. Runnable is preferred because it allows inheritance flexibility and works with thread pools.

**2. Why must we override run()?**  
run() defines the code executed by the thread. start() internally calls run().

**3. Can we call start() twice on the same thread?**  
No. Only one start is allowed. Second call throws IllegalThreadStateException.

**4. Which is better for real applications: Thread or Runnable?**  
Runnable, because it is flexible, supports separation of task and execution, and works with ExecutorService.

**5. What happens if we call run() directly?**  
It runs as a normal method inside the current thread. No new thread is created.

**6. Why is multithreading used?**  
For faster execution, parallel tasks, non-blocking operations, and better CPU utilization.

---
