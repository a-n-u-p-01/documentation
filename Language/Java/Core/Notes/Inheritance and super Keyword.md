Inheritance is one of the **four pillars of OOP** (the others are Encapsulation, Polymorphism, and Abstraction).  
It allows a new class to **reuse**, **extend**, and **enhance** the functionality of an existing class.

---

# **1. What is Inheritance?**

Inheritance is the process where one class **acquires the properties and behaviors** (fields + methods) of another class.

Java uses the **extends** keyword:

```
class Child extends Parent { }
```

Here:

- **Parent (Superclass/Base class)** → gives features
    
- **Child (Subclass/Derived class)** → receives and adds new features
    

---

# **2. Why Use Inheritance?**

1. **Code Reusability**  
    Reuse existing logic instead of rewriting.
    
2. **Method Overriding (Runtime Polymorphism)**  
    Allows a child class to modify parent behavior.
    
3. **Extensibility**  
    Add additional features to existing classes.
    
4. **Hierarchical Structure**  
    Models real-world relationships (“is-a” relation).
    

---

# **3. Types of Inheritance (Supported by Java)**

Java supports:

1. **Single Inheritance**
    
    ```
    A → B
    ```
    
2. **Multilevel Inheritance**
    
    ```
    A → B → C
    ```
    
3. **Hierarchical Inheritance**
    
    ```
       A
      / \
     B   C
    ```
    

### Java **does NOT support multiple inheritance** with classes:

```
class A extends B, C  // NOT ALLOWED
```

Reason: to avoid **Diamond Problem**.

Multiple inheritance is allowed through **interfaces**, not classes.

---

# **4. Basic Example of Inheritance**

```java
class Animal {
    void eat() { 
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}
```

Usage:

```java
Dog d = new Dog();
d.eat();   // inherited from Animal
d.bark();  // Dog's own method
```

---

# **5. What is the `super` Keyword?**

`super` refers to the **immediate parent class** of the current object.

You can use `super` to:

1. Access **parent class variables**
    
2. Access **parent class methods**
    
3. Call **parent class constructor**
    

---

# **6. Using super to Access Parent Variables**

If parent and child have variables with the same name:

```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;

    void show() {
        System.out.println(x);        // child's x
        System.out.println(super.x);  // parent's x
    }
}
```

Output:

```
20
10
```

---

# **7. Using super to Call Parent Methods**

```java
class Parent {
    void message() {
        System.out.println("Parent message");
    }
}

class Child extends Parent {
    void message() {
        System.out.println("Child message");
    }

    void display() {
        super.message();  // calls parent's message
        message();        // child's message
    }
}
```

---

# **8. Using super to Call Parent Constructor**

`super()` must be the **first statement** in child constructor.

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // calls Parent()
        System.out.println("Child constructor");
    }
}
```

Output:

```
Parent constructor
Child constructor
```

If you don't write super(), Java automatically inserts it **only for the no-arg parent constructor**.

---

# **9. Constructor Chaining with super**

Important:  
Every constructor **must** call either:

- another constructor in same class → `this()`, OR
    
- parent constructor → `super()`
    

Example:

```java
class A {
    A(int x) {
        System.out.println("A: " + x);
    }
}

class B extends A {
    B() {
        super(10);
        System.out.println("B constructor");
    }
}
```

---

# **10. Protected Members and Inheritance**

Protected members are accessible in:

- Same package
    
- Subclasses (even in different packages)
    

Example:

```java
protected int value;
```

The subclass can directly access it.

---

# **11. IS-A Relationship (Very Important)**

Inheritance creates an **IS-A relationship**:

```
Dog IS-A Animal
Car IS-A Vehicle
Teacher IS-A Person
```

This is key in OOP design.

Example:

```java
Animal a = new Dog();  // valid
```

This is the basis of **polymorphism**.

---

# **12. What Cannot Be Inherited?**

A child class **cannot inherit**:

- Constructors
    
- Private members
    
- Final methods
    
- Final classes
    
- Static blocks (not inherited, but shared)
    

---

# **13. final and Inheritance**

1. **final class** → cannot be inherited
    
    ```
    final class A { }
    class B extends A { }  // error
    ```
    
2. **final method** → cannot be overridden
    
    ```
    final void show() { }
    ```
    
3. **final variable** → inherited but cannot be changed
    

---

# **14. Method Overriding (Quick Relation Note)**

Overriding happens in inheritance. Example:

```java
class Parent { void test() {} }
class Child extends Parent { void test() {} }
```

Child’s version is used at **runtime**.

Inheritance enables **runtime polymorphism**.

---

# **15. Real-Life Example of Inheritance**

### Banking Example:

```java
class Account {
    void calculateInterest() { }
}

class SavingsAccount extends Account {
    void calculateInterest() { /* specific logic */ }
}

class CurrentAccount extends Account {
    void calculateInterest() { /* different logic */ }
}
```

All accounts share base features but override logic.

---

# **16. Advantages and Disadvantages of Inheritance**

## Advantages:

- Reuse code
    
- Reduces duplication
    
- Enables polymorphism
    
- Improves maintainability
    

## Disadvantages:

- Too much inheritance → complex hierarchy
    
- Tight coupling between parent and child
    
- Difficult to change parent without affecting children
    

---

# **17. Interview Questions (Must Learn)**

1. What is inheritance?
    
2. Why does Java not support multiple inheritance?
    
3. What is multilevel inheritance?
    
4. What is hierarchical inheritance?
    
5. What is the `super` keyword used for?
    
6. Can a constructor be inherited?
    
7. Can private members be inherited?
    
8. Difference between `super()` and `this()`?
    
9. What is IS-A and HAS-A relationship?
    
10. What are the drawbacks of inheritance?
    

---

# **18. Summary**

- Inheritance allows one class to acquire another’s members using `extends`.
    
- `super` is used to access parent variables, methods, and constructors.
    
- Java supports single, multilevel, and hierarchical inheritance.
    
- Does not support multiple inheritance with classes.
    
- Inheritance is essential for polymorphism and clean design.
    

---
