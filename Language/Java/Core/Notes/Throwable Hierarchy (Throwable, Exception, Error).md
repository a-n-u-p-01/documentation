# **Throwable Hierarchy in Java**

### **1. Overview**

In Java, **all errors and exceptions are represented by objects** derived from the `Throwable` class. This hierarchy helps the Java runtime and programmers **categorize and handle different types of problems** that occur during program execution.

- **Throwable** is the **root class** for all exceptions and errors in Java.
    
- It has **two main subclasses**:
    
    1. **Exception**
        
    2. **Error**
        

---

### **2. Throwable Class**

- **Definition:** `java.lang.Throwable`
    
- Represents **any problem that can occur during the execution of a program**.
    
- **Key Methods:**
    
    - `getMessage()` – Returns a description of the problem.
        
    - `printStackTrace()` – Prints the sequence of method calls leading to the exception.
        
    - `getCause()` – Returns the cause of the throwable, if any.
        

---

### **3. Exception Class**

- **Represents recoverable conditions** that a program **can handle**.
    
- Subclass of `Throwable`.
    
- Divided into:
    
    #### **3.1 Checked Exceptions**
    
    - Must be **handled at compile-time** using `try-catch` or declared with `throws`.
        
    - Examples:
        
        - `IOException` – input/output errors
            
        - `SQLException` – database access errors
            
        - `FileNotFoundException` – file not found
            
    - **Key point:** Compiler forces handling.
        
    
    #### **3.2 Unchecked Exceptions**
    
    - Known as **Runtime Exceptions**.
        
    - Do **not need to be declared or caught**.
        
    - Usually indicate **programming errors**.
        
    - Examples:
        
        - `NullPointerException`
            
        - `ArithmeticException`
            
        - `ArrayIndexOutOfBoundsException`
            
    - **Key point:** Checked only at runtime, not at compile-time.
        

---

### **4. Error Class**

- Represents **serious, unrecoverable problems** in the JVM.
    
- Subclass of `Throwable`.
    
- Examples:
    
    - `OutOfMemoryError` – JVM runs out of memory
        
    - `StackOverflowError` – usually due to infinite recursion
        
    - `VirtualMachineError` – JVM internal problems
        
- **Key point:** Errors are usually **not caught or handled** by applications.
    

---

### **5. Hierarchy Diagram (Simplified)**

```
Throwable
 ├── Error
 │    ├── OutOfMemoryError
 │    ├── StackOverflowError
 │    └── VirtualMachineError
 └── Exception
      ├── Checked Exceptions
      │    ├── IOException
      │    └── SQLException
      └── Unchecked Exceptions (RuntimeException)
           ├── NullPointerException
           ├── ArithmeticException
           └── ArrayIndexOutOfBoundsException
```

---

### **6. Key Points**

1. **Throwable** is the **root** for all problems in Java.
    
2. **Exception** – recoverable, can be handled with `try-catch`.
    
3. **Error** – serious, usually **cannot be handled**.
    
4. **Checked Exceptions** – compile-time checked, must be handled.
    
5. **Unchecked Exceptions** – runtime errors, optional handling.
    
6. Always catch **specific exceptions first**, then general ones.
    

---