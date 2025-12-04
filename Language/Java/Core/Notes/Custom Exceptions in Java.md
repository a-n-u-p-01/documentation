# **Custom Exceptions in Java**

### **1. Overview**

Sometimes built-in exceptions are **not sufficient** to represent specific error conditions in an application. In such cases, Java allows you to **create your own exception classes**, called **custom exceptions**.

Custom exceptions can extend:

1. **Exception** – for **checked exceptions** (must be handled or declared).
    
2. **RuntimeException** – for **unchecked exceptions** (optional handling).
    

---

### **2. Creating a Custom Exception**

- **Step 1:** Create a class that **extends Exception or RuntimeException**.
    
- **Step 2:** Provide **constructors** to pass messages or other information.
    

**Example – Checked Exception:**

```java
// Custom checked exception
class AgeTooLowException extends Exception {
    public AgeTooLowException(String message) {
        super(message);
    }
}

public class CustomExceptionExample {
    public static void checkAge(int age) throws AgeTooLowException {
        if (age < 18) {
            throw new AgeTooLowException("Age must be at least 18");
        }
        System.out.println("Age accepted: " + age);
    }

    public static void main(String[] args) {
        try {
            checkAge(15); // will throw custom exception
        } catch (AgeTooLowException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }
}
```

**Example – Unchecked Exception:**

```java
// Custom unchecked exception
class NegativeValueException extends RuntimeException {
    public NegativeValueException(String message) {
        super(message);
    }
}

public class UncheckedCustomException {
    public static void checkValue(int value) {
        if (value < 0) {
            throw new NegativeValueException("Negative values not allowed");
        }
        System.out.println("Value is valid: " + value);
    }

    public static void main(String[] args) {
        checkValue(-5); // throws custom unchecked exception
    }
}
```

---

### **3. Key Points**

1. **Checked vs Unchecked:**
    
    - **Checked:** Extends `Exception` → must be handled or declared with `throws`.
        
    - **Unchecked:** Extends `RuntimeException` → optional handling.
        
2. **Constructors:**
    
    - Default constructor
        
    - Constructor with `String message`
        
    - Constructor with `String message` and `Throwable cause` (optional)
        
3. **Why use custom exceptions:**
    
    - Makes your code **more readable and meaningful**.
        
    - Helps **differentiate between types of errors**.
        
    - Easier **debugging and maintenance**.
        

---

### **4. Best Practices**

1. Name exceptions clearly (e.g., `InvalidAgeException`, `InsufficientBalanceException`).
    
2. Use **checked exceptions** for recoverable errors, **unchecked** for programming errors.
    
3. Include **informative messages**.
    
4. Avoid creating unnecessary custom exceptions; only when meaningful.
    

---
