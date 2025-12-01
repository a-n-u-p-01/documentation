These are **important modifiers** in Java that influence **variable behavior, class/method behavior, and memory visibility**. Understanding them is crucial for **OOP, multithreading, and performance**.

---

#### **Final Keyword**

The `final` keyword is used to **restrict modification**.

#### **Uses of Final**

**Final Variable** – Value cannot be changed once assigned:

```java
final int MAX = 100;
MAX = 200; // ❌ compile-time error
```

- Can be **instance variables, local variables, or parameters**.
    
- Blank `final` variables can be assigned **once in constructor** (for instance fields).
    

```java
class Test {
    final int x;
    Test(int val) { x = val; } // OK
}
```

**Final Method** – Cannot be **overridden**:

```java
class Parent {
    final void show() {
        System.out.println("Parent method");
    }
}
class Child extends Parent {
    void show() { } // ❌ compile-time error
}
```

**Final Class** – Cannot be **subclassed**:

```java
final class Utility { }
class MyUtil extends Utility { } // ❌ compile-time error
```

**Important Notes:**

- Helps achieve **immutability** (e.g., `String` class).
    
- Can improve **performance** (compiler may inline final variables).
    

---

## **Static Keyword**

The `static` keyword belongs to the **class rather than an instance**.

### **Uses of Static**

**Static Variable (Class Variable)** – Shared among all instances:

```java
class Counter {
    static int count = 0;
    Counter() { count++; }
}
Counter c1 = new Counter();
Counter c2 = new Counter();
System.out.println(Counter.count); // 2
```

**Static Method (Class Method)** – Can be called **without creating an object**:

```java
class Utils {
    static void greet() { System.out.println("Hello"); }
}
Utils.greet(); // OK
```

- Cannot access **instance variables/methods directly**.
    

**Static Block** – Executes **once when class is loaded**:

```java
class Init {
    static { System.out.println("Class loaded"); }
}
```

**Static Inner Class** – Nested class that **does not require an outer instance**:

```java
class Outer {
    static class Inner {
        void show() { System.out.println("Static Inner"); }
    }
}
Outer.Inner in = new Outer.Inner();
```

**Important Notes:**

- Static members are stored in **Method Area** (class memory).
    
- Useful for **constants**: `static final int MAX = 100;`
    

---

## **Volatile Keyword**

The `volatile` keyword ensures **visibility of changes across threads**.

```java
class Shared {
    volatile boolean flag = false;
}
```

### **Characteristics of Volatile Variables**

- Always read from **main memory**, not CPU cache.
    
- Guarantees **visibility**, but **not atomicity** for compound operations:
    

```java
volatile int counter = 0;
counter++; // ❌ not atomic, may cause race conditions
```

- Often used as a **flag** to stop threads:
    

```java
class Worker extends Thread {
    volatile boolean running = true;
    public void run() {
        while(running) { /* do work */ }
    }
}
```

**Important Notes:**

- Use for **shared variables in multithreaded environments**.
    
- Cannot replace **synchronization** for compound operations.
    
- Works best for **simple read/write operations**.
    

---

## **Combination of Keywords**

- **static final** → Class-level constant (immutable):
    

```java
static final double PI = 3.14159;
```

- **final static methods** → Cannot override; belong to class.
    
- **volatile static** → Shared and visible across threads:
    

```java
static volatile boolean stop = false;
```

---

## **Common Pitfalls**

- Confusing **final vs immutable**: final reference cannot change, but object itself may be mutable.
    
- Volatile does **not** make operations atomic (e.g., increment, decrement).
    
- Static variables may lead to **memory leaks** if objects are referenced unintentionally.
    
- Combining `final` and `volatile` doesn’t make sense for primitive type fields (`final` cannot be reassigned).
    

---

## **Interview Questions**

- Difference between `final`, `finally`, and `finalize`?
    
    - `final` → keyword for constants, methods, classes.
        
    - `finally` → block executed in try-catch.
        
    - `finalize()` → method called by GC before object destruction.
        
- Can a static method be final?
    
    - Yes, it cannot be overridden (static methods are class-level).
        
- What happens if a final variable is **not initialized**?
    
    - Compiler error, unless initialized in constructor (for instance variable).
        
- Difference between `volatile` and `synchronized`?
    
    - `volatile` → ensures visibility of changes.
        
    - `synchronized` → ensures visibility **and atomicity**.
        
- Can a final method be static?
    
    - Yes.
        
- Can a static class be final?
    
    - Only **nested static classes** can be final; top-level class cannot be both static and final.
        
- When would you use `volatile` over `synchronized`?
    
    - Use for **flags or simple read/write shared variables**, not compound operations.
        
