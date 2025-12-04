Method Overloading is one of the core features of **compile-time polymorphism** (also called **static polymorphism**) in Java.

Java allows multiple methods with the **same name** but **different parameter lists** in the same class. This enables flexibility, code readability, and better API design.

---

# **1. What is Method Overloading?**

Method Overloading means defining **multiple methods** in the same class **with the same name**, but each having **different parameters**.

Java decides _which_ method to call **at compile time**, based on:

- number of parameters
    
- type of parameters
    
- order of parameters
    

Example:

```java
class MathUtils {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
}
```

Call:

```java
MathUtils m = new MathUtils();
m.add(3, 4);          // Calls add(int, int)
m.add(2.5, 4.7);      // Calls add(double, double)
m.add(1, 2, 3);       // Calls add(int, int, int)
```

---

# **2. Why Do We Use Method Overloading?**

1. **Readability**  
    Same task, different inputs → same method name makes sense.
    
2. **Flexibility**  
    Allows one method name to accept various parameter types.
    
3. **Code Maintainability**  
    Avoids naming multiple different methods for the same behavior.
    
4. **Better API Design**  
    Java built-in classes use overloading extensively  
    Example: `System.out.println()` has 10+ overloaded versions.
    

---

# **3. Rules of Method Overloading**

Two or more methods must differ by:

1. Number of parameters
    
2. Type of parameters
    
3. Order of parameters (rare case)
    

You **cannot overload a method by**:

- changing **return type** only
    
- changing **access modifiers only**
    
- adding/removing **throws** clause
    

Example (Invalid Overloading):

```java
int add(int a, int b)
double add(int a, int b)  // ERROR: same method signature
```

---

# **4. Types of Overloading**

## 1. Based on Number of Parameters

```java
void show(int a)
void show(int a, int b)
```

## 2. Based on Type of Parameters

```java
void show(int a)
void show(double a)
```

## 3. Based on Order of Parameters

```java
void display(int a, String b)
void display(String b, int a)
```

---

# **5. Method Overloading and Automatic Type Promotion**

Java may **promote smaller types to larger types** (widening conversion) if an exact match is not found.

Order of promotion:

byte → short → int → long → float → double

Example:

```java
void show(int x)
void show(double x)
```

Call:

```java
show(5);       // exact match → show(int)
show(5L);      // promotes to double → show(double)
```

If multiple promotions are possible, Java selects the _most specific_ method.

---

# **6. Overloading and Varargs**

Varargs create special situations.

Example:

```java
void test(int a)
void test(int... a)
```

Call:

```java
test(10);      // calls test(int), NOT test(int...)
test(1, 2, 3); // calls test(int...)
```

Varargs is the least preferred method unless necessary.

---

# **7. Constructor Overloading vs Method Overloading**

| Feature               | Constructor Overloading             | Method Overloading                           |
| --------------------- | ----------------------------------- | -------------------------------------------- |
| Purpose               | Initialize object in different ways | Perform same operation with different inputs |
| Can have return type? | No                                  | Yes                                          |
| Called when?          | Object creation                     | Method call                                  |
| Required?             | For flexible initialization         | For flexible operations                      |

---

# **8. Overloading vs Overriding (Very Important)**

|Feature|Overloading|Overriding|
|---|---|---|
|Polymorphism Type|Compile-time|Runtime|
|Same class or not?|Must be same class|Must be in different classes (inheritance)|
|Parameter list|Must differ|Must be same|
|Return type|Can differ|Must follow covariant return type rules|
|Access modifiers|No restriction|Cannot reduce visibility|
|Final/static methods|Can overload|Cannot override final; static methods hide|

---

# **9. Real-World Examples in Java Libraries**

### System.out.println()

Java has many overloaded versions:

```java
println(int x)
println(double x)
println(String s)
println(Object o)
println(char[] c)
...
```

### Math.max()

```java
max(int, int)
max(long, long)
max(float, float)
max(double, double)
```

### String.valueOf()

```java
valueOf(int)
valueOf(double)
valueOf(boolean)
valueOf(char[])
valueOf(Object)
```

---

# **10. Interview Questions on Overloading**

1. What is method overloading?
    
2. Can you overload main()?  
    Yes, JVM calls only `main(String[])`, others can be overloaded normally.
    
3. Can return type alone differentiate overloaded methods?  
    No.
    
4. What happens if two overloaded methods apply widening and boxing?  
    Widening > Boxing > Varargs (priority order).
    
5. Is method overloading runtime or compile-time polymorphism?  
    Compile-time.
    
6. Can static methods be overloaded?  
    Yes.
    
7. Does overloading use inheritance?  
    No (but parent methods can also overload child ones).
    

---

# **11. Summary**

- Overloading allows multiple methods with the same name but different parameter lists.
    
- It improves flexibility, readability, and API design.
    
- Overloading is resolved at **compile time**, not runtime.
    
- Overloading can involve number, type, or order of parameters.
    
- Java uses widening, boxing, and varargs rules to choose the best match.
    
- It is different from overriding (runtime polymorphism).
    

---