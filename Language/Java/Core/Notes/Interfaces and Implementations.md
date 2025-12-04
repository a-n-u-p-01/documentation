# **1. What is an Interface?**

An **interface** in Java is:

- A **completely abstract type** (until Java 7; Java 8+ allows default and static methods)
    
- A **contract** that defines what a class **must implement**
    
- Declared using the `interface` keyword
    
- Cannot contain instance fields (except `public static final` constants)
    
- Supports **multiple inheritance** for types (unlike classes)
    

Example:

```java
interface Vehicle {
    void start();
    void stop();
}
```

---

# **2. Key Features of Interfaces**

1. **Abstract Methods Only (pre-Java 8)**  
    All methods in an interface are implicitly `public abstract`.
    
2. **Constants Only**  
    Fields are implicitly `public static final`.
    
3. **Multiple Inheritance**  
    A class can implement multiple interfaces:
    
    ```java
    interface A { void m1(); }
    interface B { void m2(); }
    
    class C implements A, B {
        public void m1() { System.out.println("m1"); }
        public void m2() { System.out.println("m2"); }
    }
    ```
    
4. **Default Methods (Java 8+)**  
    Can have a body using `default` keyword:
    
    ```java
    interface MyInterface {
        default void greet() {
            System.out.println("Hello");
        }
    }
    ```
    
5. **Static Methods (Java 8+)**  
    Can be called using the interface name:
    
    ```java
    interface Util {
        static void info() { System.out.println("Utility method"); }
    }
    Util.info();
    ```
    
6. **Private Methods (Java 9+)**  
    Can define private helper methods for **default or static methods**.
    

---

# **3. Implementing an Interface**

- Use `implements` keyword
    
- Must provide **body for all abstract methods**
    
- Access modifier must be **public** (cannot reduce visibility)
    

Example:

```java
interface Vehicle {
    void start();
    void stop();
}

class Car implements Vehicle {
    public void start() { System.out.println("Car starting"); }
    public void stop() { System.out.println("Car stopping"); }
}
```

---

# **4. Difference Between Abstract Class and Interface**

|Feature|Abstract Class|Interface|
|---|---|---|
|Inheritance|Single|Multiple allowed|
|Methods|Abstract + Concrete|Abstract (default/static/private allowed)|
|Fields|Any|public static final only|
|Constructor|Allowed|Not allowed|
|Access Modifier|Any|Methods are public by default|
|When to Use|Shared behavior + abstraction|Contract without code sharing|

---

# **5. Marker Interface**

- An interface **without any method**
    
- Used to **tag a class** for special behavior
    
- Example: `Serializable`, `Cloneable`, `Remote`
    

```java
class Student implements Serializable { }
```

- JVM or framework checks the marker and behaves differently.
    

---

# **6. Functional Interface (Java 8+)**

- Interface with **only one abstract method**
    
- Can have default/static/private methods
    
- Can be used with **Lambda Expressions**
    

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

Lambda example:

```java
Calculator add = (a, b) -> a + b;
System.out.println(add.calculate(5, 3));  // Output: 8
```

---

# **7. Multiple Interface Implementation Example**

```java
interface A { void a(); }
interface B { void b(); }

class C implements A, B {
    public void a() { System.out.println("A method"); }
    public void b() { System.out.println("B method"); }
}
```

- This allows **multiple inheritance** of type in Java
    

---

# **8. Default Method Overriding**

- Default methods can be **overridden in implementing class**.
    

```java
interface X {
    default void greet() { System.out.println("Hello from X"); }
}

class Y implements X {
    public void greet() { System.out.println("Hello from Y"); }
}
```

---

# **9. Diamond Problem and Interfaces**

- When a class implements two interfaces with **same default method**, compiler requires **explicit override**.
    
- Resolves **diamond problem** in multiple inheritance.
    

```java
interface A { default void hello() { System.out.println("A"); } }
interface B { default void hello() { System.out.println("B"); } }

class C implements A, B {
    public void hello() { A.super.hello(); }  // Explicitly choose A
}
```

---

# **10. Advantages of Interfaces**

1. Achieve **full abstraction**
    
2. Supports **multiple inheritance of type**
    
3. **Loose coupling** between modules
    
4. Standardizes method signatures for implementing classes
    
5. **Polymorphism** using interface references:
    

```java
Vehicle v = new Car();
v.start();
```

---

# **11. Interview Questions**

1. What is an interface in Java?
    
2. Difference between abstract class and interface.
    
3. Can interfaces have concrete methods?
    
4. What is a marker interface? Example?
    
5. Explain default and static methods in interface.
    
6. Can a class implement multiple interfaces?
    
7. How does Java solve diamond problem with interfaces?
    
8. What is a functional interface?
    

---

# **12. Summary**

- Interface = Contract + abstraction
    
- Provides **method signatures without implementation** (mostly)
    
- Supports **multiple inheritance**
    
- Marker interface = tagging
    
- Functional interface = used with lambda expressions
    
- Default & static methods = Java 8+
    
- Private methods = Java 9+
    

---
