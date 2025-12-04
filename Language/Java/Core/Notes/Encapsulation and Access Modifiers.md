# **1. What is Encapsulation?**

**Encapsulation** is one of the **four pillars of OOP**.  
It is the **technique of wrapping data (variables) and code (methods) together into a single unit** (class) and **restricting direct access to some of the object's components**.

**Key Points:**

- Protects **internal state** of objects from unintended modifications.
    
- Provides **controlled access** through **getter and setter methods**.
    
- Improves **maintainability** and **modularity** of code.
    
- Can **hide implementation details** from the user (information hiding).
    

**Example:**

```java
class Student {
    // private variables – cannot be accessed directly
    private String name;
    private int age;

    // Getter method
    public String getName() { return name; }

    // Setter method
    public void setName(String name) { this.name = name; }

    public int getAge() { return age; }

    public void setAge(int age) {
        if(age > 0) this.age = age; // validation
    }
}
```

**Usage:**

```java
Student s = new Student();
s.setName("John");
System.out.println(s.getName());
```

---

# **2. Benefits of Encapsulation**

1. **Control Access:**  
    Private data cannot be accessed directly, reducing unintended changes.
    
2. **Data Validation:**  
    Setters can validate data before updating.
    
3. **Flexibility:**  
    Internal implementation can change without affecting outside code.
    
4. **Improved Maintainability:**  
    Easier to debug and maintain encapsulated code.
    
5. **Security:**  
    Sensitive data (like passwords, bank details) can be protected.
    

---

# **3. Access Modifiers in Java**

Access modifiers **define the visibility** of variables, methods, and classes.

|Modifier|Class|Package|Subclass|World (Anywhere)|
|---|---|---|---|---|
|`private`|Yes|No|No|No|
|`default` (no keyword)|Yes|Yes|No|No|
|`protected`|Yes|Yes|Yes|No|
|`public`|Yes|Yes|Yes|Yes|

---

# **4. Explanation of Access Modifiers**

### **4.1 Private**

- Accessible **only within the class**.
    
- Commonly used to achieve **encapsulation**.
    

```java
private int age;
```

### **4.2 Default (Package-Private)**

- No keyword needed.
    
- Accessible **within the same package** only.
    

```java
int age; // default access
```

### **4.3 Protected**

- Accessible **within the same package** and **subclasses**.
    
- Useful for **inheritance**.
    

```java
protected int age;
```

### **4.4 Public**

- Accessible **from anywhere**.
    
- Used when data or methods need to be shared.
    

```java
public int age;
```

---

# **5. Applying Encapsulation with Access Modifiers**

- **Private fields + public getters/setters** is the standard practice.
    
- Can combine **protected** for inheritance and controlled access.
    
- Avoid using public fields unless truly necessary.
    

**Example:**

```java
class BankAccount {
    private double balance;

    public double getBalance() { return balance; }

    public void deposit(double amount) {
        if(amount > 0) balance += amount;
    }

    public void withdraw(double amount) {
        if(amount > 0 && amount <= balance) balance -= amount;
    }
}
```

---

# **6. Real-world Analogy**

- Encapsulation is like a **capsule or pill**:
    
    - Inside = private data
        
    - Outside = public interface (getter/setter)
        
    - You can **use the functionality** without seeing the inner details.
        

---

# **7. Key Points / Best Practices**

1. **Always declare class variables as private.**
    
2. **Provide getters and setters** for controlled access.
    
3. **Use validation inside setters** to prevent invalid data.
    
4. **Limit public access** to only what is necessary.
    
5. **Protected access** can be used for inheritance-friendly design.
    
6. Helps maintain **data integrity** and **flexibility**.
    

---

# **8. Example: Complete Encapsulation**

```java
class Employee {
    private String name;
    private int salary;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public int getSalary() { return salary; }
    public void setSalary(int salary) {
        if(salary > 0) this.salary = salary;
        else System.out.println("Invalid salary");
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee();
        emp.setName("Alice");
        emp.setSalary(5000);

        System.out.println(emp.getName());
        System.out.println(emp.getSalary());
    }
}
```

---

# **9. Summary**

- **Encapsulation** = Data + Methods in a class + Controlled access
    
- **Private variables** + **Public getters/setters** = standard practice
    
- **Access modifiers** control visibility:
    
    - private → class only
        
    - default → package
        
    - protected → package + subclass
        
    - public → everywhere
        
- Provides **security, maintainability, and flexibility** to your code
    

---