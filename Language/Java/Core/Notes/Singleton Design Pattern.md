## 1. Overview

**Design Pattern Type:** Creational  
**Purpose:** Control object creation  
**Core Idea:** Only one instance of a class should exist in the JVM, and it should be globally accessible.

---

## 2. Definition

> The Singleton Design Pattern ensures that a class has **exactly one instance** and provides a **global point of access** to that instance.

---

## 3. Why Singleton Is Needed (Problem Statement)

Without Singleton:

- Multiple objects may be created unintentionally
    
- Shared resources become inconsistent
    
- Memory and performance are wasted
    
- System-wide coordination becomes difficult
    

### Example Problems

- Multiple loggers writing to the same file
    
- Multiple configuration loaders with different states
    
- Multiple database connection managers
    

Singleton solves this by enforcing **one controlled instance**.

---

## 4. When to Use Singleton

Use Singleton when:

- Exactly one object is required
    
- Object creation is expensive
    
- Object manages shared system resources
    

### Common Real-World Use Cases

- Logger
    
- Configuration manager
    
- Cache manager
    
- Thread pool
    
- Spring services (stateless)
    

---

## 5. Core Rules of Singleton

A Singleton class must follow **all three rules**:

### Rule 1: Private Constructor

Prevents external instantiation.

```java
private Singleton() { }
```

---

### Rule 2: Private Static Instance

Holds the single object.

```java
private static Singleton instance;
```

---

### Rule 3: Public Static Access Method

Provides controlled access.

```java
public static Singleton getInstance()
```

---

## 6. Singleton Implementations

---

### 6.1 Lazy Initialization (Not Thread-Safe)

```java
class Singleton {

    private static Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### Problem

In a multi-threaded environment, two threads may create two instances.

---

### 6.2 Thread-Safe Singleton (Synchronized Method)

```java
class Singleton {

    private static Singleton instance;

    private Singleton() { }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### How It Works

- Only one thread can execute the method at a time
    

#### Drawback

- Every call acquires a lock
    
- Performance degradation
    

---

### 6.3 Double-Checked Locking (Best Classic Approach)

```java
class Singleton {

    private static volatile Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

#### Why This Works

- Lock is acquired only during first initialization
    
- High performance
    
- Fully thread-safe
    

#### Why `volatile` Is Required

- Prevents instruction reordering
    
- Ensures visibility of changes across threads
    

---

### 6.4 Eager Initialization

```java
class Singleton {

    private static final Singleton INSTANCE = new Singleton();

    private Singleton() { }

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

#### Pros

- Thread-safe
    
- Simple implementation
    

#### Cons

- Instance created even if not used
    

---

### 6.5 Enum Singleton (Best Overall)

```java
enum Singleton {
    INSTANCE;

    public void execute() {
        System.out.println("Singleton logic");
    }
}
```

#### Why Enum Singleton Is Best

- Thread-safe by JVM
    
- Serialization-safe
    
- Reflection-safe
    
- Minimal code
    
- Recommended in _Effective Java_
    

---

## 7. How Singleton Can Be Broken

---

### 7.1 Using Reflection

```java
Constructor<Singleton> c =
        Singleton.class.getDeclaredConstructor();
c.setAccessible(true);

Singleton s1 = Singleton.getInstance();
Singleton s2 = c.newInstance();
```

This creates a second instance.

---

### 7.2 Using Serialization

Deserialization creates a new object.

---

### 7.3 Fixing Serialization Issue

```java
protected Object readResolve() {
    return instance;
}
```

Enum Singleton automatically prevents both issues.

---

## 8. Singleton vs Static (Interview Question)

|Aspect|Singleton|Static|
|---|---|---|
|Nature|Object|Class-level|
|Lazy loading|Yes|No|
|Polymorphism|Supported|Not supported|
|Interface|Can implement|Cannot|
|Testability|Can be mocked|Hard to mock|

---

## 9. Singleton in Spring Boot

### Default Bean Scope

Spring beans are **singleton by default**.

```java
@Service
public class UserService {
}
```

- One instance per Spring ApplicationContext
    
- Managed by IoC container
    

---

### Spring Singleton vs Classic Singleton

|Aspect|Classic|Spring|
|---|---|---|
|Control|Manual|Container|
|Scope|JVM|ApplicationContext|
|Lifecycle|Developer|Framework|

---

### Changing Scope

```java
@Scope("prototype")
@Service
class TestService { }
```

---

## 10. When NOT to Use Singleton

Avoid Singleton when:

- Object holds mutable shared state
    
- Object is user-specific
    
- Object is request-scoped
    
- Building microservices (prefer stateless design)
    

---

## 11. Interview Questions and Answers

**Q1. Why constructor is private?**  
To prevent object creation using `new`.

**Q2. Is Singleton thread-safe by default?**  
No, depends on implementation.

**Q3. Best Singleton implementation?**  
Enum Singleton.

**Q4. Are Spring beans Singleton?**  
Yes, by default.

**Q5. Can Singleton be garbage collected?**  
Yes, if no references exist.

---

## 12. Real-World Spring-Style Example

```java
@Service
class ConfigService {

    private final Map<String, String> config = new HashMap<>();

    public String getValue(String key) {
        return config.get(key);
    }
}
```

- Single instance
    
- Shared configuration
    
- Managed by Spring
    

---

## 13. Key Takeaways

- Singleton controls object creation
    
- Thread safety is critical
    
- Enum Singleton is the safest
    
- Spring already implements Singleton at container level
    
- Avoid stateful singletons
    

---

## 14. One-Line Summary

> Singleton ensures one and only one instance of a class with controlled global access.

---
