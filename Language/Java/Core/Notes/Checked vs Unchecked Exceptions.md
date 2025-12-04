# **Checked vs Unchecked Exceptions in Java**

### **1. Overview**

Java exceptions are divided into **two main categories** based on when they are checked:

1. **Checked Exceptions** – checked at **compile-time**
    
2. **Unchecked Exceptions** – checked at **runtime**
    

Understanding the difference is essential for **proper exception handling**.

---

### **2. Checked Exceptions**

- **Definition:** Exceptions that the **compiler forces the programmer to handle** either by `try-catch` or by declaring with `throws`.
    
- **Characteristics:**
    
    - Subclass of `Exception` (but **not RuntimeException**).
        
    - Can be **predicted** during program execution.
        
    - Failure to handle results in **compile-time error**.
        
- **Common Examples:**
    
    - `IOException` – file I/O problems
        
    - `FileNotFoundException` – file is missing
        
    - `SQLException` – database errors
        
    - `ClassNotFoundException` – missing class
        
- **Example:**
    

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class CheckedExample {
    public static void main(String[] args) {
        try {
            File file = new File("test.txt");
            FileReader fr = new FileReader(file); // IOException might occur
        } catch (IOException e) {
            System.out.println("File not found or cannot be read.");
        }
    }
}
```

---

### **3. Unchecked Exceptions**

- **Definition:** Exceptions that are **not checked by the compiler**, also called **Runtime Exceptions**.
    
- **Characteristics:**
    
    - Subclass of `RuntimeException`.
        
    - Usually caused by **programming errors**.
        
    - Can be **handled optionally**, but not mandatory.
        
    - Detected **at runtime**.
        
- **Common Examples:**
    
    - `ArithmeticException` – divide by zero
        
    - `NullPointerException` – dereferencing null
        
    - `ArrayIndexOutOfBoundsException` – invalid array access
        
    - `IllegalArgumentException` – wrong method argument
        
- **Example:**
    

```java
public class UncheckedExample {
    public static void main(String[] args) {
        int a = 10, b = 0;
        int result = a / b; // ArithmeticException at runtime
        System.out.println(result);
    }
}
```

---

### **4. Key Differences**

|Feature|Checked Exception|Unchecked Exception|
|---|---|---|
|Compilation|Checked at compile-time|Not checked at compile-time|
|Subclass|Exception (excluding RuntimeException)|RuntimeException|
|Handling|Must handle or declare `throws`|Optional to handle|
|Cause|External issues (I/O, DB)|Programming errors (logic mistakes)|
|Examples|IOException, SQLException|NullPointerException, ArithmeticException|

---

### **5. Best Practices**

1. Handle **checked exceptions** properly to ensure program stability.
    
2. Avoid catching **unchecked exceptions** unless necessary; fix the root cause instead.
    
3. Use **custom exceptions** for application-specific errors.
    
4. Prefer **specific exception handling** over generic `Exception`.
    

---

