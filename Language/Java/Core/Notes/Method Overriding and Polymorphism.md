## **1. What is Method Overriding?**

**Method Overriding** occurs when a **subclass provides its own implementation** of a method that already exists in the **parent class** with the **same signature**.

A method is overridden when:

1. Same method name
    
2. Same return type (or covariant return type)
    
3. Same parameter list
    
4. Occurs between parent and child classes
    
5. Child method has **equal or greater access** than parent method
    

Example:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

---

# **2. Why Do We Use Method Overriding?**

1. To **provide specific behavior** in subclass
    
2. To achieve **Runtime Polymorphism**
    
3. To implement core OOP concepts like Abstraction and Dynamic Method Dispatch
    
4. To modify or extend functionality of parent class
    

---

# **3. Rules for Method Overriding**

### 3.1 Method Signature Must Match

Same name + same arguments.

### 3.2 Return Type Must Match

Or be a **covariant return type**:

```java
class Parent {
    Object show() { return null; }
}
class Child extends Parent {
    String show() { return "Hello"; }  // allowed
}
```

### 3.3 Access Modifier Rules

|Parent Method|Child Method|
|---|---|
|public|public|
|protected|protected or public|
|default|default, protected, or public|
|private|cannot be overridden|

Important:  
**You cannot reduce the visibility** in child.

### 3.4 final Methods Cannot Be Overridden

```java
final void test() { } // cannot override
```

### 3.5 static Methods Cannot Be Overridden

But they can be **hidden**, not overridden.

### 3.6 Constructors Cannot Be Overridden

Because constructors are **not inherited**.

---

# **4. What is Polymorphism?**

Polymorphism = **Many forms**.

In Java, two types:

### 1. Compile-time polymorphism → Method Overloading

### 2. Runtime polymorphism → Method Overriding

This chapter deals with **runtime polymorphism**.

---

# **5. Runtime Polymorphism (Dynamic Method Dispatch)**

Java resolves overridden methods **at runtime**, not compile-time.

```java
Animal animal = new Dog();
animal.sound(); 
```

Which `sound()` is called?

→ JVM checks the **actual object type (Dog)**, not reference type (Animal).

This is **dynamic dispatch**.

### Example:

```java
Animal a = new Dog();
a.sound();  // Dog barks

a = new Cat();
a.sound();  // Cat meows
```

This is the power of runtime polymorphism.

---

# **6. Why Runtime Polymorphism is Needed?**

1. Code Flexibility
    
2. Decoupling behavior from references
    
3. Using parent references to hold child objects
    
4. Achieving abstraction
    
5. Designing extensible systems (like Spring, JDBC drivers, frameworks)
    

---

# **7. super Keyword in Overriding**

You can call parent version of overridden method:

```java
class Dog extends Animal {
    @Override
    void sound() {
        super.sound(); // optional
        System.out.println("Dog barks");
    }
}
```

---

# **8. Upcasting and Downcasting with Polymorphism**

### Upcasting

Parent reference → child object (allowed)

```java
Animal a = new Dog();
```

### Downcasting

Child reference → from parent reference (explicit, risky)

```java
Dog d = (Dog) a;
```

If a is not actually Dog → **ClassCastException**

---

# **9. Real-Life Example: Payment System**

```java
class Payment {
    void pay() {
        System.out.println("General payment");
    }
}

class CreditCard extends Payment {
    void pay() {
        System.out.println("Paid with Credit Card");
    }
}

class UPI extends Payment {
    void pay() {
        System.out.println("Paid using UPI");
    }
}
```

Usage:

```java
Payment p;

p = new CreditCard();
p.pay(); // Credit card

p = new UPI();
p.pay(); // UPI
```

Flexible, extensible design → new payment types can be added easily.

---

# **10. Covariant Return Types (Important)**

Child class method can return a **subtype** of parent return type.

```java
class A {
    A show() { return this; }
}
class B extends A {
    B show() { return this; }
}
```

Useful for method chaining and fluent APIs.

---

# **11. Method Overriding vs Method Overloading**

|Feature|Overloading|Overriding|
|---|---|---|
|Polymorphism Type|Compile-time|Runtime|
|Parameter List|Must differ|Must match|
|Return Type|Can differ|Must match / covariant|
|Occurs In|Same class|Parent-child|
|Access Modifier|Can change|Cannot reduce visibility|
|Purpose|Flexibility|Specialization|

---

# **12. Mistakes Beginners Make**

1. Changing parameter types and thinking it is overriding  
    (It becomes overloading)
    
2. Reducing access modifier
    
3. Changing return type improperly
    
4. Forgetting @Override annotation  
    (Very useful—it detects mistakes!)
    

---

# **13. @Override Annotation**

Not required, but best practice.

It:

- Prevents mistakes
    
- Improves clarity
    
- Tells compiler to validate overriding
    

Example:

```java
@Override
void sound() { }
```

If signature is wrong → compiler error.

---

# **14. When Not to Use Inheritance/Overriding**

- When the new class is **not truly an IS-A** relationship
    
- When composition would serve better
    
- When overriding only to disable parent method
    
- When inheritance increases complexity unnecessarily
    

Prefer **HAS-A** (composition) over **IS-A** when possible.

---

# **15. Interview Questions (Very Important)**

1. What is method overriding?
    
2. What is runtime polymorphism?
    
3. What is dynamic method dispatch?
    
4. Why is method overriding needed?
    
5. Can we override static methods?
    
6. Can we override constructors?
    
7. Difference between overriding and overloading.
    
8. What is covariant return type?
    
9. What access modifier rules apply in overriding?
    
10. Why does JVM decide overridden method at runtime?
    

---

# **16. Summary**

- Overriding allows a subclass to redefine parent behavior.
    
- It enables runtime polymorphism (dynamic method dispatch).
    
- Uses IS-A relationship through inheritance.
    
- @Override annotation ensures correctness.
    
- Covariant return types allow cleaner design.
    
- Overriding is foundational for frameworks, APIs, and polymorphic code.
    

---