Varargs (**variable-length arguments**) allow a method to accept **zero or more arguments** of the same type. It improves API flexibility and avoids writing multiple overloaded methods.

---

#### **Basic Syntax**

Use `...` after the type:

```java
void print(int... nums) {
    for (int n : nums) System.out.println(n);
}
```

Calling the method:

```java
print();            // zero arguments
print(1);           // one argument
print(1, 2, 3, 4);  // many arguments
```

Internally, **varargs = array**.  
`int... nums` is treated as: `int[] nums`.

---

### **Varargs with Other Parameters**

Varargs can be combined with normal parameters:

```java
void log(String level, String... messages) {
    System.out.println("[" + level + "]");
    for(String m : messages) System.out.println(m);
}

log("INFO", "Started", "Processing", "Done");
```

---

### **Only One Varargs Parameter Allowed**

A method can have **only one varargs parameter**.

```java
void m(int... a, String... b); // ❌ illegal
```

Because the compiler can't determine how to split the arguments.

---

### **Varargs Must Be the Last Parameter**

This is mandatory:

```java
void test(String name, int... scores); // ✔ allowed

void test(int... scores, String name); // ❌ compile error
```

---

### **Varargs Are Arrays Internally**

You can pass an array explicitly:

```java
int[] data = {1, 2, 3};
print(data); // valid
```

This means varargs do not create a new array if one is already provided.

---

### **Overloading with Varargs (Tricky!)**

Varargs can confuse method resolution:

```java
void m(int a, int b) {}
void m(int... a) {}

m(10, 20);  
```

This calls the **non-varargs version** because it is more specific.

Another tricky example:

```java
void m(Object... o) {}
void m(String s, Object o) {}

m("hello");  
```

This calls the second method because it's more specific.

---

### **Performance Consideration**

Each varargs call **allocates an array**, even for one argument.

This can affect performance in low-latency or high-frequency code (e.g., logging frameworks).

Java uses techniques like:

```java
void debug(String msg, Object... args)
```

and checks:

```java
if (!logEnabled) return;
```

to avoid unnecessary array creation.

---

## **Varargs with Generics (Warning)**

Varargs with generics create **"heap pollution"** risks:

```java
void add(List<String>... lists) { } // warning
```

Compiler gives:

```
Warning: [varargs] Possible heap pollution from parameterized vararg type
```

To mark it safe, you can use:

```java
@SafeVarargs
```

But only on **static**, **final**, or **private** methods.

---

## **Empty Varargs Behavior**

Calling method with empty varargs:

```java
print(); 
```

Inside the method:

- `nums` is **an empty array** (`length = 0`)
    
- Not `null`.
    

This ensures you don’t get `NullPointerException`.

---

# **Common Pitfalls**

### **Varargs + Autoboxing Ambiguity**

```java
void m(Integer... a) {}
void m(int... a) {}

m(10); // ❌ ambiguous → compiler error
```

Because `10` can autobox to `Integer` or fit in primitive `int`.

---

### **Using Varargs in Overloaded Methods Requires Care**

Bad API:

```java
void print(String s) {}
void print(String... s) {}
```

Calling:

```java
print("hello"); // ambiguous? → calls single-arg method
```

Varargs method rarely gets called for single argument.

---

### **Varargs May Hide Logical Bugs**

```java
void read(int... ids) {}

read();  // allowed, but maybe the caller expected a compile error
```

Avoid varargs when zero arguments are invalid—instead validate inside method.

---

# **Interview Questions**

- What is varargs and how does it work internally?  
    → It allows passing variable number of arguments; internally treated as an array.
    
- Why can there be only one varargs parameter and why must it be last?  
    → To avoid ambiguity in argument mapping.
    
- Are varargs slower?  
    → Yes, because each call creates an array.
    
- Difference between calling method with `m(1,2,3)` and `m(new int[]{1,2,3})`?  
    → Both are identical; the latter explicitly creates the array.
    
- What is heap pollution in varargs with generics?  
    → Storing wrong typed objects in a generic array due to type erasure.
    
- Why does Java give a warning for `List<String>...`?  
    → Generic arrays are unsafe; type safety cannot be guaranteed.
    
- When does varargs cause method resolution ambiguity?  
    → With overloaded methods that use autoboxing, primitives, or generics.
    
- Can you annotate varargs? What is `@SafeVarargs`?  
    → Used to suppress heap pollution warnings for certain safe methods.
    

---

If you want, I can also create a **Varargs Cheatsheet** or compare **Varargs vs method overloading** for interviews.