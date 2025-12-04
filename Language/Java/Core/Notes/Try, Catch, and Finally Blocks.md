## **Try, Catch, and Finally Blocks**

### **1. Overview**

- In Java, **`try-catch-finally`** blocks are used to **handle exceptions**.
    
- **Syntax:**
    

```java
try {
    // Code that may throw an exception
} catch (ExceptionType1 e1) {
    // Handle ExceptionType1
} catch (ExceptionType2 e2) {
    // Handle ExceptionType2
} finally {
    // Code that always executes
}
```

---

### **2. Try Block**

- Contains **code that might throw an exception**.
    
- Must be **followed by at least one catch block or a finally block**.
    
- If no exception occurs, catch blocks are skipped, but finally executes (if present).
    

---

### **3. Catch Block**

- Catches and **handles the exception** thrown in the try block.
    
- Can have **multiple catch blocks** to handle different exception types.
    
- Catch blocks are evaluated **top to bottom**.
    
- **Example:**
    

```java
try {
    int result = 10 / 0; // ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero.");
} catch (Exception e) {
    System.out.println("Some other exception occurred.");
}
```

---

### **4. Finally Block**

- **Optional**, executes **regardless of exception occurrence**.
    
- Useful for **resource cleanup**, like closing files or database connections.
    
- **Example:**
    

```java
try {
    int[] arr = new int[5];
    arr[10] = 50; // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Invalid index access.");
} finally {
    System.out.println("This block always executes.");
}
```

---

### **5. Key Points**

- A try block **cannot exist alone**.
    
- Multiple catch blocks **must handle exceptions in order of inheritance** (child first, parent later).
    
- Finally block is executed **even if there is a return statement** in try or catch.
    

---

I can next create the **“Checked vs Unchecked Exceptions”** note in the same detailed format. Do you want me to continue?