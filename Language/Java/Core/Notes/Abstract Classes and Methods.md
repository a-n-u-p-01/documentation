# **1. What is an Abstract Class?**

An **abstract class** is a class that:

- **Cannot be instantiated** (you cannot create its object)
    
- May contain **abstract methods** (methods without body)
    
- May also contain **non-abstract methods** (normal methods)
    
- Is used to provide a **base/template** for all subclasses
    
- Is declared using the `abstract` keyword
    

Example:

```java
abstract class Animal {
    abstract void sound(); // abstract method
    void sleep() {         // concrete method
        System.out.println("Sleeping...");
    }
}
```

You cannot do:

```java
Animal a = new Animal();  // Error
```

---

# **2. What is an Abstract Method?**

An **abstract method**:

- Has **no body**
    
- Ends with a semicolon
    
- Must be overridden in a subclass
    
- Can exist only inside an abstract class or interface
    

```java
abstract void sound();
```

---

# **3. Why Use Abstract Classes?**

1. **Define common templates** for subclasses
    
2. Provide **partial abstraction**
    
3. Force subclasses to implement specific methods
    
4. Reuse code in subclasses
    
5. Build frameworks and enforce standard operations
    

Example:  
Payment → CreditCard, UPI, Paypal  
Animal → Dog, Cat  
Shape → Circle, Rectangle

---

# **4. Example of Abstract Class and Subclass Implementation**

```java
abstract class Animal {
    abstract void sound();  // must be implemented

    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Bark");
    }
}
```

Usage:

```java
Animal a = new Dog();
a.sound();
a.eat();
```

---

# **5. Abstract Class vs Concrete Class**

|Feature|Concrete Class|Abstract Class|
|---|---|---|
|Instantiation|Possible|Not possible|
|Abstract Methods|Not allowed|Allowed|
|Concrete Methods|Allowed|Allowed|
|Constructors|Allowed|Allowed|
|Usage|Complete object|Base template|

---

# **6. Abstract Class Can Have Constructors**

Although you cannot create its object directly, an abstract class **can have constructors**, and they are called when a subclass object is created.

Example:

```java
abstract class A {
    A() {
        System.out.println("A constructor");
    }
}

class B extends A {
    B() {
        System.out.println("B constructor");
    }
}
```

Creating:

```java
B b = new B();
```

Output:

```
A constructor
B constructor
```

---

# **7. Abstract Class Can Also Have:**

- Variables (including private, protected, final)
    
- Methods (abstract + concrete)
    
- Static methods
    
- Static blocks
    
- Final methods (cannot be overridden)
    
- Any access modifiers
    

This is **unlike interfaces**, which were limited before Java 8.

---

# **8. Can an Abstract Class have 0 Abstract Methods?**

YES.

Example:

```java
abstract class Vehicle {
    void start() { }
}
```

Why?  
To **prevent instantiation** while still offering partial implementation.

---

# **9. Abstract Class and Polymorphism**

You can use an abstract class reference to point to subclass objects.

```java
Animal a = new Dog();
a.sound();
```

This achieves **runtime polymorphism**.

---

# **10. Real-World Example: Shape Framework**

```java
abstract class Shape {
    abstract double area();
}

class Circle extends Shape {
    double radius;
    Circle(double r){ this.radius = r; }
    double area(){ return Math.PI * radius * radius; }
}

class Rectangle extends Shape {
    double area(){ return 10 * 20; }
}
```

Usage:

```java
Shape s = new Circle(5);
System.out.println(s.area());
```

---

# **11. Abstract Class vs Interface (Most Important)**

|Feature|Abstract Class|Interface|
|---|---|---|
|Methods|Abstract + Concrete|Until Java 7: abstract only; Java 8+: default + static; Java 9+: private methods|
|Multiple Inheritance|Not supported|Supported|
|Variables|Any type|public static final only|
|Constructor|Allowed|Not allowed|
|When to Use|Shared code + some abstraction|Full abstraction or contract|

**Summary:**

- Use **abstract class** when classes **share behavior/code**.
    
- Use **interface** when you need a **contract** or **multiple inheritance**.
    

---

# **12. Rules and Restrictions**

1. A class with even **one abstract method must be abstract**.
    
2. You cannot create objects of abstract classes.
    
3. Subclass must override all abstract methods unless the subclass is also abstract.
    
4. Abstract methods **cannot be private**, because they must be overridden.
    
5. Abstract methods cannot be **final**, because final prevents overriding.
    

---

# **13. Abstract Classes in Multilevel Inheritance**

```java
abstract class A {
    abstract void m1();
}

abstract class B extends A {
    void m2() { }
}

class C extends B {
    void m1() { }  // must be implemented here
}
```

---

# **14. Interview Questions**

1. What is an abstract class?
    
2. Why can’t we instantiate an abstract class?
    
3. Can an abstract class have constructors?
    
4. Can abstract classes contain static methods?
    
5. Difference between abstract class and interface?
    
6. Can an abstract class have no abstract method?
    
7. Can abstract methods be private or final?
    
8. When should you use inheritance vs abstraction?
    

---

# **15. Summary**

- Abstract class = template + partial abstraction
    
- Cannot be instantiated
    
- May contain abstract + concrete methods
    
- Constructors are allowed
    
- Supports single inheritance
    
- Best when subclasses require **shared logic**
    
- Interfaces are preferred for **full abstraction**
    

---
