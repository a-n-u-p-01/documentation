### **What is an Exception?**

- An **exception** is an **event that occurs during the execution of a program** that disrupts the normal flow of instructions.
    
- Exceptions are **unwanted or unexpected situations** that a program may encounter, such as:
    
    - Division by zero
        
    - File not found
        
    - Invalid user input
        
    - Array index out of bounds
        

### **Why Exception Handling is Needed**

- Prevents **program termination** due to runtime errors.
    
- Helps in **maintaining normal flow** of the application.
    
- Provides a mechanism to **handle errors gracefully** and provide meaningful messages to the user.
    

### **Types of Errors in Java**

1. **Compile-time Errors**
    
    - Errors detected by the compiler.
        
    - Examples: Syntax errors, type mismatch.
        
2. **Runtime Errors**
    
    - Errors detected during program execution.
        
    - Examples: NullPointerException, ArrayIndexOutOfBoundsException.
        

### **Exception Handling in Java**

- Java provides a **robust mechanism** to handle exceptions using **try-catch-finally**, **throw**, **throws**, and **custom exceptions**.
    
- All exceptions in Java are subclasses of the **`Throwable`** class.
    

### **Benefits of Exception Handling**

- Improves **program reliability**.
    
- Makes programs **robust** and easier to **debug**.
    
- Separates **error-handling code** from **regular code**.
    
- Allows **propagation of errors** to higher levels for centralized handling.
    

---