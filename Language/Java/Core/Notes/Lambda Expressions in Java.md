Lambda expressions were introduced in Java 8 to make code **shorter, cleaner, and functional**.  
They allow you to **pass behavior (functions) as data**, which was not possible before.

A lambda is basically an **anonymous function** — a method without a name.

---

# **1. Basic Syntax**

```
(parameters) -> expression
(parameters) -> { statements }
```

Examples:

```java
() -> System.out.println("Hello");
x -> x * x;
(a, b) -> a + b;
```

---

# **2. Why Lambdas?**

Before Java 8:

You had to write anonymous classes:

```java
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Running");
    }
};
```

With lambda:

```java
Runnable r = () -> System.out.println("Running");
```

### Benefits

- Less code
    
- Clean and readable
    
- Enables functional programming
    
- Used heavily in Streams and Collections
    

---

# **3. Lambda with Functional Interfaces**

A lambda can only work with a **Functional Interface**, i.e., an interface with **one abstract method**.

Example:

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Using lambda:

```java
Calculator c = (a, b) -> a + b;
```

---

# **4. Types of Lambda**

### A. No Parameter

```java
() -> System.out.println("Hello");
```

### B. Single Parameter

```java
x -> x * x;
```

### C. Multiple Parameters

```java
(a, b) -> a + b;
```

### D. Multi-line Lambda

```java
(a, b) -> {
    System.out.println("Adding...");
    return a + b;
};
```

---

# **5. Lambdas with Collections**

Sorting:

Before:

```java
Collections.sort(list, new Comparator<String>() {
    public int compare(String a, String b) {
        return a.length() - b.length();
    }
});
```

With lambda:

```java
list.sort((a, b) -> a.length() - b.length());
```

Filtering:

```java
list.stream()
    .filter(s -> s.startsWith("A"))
    .forEach(System.out::println);
```

---

# **6. Lambda + Thread Example**

```java
Thread t = new Thread(() -> System.out.println("Thread running"));
t.start();
```

Replaces anonymous Runnable.

---

# **7. Important Concept: Variable Capture**

Lambda can use **final or effectively final variables**:

```java
int x = 10;

Runnable r = () -> System.out.println(x);
```

If you modify `x` later → compile error.

---

# **8. Method Reference (Shortcut of Lambda)**

Lambda:

```java
s -> System.out.println(s)
```

Method reference:

```java
System.out::println
```

---

# **9. When NOT to Use Lambdas**

- When code inside lambda becomes too long
    
- When multiple statements reduce readability
    
- When debugging becomes difficult
    

---

# **10. Interview Questions (with Answers)**

### **Q1. What is a lambda expression?**

A lambda is an anonymous function that allows passing behavior as data.  
It works only with functional interfaces.

---

### **Q2. What is a functional interface?**

An interface with **exactly one abstract method** (SAM).  
Example: Runnable, Callable, Comparator.

---

### **Q3. Difference between anonymous class and lambda?**

|Anonymous Class|Lambda|
|---|---|
|Creates new class file|No new class|
|Can override multiple methods|Only 1 abstract method|
|Verbose|Short & clean|
|Has its own `this`|Shares outer `this`|

---

### **Q4. What is “effective final”?**

A variable not declared final but **not modified** after initialization.

---

### **Q5. Can lambda access instance variables?**

Yes, it can access instance variables and static variables normally.

---

### **Q6. Can lambda throw checked exceptions?**

Yes, but the functional interface method must declare them.

---
