# **Throw and Throws Keywords in Java**

### **1. Overview**

Java provides **two keywords** for exception handling related to propagating or signaling errors:

1. **throw** – used to **explicitly throw a single exception** from a method or block.
    
2. **throws** – used in a **method declaration** to indicate **one or more exceptions a method might throw**.
    

Although similar in spelling, their usage and purpose are **different**.

---

### **2. throw Keyword**

- **Purpose:** Used to **throw an exception explicitly** from a method or block.
    
- **Syntax:**
    

```java
throw new ExceptionType("message");
```

- **Key Points:**
    
    - Can throw **checked or unchecked exceptions**.
        
    - Can be used **inside method or block**.
        
    - Stops the normal flow of execution.
        
- **Example:**
    

```java
public class ThrowExample {
    public static void checkAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be 18 or older");
        } else {
            System.out.println("Age accepted: " + age);
        }
    }

    public static void main(String[] args) {
        checkAge(15); // Throws IllegalArgumentException
    }
}
```

---

### **3. throws Keyword**

- **Purpose:** Declares **exceptions a method can throw** to the calling method.
    
- **Syntax:**
    

```java
returnType methodName() throws ExceptionType1, ExceptionType2 {
    // method code
}
```

- **Key Points:**
    
    - Used in **method declaration**, not inside method body.
        
    - Can declare **multiple exceptions** separated by commas.
        
    - Only informs the **caller** about potential exceptions; doesn’t throw them by itself.
        
- **Example:**
    

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class ThrowsExample {
    public static void readFile() throws IOException {
        File file = new File("test.txt");
        FileReader fr = new FileReader(file); // may throw IOException
    }

    public static void main(String[] args) {
        try {
            readFile(); // must handle IOException
        } catch (IOException e) {
            System.out.println("File not found or cannot be read");
        }
    }
}
```

---

### **4. Differences between throw and throws**

|Feature|throw|throws|
|---|---|---|
|Usage|Inside method/block to **throw an exception**|In method declaration to **declare exceptions**|
|Number of Exceptions|One exception at a time|Can declare multiple exceptions separated by commas|
|Normal Flow|Stops execution immediately|Does not stop execution; only informs the caller|
|Example|`throw new IOException();`|`void read() throws IOException, SQLException`|

---

### **5. Best Practices**

1. Use **throw** for **specific error conditions** inside methods.
    
2. Use **throws** to inform **caller methods** of potential exceptions.
    
3. Prefer **specific exceptions** over generic `Exception` to improve readability.
    
4. Combine `throw` and `throws` effectively: `throw` to generate, `throws` to propagate.
    

---