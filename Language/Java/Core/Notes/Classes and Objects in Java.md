Java is a pure **object-oriented programming language**, and the concepts of **classes** and **objects** form its foundation. Understanding these correctly is essential before learning constructors, inheritance, polymorphism, or interfaces.

---

### **1. What is a Class in Java?**

A **class** is a **blueprint**, **template**, or **prototype** from which objects are created.
It defines:

- **State** → variables (fields)
    
- **Behavior** → methods
    
- **Constructors** → object initialization rules
    
- **Inner classes**, **blocks**, **interfaces**, etc. (optional)
    

A class does **not occupy memory** until objects are created from it.

### Example:

```java
class Car {
    // Fields (state)
    String brand;
    int speed;

    // Method (behavior)
    void drive() {
        System.out.println("Car is driving");
    }
}
```

---

# **2. What is an Object?**

An **object** is an **instance of a class**, and it represents a real-world entity. Each object has:

- Its own **state** (values of fields)
    
- Its own **identity** (unique memory address)
    
- Its own **behavior** (methods operate on object data)
    

Objects **consume memory**.

### Creating Objects in Java:

```java
Car car1 = new Car();
```

### What happens here internally?

1. `new` → allocates memory on heap
    
2. `Car()` → calls constructor
    
3. Reference `car1` stores the address of the object
    

---

# **3. Class Members**

A class typically contains:

### **1. Fields (Instance Variables)**

Variables whose values are **unique for each object**.

```java
String color;
int modelYear;
```

### **2. Static Variables (Class Variables)**

Shared across all objects of a class.

```java
static int numberOfCars;
```

### **3. Methods**

Define behavior.

```java
void startEngine() { ... }
```

### **4. Constructors**

Special methods to initialize objects.

```java
Car() { ... }
```

### **5. Blocks / Initializers**

Optional initialization code.

### **6. Nested / Inner Classes (optional)**

---

# **4. Creating and Using Objects**

### Declaration + Instantiation + Initialization:

```java
Car myCar = new Car();
myCar.brand = "BMW";
myCar.speed = 120;
myCar.drive();
```

### Accessing members:

- Using dot `.` operator.
    
- Access modifiers decide visibility.
    

---

# **5. The 'this' Reference (Important for Objects)**

`this` refers to the **current object** inside a method or constructor.

Uses:

1. Distinguish between local and instance variables
    
2. Call other constructors
    
3. Return current object
    

Example:

```java
class Car {
    String brand;
    Car(String brand) {
        this.brand = brand;   // refers to instance variable
    }
}
```

---

# **6. Memory Allocation for Classes and Objects**

### 1. **Class Loading (JVM)**

- Classes are loaded by ClassLoader into **Method Area**.
    

### 2. **Object Creation**

- Objects live in the **Heap memory**.
    

### 3. **Reference Variables**

- Stored in **Stack memory** (inside methods).
    

**Diagram:**

```
Stack               Heap
-----               ------
car1  --->   [Car object]
car2  --->   [Car object]
```

---

# **7. Anonymous Objects**

Objects without a reference.

```java
new Car().drive();
```

Created and used immediately → becomes eligible for garbage collection.

---

# **8. Accessing Class Members (Rules)**

1. Instance variables → through object
    
2. Static variables → through class name
    

Example:

```java
Car.numberOfCars++;        // static
car1.speed = 100;          // instance
```

---

# **9. Class Types in Java**

### 1. **Concrete Class**

Normal class with full implementation.

### 2. **Abstract Class**

Cannot be instantiated; contains abstract methods.

### 3. **Final Class**

Cannot be inherited.

### 4. **POJO / Model / Entity Class**

Simple class for storing data (fields + getters/setters).

---

# **10. Real-World Analogy**

Class = Design of a House  
Object = Actual House

- The design exists once but can be used to build multiple houses.
    
- Houses consume space (memory); design does not.
    

---

# **11. Important Points About Classes and Objects**

- A class describes **what an object can have and do**.
    
- Objects are created using `new` operator.
    
- Multiple objects can be created from one class.
    
- Every object has a **unique identity** (memory address).
    
- Access modifiers control visibility of class members.
    
- Garbage Collector removes unused objects.
    

---

# **12. Interview Questions**

1. What is a class and object? Explain with example.
    
2. Difference between object and reference variable.
    
3. What is the memory allocation difference for class vs object?
    
4. What is `this` keyword and its uses?
    
5. Can a class exist without an object?
    
6. What is an anonymous object?
    
7. What are different types of classes in Java?
    

---

# **13. Summary**

- A **class** is the blueprint of an object.
    
- An **object** is an instance with its own data and behavior.
    
- Objects are created using the `new` keyword.
    
- Instance variables belong to objects; static variables belong to class.
    
- `this` refers to current object.
    
- Objects live on the heap; references live in stack.
    
- Classes support object-oriented principles like abstraction, encapsulation, inheritance, and polymorphism.
    

---