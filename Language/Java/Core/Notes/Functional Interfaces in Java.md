**Functional Programming (FP)** is a programming style where:

- Computation is treated as **evaluation of functions**
    
- Functions are treated as **values**
    
- Code focuses on **what to do**, not **how to do it**
    
- State changes and mutable data are **minimized**
    

Java introduced functional programming features mainly in **Java 8**.

---

## Why Functional Programming was added to Java

Before Java 8:

- Code was **imperative** (step-by-step instructions)
    
- Harder to write parallel and clean code
    
- Lots of boilerplate (anonymous classes)
    

After Java 8:

- Cleaner code
    
- Better support for parallel processing
    
- Easier handling of collections
    
- Less mutable shared state → fewer bugs
    

---

## Core Ideas of Functional Programming

### 1. Functions as First-Class Citizens

A function can be:

- Stored in a variable
    
- Passed as an argument
    
- Returned from another function
    

In Java, this is achieved using:

- **Lambda expressions**
    
- **Functional interfaces**
    

Example:

```java
Runnable r = () -> System.out.println("Hello");
```

Here, a function is stored in a variable.

---

### 2. Immutability

Once data is created, it should not be changed.

Instead of:

```java
count = count + 1;
```

We prefer:

```java
newCount = count + 1;
```

Benefits:

- Thread-safe by default
    
- Easier reasoning
    
- Fewer side effects
    

---

### 3. No Side Effects (or Minimal)

A function should:

- Not modify external variables
    
- Always produce the same output for the same input
    

Bad example:

```java
int sum = 0;
void add(int x) {
    sum += x; // side effect
}
```

Good example:

```java
int add(int a, int b) {
    return a + b;
}
```

---

### 4. Declarative Style

Instead of telling **how** to do something, you tell **what** you want.

Imperative:

```java
for (int i : list) {
    if (i > 10) {
        System.out.println(i);
    }
}
```

Functional:

```java
list.stream()
    .filter(i -> i > 10)
    .forEach(System.out::println);
```

---

## Functional Programming in Java is Built Using These Concepts

Java does not become 100% functional, but supports FP using:

1. Lambda Expressions
    
2. Functional Interfaces
    
3. Stream API
    
4. Method References
    
5. Optional
    
6. Immutable data patterns
    

---

## 1. Lambda Expressions

Lambda = **anonymous function**

Syntax:

```java
(parameters) -> expression
```

Example:

```java
(a, b) -> a + b
```

Replaces:

```java
new Comparator<Integer>() {
    public int compare(Integer a, Integer b) {
        return a - b;
    }
}
```

---

## 2. Functional Interfaces (Very Important)

A **functional interface** has:

- Exactly **one abstract method**
    

Examples:

- Runnable
    
- Callable
    
- Comparator
    
- Predicate
    
- Function
    
- Consumer
    
- Supplier
    

Example:

```java
@FunctionalInterface
interface MyFunction {
    int add(int a, int b);
}
```

Used with lambda:

```java
MyFunction f = (a, b) -> a + b;
```

---

## 3. Stream API

Streams allow functional-style operations on collections.

Example:

```java
list.stream()
    .filter(x -> x > 10)
    .map(x -> x * 2)
    .forEach(System.out::println);
```

Key points:

- Streams do not modify original data
    
- Operations are lazy
    
- Supports parallel execution
    

---

## 4. Method References

Shortcut for lambdas when calling existing methods.

Example:

```java
System.out::println
```

Instead of:

```java
x -> System.out.println(x)
```

Types:

- Static method reference
    
- Instance method reference
    
- Constructor reference
    

---

## 5. Optional

Used to avoid `NullPointerException`.

Instead of:

```java
if (obj != null) {
    obj.getName();
}
```

Use:

```java
Optional.ofNullable(obj)
        .map(Object::getName)
        .ifPresent(System.out::println);
```

---

## Functional Programming vs OOP (Java Context)

|OOP|Functional Programming|
|---|---|
|Focus on objects|Focus on functions|
|Mutable state|Immutable data|
|Behavior tied to data|Behavior passed as functions|
|Harder parallelism|Easier parallelism|

Java uses **both together**, not either-or.

---

## Where Functional Programming is Used in Real Projects

- Filtering and transforming collections
    
- Parallel data processing
    
- Event handling
    
- Callback implementations
    
- Asynchronous programming
    
- Cleaner multithreaded code
    

---

## Interview-Level Understanding

### Why is Functional Programming safer for concurrency?

Because immutable data and no side effects reduce race conditions.

### Is Java a functional language?

No. Java is **object-oriented with functional features**.

### Why functional style is preferred with streams?

Less code, fewer bugs, better readability, easy parallelization.

---

## Very Short Summary

- Functional Programming focuses on **functions, not state**
    
- Java supports FP from Java 8
    
- Lambdas + Functional Interfaces are the foundation
    
- Streams make data processing clean and safe
    
- Immutability and no side effects are key goals
    

---

### Next logical topic (after this):

**Functional Interfaces in Java**  
(Consumer, Predicate, Function, Supplier in depth)

If you want, just say: **next note**