## **1. What is an Exception?**

- An **exception** is an **event that disrupts normal flow** of a program.
    
- It occurs **during runtime**.
    
- Types of exceptions:
    
    1. **Checked Exceptions** – must be handled at compile-time (e.g., `IOException`, `SQLException`).
        
    2. **Unchecked Exceptions** – runtime exceptions, not required to be caught (e.g., `NullPointerException`, `ArrayIndexOutOfBoundsException`).
        

---

## **2. Try Block**

- Used to **wrap code that might throw an exception**.
    
- Must be **followed by at least one catch block or finally block**.
    

```java
try {
    int result = 10 / 0; // may throw ArithmeticException
}
```

- If no exception occurs: code executes normally.
    
- If an exception occurs: control moves to the **catch block**.
    

---

## **3. Catch Block**

- Used to **handle the exception**.
    
- Syntax:
    

```java
catch (ExceptionType e) {
    // exception handling code
}
```

**Example:**

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero: " + e.getMessage());
}
```

- Multiple catch blocks can be used for different exception types:
    

```java
try {
    // code that may throw multiple exceptions
} catch (NullPointerException e) {
    System.out.println("Null value found");
} catch (ArithmeticException e) {
    System.out.println("Divide by zero");
}
```

**Notes:**

- Catch blocks are **checked in order**.
    
- More specific exceptions should appear **before** general exceptions (`Exception`).
    

---

## **4. Finally Block**

- Used to **execute code regardless of exception occurrence**.
    
- Commonly used for **resource cleanup** (closing files, DB connections, sockets).
    

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("This block always executes");
}
```

**Notes:**

- Executes **even if try block exits via return**.
    
- Rare cases where `finally` may not execute:
    
    - JVM crashes
        
    - `System.exit()` is called
        

---

## **5. Try-with-Resources (Java 7+)**

- Automatically closes **resources** implementing `AutoCloseable`.
    
- Simplifies cleanup code.
    

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

- No need for explicit `finally` to close the resource.
    

---

## **6. Key Points**

1. `try` → wrap risky code.
    
2. `catch` → handle exception.
    
3. `finally` → cleanup code, always executed.
    
4. `try` block **must be followed** by at least one `catch` or `finally`.
    
5. Use **specific exception types** first in multiple catch blocks.
    
6. Avoid empty catch blocks; always log or handle exceptions properly.
    

---

## **7. Example – Full Flow**

```java
public class Main {
    public static void main(String[] args) {
        try {
            int a = 10, b = 0;
            int c = a / b;
            System.out.println("Result: " + c);
        } catch (ArithmeticException e) {
            System.out.println("Exception: Cannot divide by zero!");
        } finally {
            System.out.println("Execution completed.");
        }
    }
}
```

**Output:**

```
Exception: Cannot divide by zero!
Execution completed.
```

---

## **8. Summary Table**

|Block|Purpose|Executes when|
|---|---|---|
|try|Wrap code that may throw an exception|Only code inside try|
|catch|Handle the exception|Only if exception occurs|
|finally|Cleanup or mandatory execution code|Always (unless JVM exits/crash)|

---

Next topic would be **Exception Hierarchy and Types**.

Do you want me to create that detailed note next?