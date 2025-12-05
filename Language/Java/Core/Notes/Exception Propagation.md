### **What is Exception Propagation?**

Exception Propagation refers to the process where an exception **moves up the call stack** until it is handled by a `catch` block or terminates the program.

If a method does not handle an exception, it is **automatically passed** to the method that called it.  
This continues until:

1. A matching `catch` block is found, OR
    
2. It reaches the JVM → **program crashes**
    

---

## **How Propagation Works**

When an exception occurs:

1. The method where the exception happened
    
2. Transfers control to the inner `catch` block (if any)
    
3. If no matching `catch`, the exception is thrown to the **caller method**
    
4. This continues through all calling methods
    
5. If no method handles it → JVM handles it → abnormal termination
    

---

## **Propagation in Unchecked Exceptions**

Unchecked exceptions (like `ArithmeticException`, `NullPointerException`)  
**always propagate automatically** because they do NOT require `throws`.

```java
void m1() {
    int x = 10 / 0;   // ArithmeticException
}

void m2() {
    m1();             // exception propagated here
}

void m3() {
    m2();             // propagated again
}

public static void main(String[] args) {
    new Test().m3();  // JVM gets it → program crashes
}
```

Output:  
`Exception in thread "main" java.lang.ArithmeticException: / by zero`

---

## **Propagation in Checked Exceptions**

Checked exceptions **do not propagate automatically**  
unless:

- The method explicitly declares `throws`, OR
    
- The method catches the exception.
    

Example:

```java
void m1() throws IOException {
    throw new IOException("error");
}

void m2() throws IOException {
    m1();  // must declare throws or handle
}

void m3() {
    try {
        m2();
    } catch (IOException e) {
        System.out.println("Handled in m3");
    }
}
```

Output:  
`Handled in m3`

---

## **Key Rules of Exception Propagation**

1. **Unchecked exceptions** → propagate automatically
    
2. **Checked exceptions** → require `throws` keyword
    
3. If not handled by any method → JVM handles → program stops
    
4. Propagation always goes **reverse of calling order** (stack unwinding)
    
5. `finally` block executes during propagation (unless JVM shuts down)
    

---

## **When Propagation is Useful?**

- To let higher-level methods decide how to handle exceptions
    
- To avoid handling exceptions deep inside business logic
    
- To keep code cleaner by centralizing exception handling
    

---

## **When Propagation Should Not Be Used?**

- When the method itself should handle the issue
    
- When you need precise recovery control
    
- When exception adds noise to method signatures (`throws` everywhere)
    

---

## **Simple Diagram**

```
main() → m3() → m2() → m1()
                       ↑
         Exception originates here
```

Exception travels **upward** until someone catches it.

---

## **Common Misconceptions**

❌ Propagation means exception is ignored  
→ No. It only moves upward to find a handler.

❌ Only checked exceptions propagate  
→ No. Unchecked exceptions propagate by default.

---

## **Interview Questions (with answers)**

### **1. What is exception propagation?**

It is the process where an exception is forwarded up the call stack until it is handled by a matching `catch` block or the JVM.

---

### **2. Do unchecked exceptions propagate automatically?**

Yes. Unchecked exceptions (RuntimeExceptions) propagate without needing `throws`.

---

### **3. Why don’t checked exceptions propagate automatically?**

Because Java requires checked exceptions to be either handled with `try-catch` or declared using `throws`.

---

### **4. Does a finally block run during exception propagation?**

Yes. `finally` always executes unless JVM exits (System.exit).

---

### **5. Can we stop exception propagation?**

Yes, by catching the exception in any method.

---
>next : [Common Exception Classes in Java](Common%20Exception%20Classes%20in%20Java.md)