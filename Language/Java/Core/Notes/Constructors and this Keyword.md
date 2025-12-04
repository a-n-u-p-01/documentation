Constructors and the `this` keyword are foundational concepts in Java OOP. They control **object creation**, **initialization**, and **interaction with instance members**.

This note explains _everything_ from basics to advanced concepts with examples and internal behaviors.

---

### **1. What is a Constructor?**

A **constructor** is a special method that gets executed **automatically** when an object is created using the `new` keyword.

Its main purpose:  
**Initialize object state (assign initial values to instance variables).**

### Characteristics of Constructors:

1. Must have the **same name as the class**
    
2. Must **not have a return type** (not even void)
    
3. Automatically invoked during object creation
    
4. Cannot be called like normal methods
    
5. Can be overloaded
    
6. Cannot be inherited but can be invoked using `super()`
    

Example:

```java
class Car {
    Car() {
        System.out.println("Constructor called");
    }
}
```

Calling:

```java
Car c = new Car();  // Constructor called
```

---

# **2. Types of Constructors**

Java provides **two types** of constructors:

---

## **1. Default Constructor (Provided by Java)**

If **no constructor** is written, Java automatically provides a default constructor.

Example:

```java
class Example { }
```

Compiler converts to:

```java
class Example {
    Example() { }
}
```

### Features:

- Provided only when **no constructors are defined**
    
- Initializes fields to their **default values**
    

|Type|Default Value|
|---|---|
|int|0|
|float|0.0|
|boolean|false|
|object references|null|

---

## **2. No-Argument Constructor (User Defined)**

```java
class Student {
    Student() {
        System.out.println("No-arg constructor");
    }
}
```

Used when you want custom initialization logic.

---

## **3. Parameterized Constructor**

Used to initialize object with specific values.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Calling:

```java
Student s = new Student("John", 20);
```

---

# **3. Constructor Overloading**

Defining **multiple constructors** in the same class with different parameter lists.

```java
class Box {
    int width, height;

    Box() { width = 10; height = 10; }
    Box(int w) { width = w; height = w; }
    Box(int w, int h) { width = w; height = h; }
}
```

JVM decides which constructor to call based on arguments.

---

# **4. Constructor Chaining (Using this() and super())**

Constructor chaining means invoking one constructor from another.

Two types:

---

## **1. Within Same Class → Using this()**

```java
class A {
    A() {
        this(10);   // calls A(int)
        System.out.println("No-arg");
    }

    A(int x) {
        System.out.println("Param: " + x);
    }
}
```

Output:

```
Param: 10
No-arg
```

Rules for `this()`:

- Must be **first statement** inside constructor
    
- Cannot call multiple constructors simultaneously
    

---

## **2. Between Parent and Child (Inheritance) → Using super()**

`super()` calls **parent class constructor**.

```java
class Parent {
    Parent() { System.out.println("Parent"); }
}

class Child extends Parent {
    Child() { System.out.println("Child"); }
}
```

Output:

```
Parent
Child
```

Why?  
Every child object must initialize its **parent part** first.

---

# **5. Why Constructors Cannot Be Inherited?**

Because they are tightly bound to object creation and the class name.  
However, the child class **must explicitly call** the parent constructor using **super()**.

---

# **6. Private Constructors (Used in Singleton Pattern)**

A constructor can be private.

```java
class Singleton {
    private Singleton() { }
}
```

This prevents:

```java
new Singleton(); // Error
```

Useful for:

- Singleton pattern
    
- Factory methods
    
- Utility classes (Math, Arrays)
    

---

# **7. Copy Constructor (Not Built-in, Can Be Created Manually)**

Java does not provide built-in copy constructors, but you can implement them:

```java
class Student {
    String name;
    int age;

    Student(Student s) {
        this.name = s.name;
        this.age = s.age;
    }
}
```

---

# **8. Difference Between Constructor and Method**

|Feature|Constructor|Method|
|---|---|---|
|Name|Same as class|Any name|
|Return type|None|Must have return type|
|Called|Automatically|Explicitly|
|Purpose|Initialize object|Perform operation|
|Inheritance|Not inherited|Methods are inherited|
|Overloading|Allowed|Allowed|

---

# **9. Role of `new` Keyword**

When you write:

```java
Student s = new Student();
```

The `new` keyword performs:

1. Allocates memory on heap
    
2. Calls constructor
    
3. Returns object reference
    

Without `new`, constructors do not run.

---

# **10. What is `this` Keyword?**

`this` refers to the **current object**.

It is used inside instance methods and constructors.

---

# **11. Uses of this Keyword**

### **1. To refer to instance variables**

```java
class A {
    int x;
    A(int x) {
        this.x = x;  // distinguish instance variable from parameter
    }
}
```

---

### **2. To call another constructor → this()**

```java
this(10);
```

Must be first statement.

---

### **3. To return current object**

```java
A getObj() {
    return this;
}
```

---

### **4. To pass current object as argument**

```java
obj.display(this);
```

---

### **5. To avoid naming conflicts**

Without `this`, Java assumes local variable.

---

# **12. this vs super**

|Feature|this|super|
|---|---|---|
|Refers to|Current object|Parent object|
|Used in|Child & parent|Only in child|
|Access|Instance members|Parent members|
|Calling constructor|`this()`|`super()`|
|Cannot be used|In static context|In static context|

---

# **13. Important Rules About Constructors and this Keyword**

1. Constructor cannot be abstract, static, final, or synchronized
    
2. Constructor does not have a return type
    
3. `this()` and `super()` must be the **first statement**
    
4. If no constructor exists, Java creates default constructor
    
5. Constructors execute top-down in inheritance hierarchy
    
6. `this` cannot be used in static methods
    

---

# **14. Common Interview Questions**

1. Why do we use constructors?
    
2. What is constructor overloading?
    
3. Why must super() be the first statement?
    
4. Difference between this() and this.
    
5. Can a constructor be private?
    
6. Constructor vs method difference.
    
7. Real-world use of this keyword.
    
8. How constructor chaining works?
    
9. Why constructors are not inherited?
    

---

# **15. Summary**

- Constructors initialize new objects.
    
- Overloading allows multiple initialization patterns.
    
- this() → calls same class constructor.
    
- super() → calls parent class constructor.
    
- `this` refers to the current object.
    
- Private constructors restrict object creation.
    
- JVM automatically calls the appropriate constructor during object creation.
    

---

If you want, I can now give the next topic:  
**Method Overloading in Java**