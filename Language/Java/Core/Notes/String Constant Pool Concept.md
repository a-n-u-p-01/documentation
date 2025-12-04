The **String Constant Pool** (also called **String Intern Pool**) is a special memory area inside the **Heap** where **string literals** are stored. Its main purpose is **memory optimization and reuse of immutable strings**.

---

#### **Why String Pool Exists**

- Strings in Java are **immutable**.
    
- Creating multiple identical strings wastes memory.
    
- SCP ensures **one copy of a string literal** exists in memory.
    

```java
String s1 = "Hello";
String s2 = "Hello";

System.out.println(s1 == s2); // true
```

- `s1` and `s2` point to the **same object in the SCP**.
    
- This is **different from `new String()`**, which always creates a new object.
    

```java
String s3 = new String("Hello");
System.out.println(s1 == s3); // false
```

- `s3` points to a **heap object**, not the SCP.
    

---

## **How String Pool Works**

1. **Compile-Time**:
    
    - String literals in code are added to the SCP at compile time.
        
2. **Runtime**:
    
    - When a string literal is used, JVM checks SCP:
        
        - If exists → returns reference.
            
        - If not → adds it and returns reference.
            
3. **`intern()` method**:
    
    - Converts a heap string to **pool reference**.
        

```java
String s4 = new String("Java");
String s5 = s4.intern();
System.out.println(s4 == s5); // false
System.out.println(s5 == "Java"); // true
```

- `intern()` ensures the string is in **SCP** and returns its reference.
    

---

## **String Immutability and Pool**

- Strings are **immutable**, so sharing them in SCP is safe.
    
- Modifying a string always creates a **new object**.
    

```java
String s = "Hello";
s = s + " World"; // new string object created, original in pool remains
```

- Immutability + SCP = memory efficiency + thread safety.
    

---

## **SCP vs Heap Strings**

|Feature|String Literal (SCP)|`new String()` (Heap)|
|---|---|---|
|Memory location|String Constant Pool|Heap|
|Reuse|Yes|No|
|Object creation|Once|Every time `new` is used|
|`==` comparison|true if same literal|false unless `intern()` used|
|Mutability|Immutable|Immutable (same)|

---

## **SCP in Java 7+**

- Before Java 7: SCP was in **PermGen**, limited memory.
    
- Java 7+: SCP moved to **Heap**, allowing **dynamic resizing**.
    
- Better for **large number of string literals**.
    

---

## **String Concatenation and SCP**

- **Compile-time concatenation** of literals → stored in SCP:
    

```java
String s1 = "Hello" + "World"; // resolved at compile-time
String s2 = "HelloWorld";
System.out.println(s1 == s2); // true
```

- **Runtime concatenation** with variables → stored in Heap:
    

```java
String a = "Hello";
String b = "World";
String s3 = a + b;
System.out.println(s3 == "HelloWorld"); // false
```

- Use **`intern()`** to move concatenated string to SCP:
    

```java
String s4 = (a + b).intern();
System.out.println(s4 == "HelloWorld"); // true
```

---

## **Best Practices**

- Prefer **string literals** when possible.
    
- Use `StringBuilder` for **runtime concatenation in loops**.
    
- Use `intern()` for **frequently reused dynamic strings**, but avoid overusing in **high-memory apps**.
    
- Be aware of **immutability** to prevent accidental memory overhead.
    

---

## **Common Pitfalls**

- Using `new String("literal")` unnecessarily creates **extra objects**.
    
- Runtime concatenation of strings in loops can lead to **memory overhead**.
    
- Overusing `intern()` may lead to **memory pressure** in very large applications.
    

---
