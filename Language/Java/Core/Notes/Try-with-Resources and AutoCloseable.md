# **Try with Resources and AutoCloseable in Java**

### **1. Overview**

The **try-with-resources** statement was introduced in **Java 7** to simplify resource management and avoid **resource leaks**.  
Resources like **files, database connections, sockets** need to be **closed explicitly** after use. Using try-with-resources, Java **automatically closes** them.

---

### **2. AutoCloseable Interface**

- Any resource used in try-with-resources must implement the **`AutoCloseable`** interface.
    
- The interface has a single method:
    

```java
void close() throws Exception;
```

- All classes that implement **Closeable** (like FileInputStream, BufferedReader, Scanner) also implement AutoCloseable.
    

---

### **3. Syntax of Try-with-Resources**

```java
try (ResourceType resource = new ResourceType()) {
    // Use the resource
} catch (ExceptionType e) {
    // Handle exception
} finally {
    // Optional block
}
```

**Key Points:**

1. The resource declared inside `()` is **automatically closed** at the end of the try block.
    
2. Can declare **multiple resources** separated by semicolon `;`.
    
3. **No need for explicit `finally` block** for closing resources.
    

---

### **4. Example**

**Reading a file using try-with-resources:**

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TryWithResourcesExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

- No need to call `br.close()` explicitly; it’s closed automatically.
    

---

### **5. Multiple Resources Example**

```java
try (FileReader fr = new FileReader("file1.txt");
     BufferedReader br = new BufferedReader(fr)) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### **6. Advantages**

1. **Automatic resource management** → avoids memory/resource leaks.
    
2. **Cleaner code** → no explicit finally block needed.
    
3. Works with **any resource implementing AutoCloseable**.
    
4. Supports **suppressed exceptions**, i.e., if an exception occurs in both try block and close() method, the one from close() is suppressed but can be accessed.
    

---

### **7. Suppressed Exceptions Example**

```java
class MyResource implements AutoCloseable {
    public void close() throws Exception {
        throw new Exception("Exception in close()");
    }
}

public class SuppressedExample {
    public static void main(String[] args) {
        try (MyResource r = new MyResource()) {
            throw new Exception("Exception in try block");
        } catch (Exception e) {
            System.out.println("Caught: " + e.getMessage());
            for (Throwable t : e.getSuppressed()) {
                System.out.println("Suppressed: " + t.getMessage());
            }
        }
    }
}
```

**Output:**

```
Caught: Exception in try block
Suppressed: Exception in close()
```

---

### **8. Best Practices**

1. Always use **try-with-resources** for files, streams, database connections.
    
2. Declare resources **inside the try parentheses**.
    
3. Avoid mixing manual close with try-with-resources.
    
4. For custom resources, implement **AutoCloseable** for automatic management.
    

---