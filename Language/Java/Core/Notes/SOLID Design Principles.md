**SOLID** is a set of **5 object-oriented design principles** that help you write:

- Maintainable code
    
- Scalable systems
    
- Testable classes
    
- Flexible architectures
    

SOLID is not about syntax — it is about **design thinking**.

---

## S – Single Responsibility Principle (SRP)

### What It Means

> A class should have **only one reason to change**.

In simple words:

- One class = one responsibility
    

---

### Why It Exists

If a class does many things:

- Hard to understand
    
- Hard to test
    
- Changes break unrelated logic
    

---

### Bad Design (Violates SRP)

```java
class UserService {
    void registerUser() { }

    void saveToDatabase() { }

    void sendEmail() { }
}
```

Problems:

- Business logic
    
- Persistence logic
    
- Email logic  
    all mixed together
    

---

### Good Design (Follows SRP)

```java
class UserService {
    void registerUser() { }
}

class UserRepository {
    void save() { }
}

class EmailService {
    void send() { }
}
```

Each class has **one responsibility**.

---

### Interview Line

> SRP means a class should have only one reason to change.

---

## O – Open/Closed Principle (OCP)

### What It Means

> Software entities should be **open for extension** but **closed for modification**.

---

### Why It Exists

When requirements change:

- You should add new code
    
- Not modify existing tested code
    

---

### Bad Design

```java
class PaymentService {
    void pay(String type) {
        if (type.equals("CARD")) { }
        else if (type.equals("UPI")) { }
    }
}
```

Adding new payment → modify this class ❌

---

### Good Design (Using Polymorphism)

```java
interface Payment {
    void pay();
}

class CardPayment implements Payment {
    public void pay() { }
}

class UpiPayment implements Payment {
    public void pay() { }
}
```

```java
class PaymentService {
    void process(Payment payment) {
        payment.pay();
    }
}
```

Add new payment = new class  
No modification needed

---

### Interview Line

> OCP avoids modifying existing code by using abstraction.

---

## L – Liskov Substitution Principle (LSP)

### What It Means

> A subclass should be **replaceable** for its superclass **without breaking behavior**.

---

### Why It Exists

Inheritance should not change expected behavior.

---

### Bad Design

```java
class Bird {
    void fly() { }
}

class Penguin extends Bird {
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```

Penguin is a Bird, but cannot fly ❌

---

### Good Design

```java
interface Bird { }

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() { }
}

class Penguin implements Bird {
}
```

Behavior is preserved.

---

### Interview Line

> Subclasses must honor the behavior of their parent class.

---

## I – Interface Segregation Principle (ISP)

### What It Means

> Clients should not be forced to depend on interfaces they do not use.

---

### Why It Exists

Large interfaces:

- Force unnecessary method implementations
    
- Create weak designs
    

---

### Bad Design

```java
interface Machine {
    void print();
    void scan();
    void fax();
}
```

```java
class Printer implements Machine {
    public void fax() { } // unnecessary
}
```

---

### Good Design

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}
```

```java
class SimplePrinter implements Printer {
    public void print() { }
}
```

---

### Interview Line

> ISP promotes small, focused interfaces.

---

## D – Dependency Inversion Principle (DIP)

### What It Means

> High-level modules should not depend on low-level modules.  
> Both should depend on abstractions.

---

### Why It Exists

Direct dependencies:

- Tight coupling
    
- Hard to test
    
- Hard to replace implementations
    

---

### Bad Design

```java
class UserService {
    private MySqlDatabase db = new MySqlDatabase();
}
```

UserService depends on a concrete DB ❌

---

### Good Design

```java
interface Database {
    void save();
}
```

```java
class MySqlDatabase implements Database {
    public void save() { }
}
```

```java
class UserService {
    private Database db;

    UserService(Database db) {
        this.db = db;
    }
}
```

Now:

- Database can change
    
- UserService stays unchanged
    

---

### Interview Line

> DIP reduces coupling by depending on abstractions.

---

## How SOLID Relates to Spring Framework

|SOLID|Spring Usage|
|---|---|
|SRP|@Service, @Repository separation|
|OCP|Adding new implementations|
|LSP|Interface-based design|
|ISP|Small Spring interfaces|
|DIP|Dependency Injection|

Spring **enforces SOLID naturally**.

---

## Common Mistakes with SOLID

- Over-engineering small projects
    
- Too many interfaces unnecessarily
    
- Confusing SRP with “one method per class”
    
- Using inheritance instead of composition
    

---

## Final Mental Model

```
S → One responsibility
O → Extend, don’t modify
L → Don’t break parent behavior
I → Small interfaces
D → Depend on abstractions
```

---

## One-Line Interview Summary

> SOLID principles help create flexible, maintainable, and testable object-oriented systems by promoting proper responsibility separation, abstraction, and loose coupling.

---
