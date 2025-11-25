### **Variables and Scope in Java**

Variables in Java are **named memory locations** used to store data during program execution. Understanding variable types and their scopes is crucial for memory management, performance, and avoiding bugs.

---

#### ✅ **1. Types of Variables in Java**

Java variables are mainly categorized into **three types**:

#### **1️⃣ Local Variables**

- Declared **inside a method**, constructor, or block.
    
- Created when method is **invoked**, destroyed when method **exits**.
    
- **No default values** (must be initialized before use).
    
- Stored on the **stack**.
    

### Example:

```java
void show() {
    int x = 10; // local variable
    System.out.println(x);
}
```

---

#### **2️⃣ Instance Variables (Non-static Fields)**

- Declared **inside a class but outside any method**.
    
- Each **object has its own copy**.
    
- Have **default values**.
    
- Stored on the **heap**.
    

### Example:

```java
class Person {
    String name;   // instance variable
    int age;       // instance variable
}
```

---

#### **3️⃣ Static Variables (Class Variables)**

- Declared with the **static** keyword inside a class.
    
- **Shared** among all objects.
    
- Loaded when the class is **loaded into memory**.
    
- Have **default values**.
    
- Stored in the **method area** (JVM metadata).
    

### Example:

```java
class Counter {
    static int count = 0; // static variable
}
```

---

# 📌 **Comparison Table**

|Feature|Local Variable|Instance Variable|Static Variable|
|---|---|---|---|
|Declared|in method/block|in class|in class|
|Default value|❌ No|✔ Yes|✔ Yes|
|Lifetime|during method|object lifetime|class lifetime|
|Memory|Stack|Heap|Method area|
|Shared?|No|No|Yes|

---

# ✅ **2. Variable Scope in Java**

Scope defines **where a variable can be accessed**.

## **1️⃣ Block Scope**

Variables inside `{}` blocks are only accessible within that block.

```java
if (true) {
    int x = 5;
}
// x is NOT accessible here
```

---

## **2️⃣ Method Scope**

Local variables are accessible only inside the method.

```java
void test() {
    int a = 10;
}
// a is not accessible outside test()
```

---

## **3️⃣ Class Scope**

Applies to **instance** and **static** variables — accessible throughout the class (depending on access modifiers).

```java
class Demo {
    int a = 10;       // instance
    static int b = 20; // static
}
```

---

## **4️⃣ Loop & Enhanced Loop Scope**

The loop variable only lives within the loop.

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
// i is not accessible here
```

---

## **5️⃣ Scope of Variables in try-catch-finally**

Variables declared inside try/catch blocks exist only in that block.

```java
try {
    int x = 10;
} catch (Exception e) {
    int y = 20;
}
// x and y are NOT accessible here
```

---

# 🚫 **Shadowing**

When a local variable has the same name as an instance variable, the instance variable is **shadowed**.

```java
class Demo {
    int x = 10;

    void show() {
        int x = 20; // shadows instance variable
        System.out.println(x);       // 20
        System.out.println(this.x);  // 10
    }
}
```

---

# 🚀 **Best Practices**

- Use **meaningful names** (`totalMarks`, not `tm`).
    
- Keep **variable scope as small as possible** → prevents errors.
    
- Avoid too many **static variables**; use them only when shared state is required.
    
- Always initialize **local variables** before use.
    

---

# 🎯 **Quick Interview Questions**

1. **Do local variables have default values?**  
    → _No._
    
2. **Where are instance variables stored?**  
    → _Heap._
    
3. **What is variable shadowing?**  
    → _Local variable hides instance variable._
    
4. **Can static methods access instance variables?**  
    → _No, not directly._
    
5. **Can you declare a local variable as static?**  
    → _No. Only class-level variables can be static._
    

---

If you want, I can also generate:  
✨ **MCQs**  
✨ **Coding exercises**  
✨ **Flashcards**  
✨ **Mind map for this topic**

Just tell me!