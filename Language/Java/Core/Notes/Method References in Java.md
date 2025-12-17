Method References are a **shorter and cleaner way** to write **lambda expressions** when the lambda **only calls an existing method**.

They improve **readability**, **maintainability**, and are heavily used in **Streams and Functional Programming**.

---

## **1. What is a Method Reference?**

A **method reference** is a shorthand notation of a lambda expression that calls a method.

### Lambda form

```java
(x) -> System.out.println(x)
```

### Method reference form

```java
System.out::println
```

Both do the **same work**.

---

## **2. Why Method References Exist**

- Reduce boilerplate code
    
- Make code easier to read
    
- Avoid repeating parameter names
    
- Works naturally with functional interfaces
    

Rule:

> **If a lambda only calls a method, use method reference**

---

## **3. Syntax**

```java
ClassName::methodName
object::methodName
ClassName::new
```

The `::` operator is called the **method reference operator**.

---

## **4. Types of Method References**

There are **4 types**.

---

### **1. Reference to a Static Method**

Used when the lambda calls a **static method**.

#### Example

```java
class MathUtils {
    static int square(int x) {
        return x * x;
    }
}
```

Lambda:

```java
Function<Integer, Integer> f = x -> MathUtils.square(x);
```

Method reference:

```java
Function<Integer, Integer> f = MathUtils::square;
```

---

### **2. Reference to an Instance Method (of a particular object)**

Used when a **specific object’s method** is called.

#### Example

```java
class Printer {
    void print(String s) {
        System.out.println(s);
    }
}
```

Lambda:

```java
Printer p = new Printer();
Consumer<String> c = s -> p.print(s);
```

Method reference:

```java
Consumer<String> c = p::print;
```

---

### **3. Reference to an Instance Method of an Arbitrary Object**

Used when the method is called on **each object** in a collection.

Very common with **Streams**.

#### Example

Lambda:

```java
List<String> list = List.of("java", "spring");
list.forEach(s -> s.toUpperCase());
```

Method reference:

```java
list.forEach(String::toUpperCase);
```

Another example:

```java
list.forEach(System.out::println);
```

---

### **4. Reference to a Constructor**

Used to create objects.

#### Example

Lambda:

```java
Supplier<ArrayList<String>> s = () -> new ArrayList<>();
```

Method reference:

```java
Supplier<ArrayList<String>> s = ArrayList::new;
```

---

## **5. Method Reference vs Lambda**

|Lambda|Method Reference|
|---|---|
|More flexible|More concise|
|Can contain logic|Only method call|
|Verbose|Clean|
|`(x) -> foo(x)`|`Class::foo`|

Use **method reference only when logic is simple**.

---

## **6. Method Reference with Streams (Very Important)**

### Example 1 – forEach

```java
list.forEach(System.out::println);
```

### Example 2 – map

```java
list.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

### Example 3 – sort

```java
Collections.sort(list, String::compareTo);
```

---

## **7. How Java Decides Which Method to Call**

Java uses **context** from the **functional interface**.

Example:

```java
Consumer<String> c = System.out::println;
```

Java sees:

- `Consumer.accept(String)`
    
- Finds `println(String)`
    

So it binds automatically.

---

## **8. Common Mistakes**

❌ Trying to use method reference when logic exists

```java
x -> x + 1   // cannot convert to method reference
```

❌ Method signature mismatch

```java
Function<String, Integer> f = String::length; // OK
Function<String, String> f = String::length;  // ERROR
```

---

## **9. When to Use Method References**

Use when:

- Lambda body has **one method call**
    
- No extra logic
    
- Improves readability
    

Avoid when:

- Complex logic
    
- Multiple statements
    
- Condition checks
    

---

## **10. Interview Questions + Answers**

### **1. What is a method reference?**

A shorthand syntax for calling a method using a lambda expression.

---

### **2. What operator is used in method reference?**

The `::` operator.

---

### **3. How many types of method references are there?**

Four:

- Static method
    
- Instance method of object
    
- Instance method of class
    
- Constructor reference
    

---

### **4. Is method reference faster than lambda?**

No significant performance difference. It’s mainly for readability.

---

### **5. Can every lambda be replaced with method reference?**

No. Only lambdas that directly call a method.

---

## **11. Simple Mental Model**

Think of method reference as:

> “**Pass the method itself, not the result**”

---

## **12. Summary**

- Method references are **clean lambda shortcuts**
    
- Use `::` operator
    
- Work with functional interfaces
    
- Widely used in Streams
    
- Improve readability, not logic
    

---
