## 1. Overview

**Design Pattern Type:** Creational  
**Main Goal:** Encapsulate object creation logic  
**Core Idea:**

> Define an interface for creating an object, but let subclasses decide **which concrete class to instantiate**.

In short:  
**“Client code does not use `new` directly.”**

---

## 2. Why Factory Method Is Needed (The Real Problem)

### Problem with Direct Object Creation

```java
PaymentService paymentService = new CreditCardPayment();
```

Problems:

- Tight coupling with concrete class
    
- Hard to change implementation
    
- Violates **Open/Closed Principle**
    
- Difficult to test and extend
    

If tomorrow you add:

- UPI payment
    
- NetBanking payment
    
- Wallet payment
    

You must modify client code everywhere.

This is **bad design**.

---

## 3. What Factory Method Solves

Factory Method:

- Centralizes object creation
    
- Decouples client from concrete classes
    
- Makes code open for extension
    
- Improves maintainability and testability
    

---

## 4. Definition (Interview-Ready)

> Factory Method Pattern defines an interface for creating an object, but allows subclasses to alter the type of objects that will be created.

---

## 5. Structure of Factory Method Pattern

### Key Components

1. **Product (Interface / Abstract Class)**
    
2. **Concrete Products (Implementations)**
    
3. **Creator (Factory Interface / Abstract Class)**
    
4. **Concrete Creator (Factory Implementation)**
    
5. **Client (Uses factory, not `new`)**
    

---

## 6. Simple Example (Step by Step)

### Step 1: Product Interface

```java
public interface Payment {
    void pay(double amount);
}
```

---

### Step 2: Concrete Products

```java
public class CreditCardPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}
```

```java
public class UpiPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}
```

---

### Step 3: Factory Interface

```java
public interface PaymentFactory {
    Payment createPayment();
}
```

---

### Step 4: Concrete Factories

```java
public class CreditCardPaymentFactory implements PaymentFactory {
    @Override
    public Payment createPayment() {
        return new CreditCardPayment();
    }
}
```

```java
public class UpiPaymentFactory implements PaymentFactory {
    @Override
    public Payment createPayment() {
        return new UpiPayment();
    }
}
```

---

### Step 5: Client Code

```java
public class PaymentService {

    private final Payment payment;

    public PaymentService(PaymentFactory factory) {
        this.payment = factory.createPayment();
    }

    public void process(double amount) {
        payment.pay(amount);
    }
}
```

---

### Step 6: Usage

```java
PaymentFactory factory = new UpiPaymentFactory();
PaymentService service = new PaymentService(factory);
service.process(500);
```

---

## 7. Key Advantages

1. Loose coupling
    
2. No `new` keyword in client code
    
3. Easy to add new products
    
4. Follows SOLID principles
    
5. Improves testability
    

---

## 8. Factory Method vs Simple Factory (Very Important)

### Simple Factory (NOT a design pattern)

```java
class PaymentFactory {
    static Payment getPayment(String type) {
        if (type.equals("UPI")) return new UpiPayment();
        if (type.equals("CARD")) return new CreditCardPayment();
        throw new IllegalArgumentException();
    }
}
```

Problems:

- Violates Open/Closed Principle
    
- Requires modification for every new type
    

---

### Factory Method (REAL pattern)

- Uses inheritance
    
- Uses polymorphism
    
- Extensible without modifying existing code
    

---

## 9. Factory Method vs Abstract Factory

|Factory Method|Abstract Factory|
|---|---|
|Creates one product|Creates family of products|
|Simpler|More complex|
|One factory per product|Factory of factories|

Factory Method is the foundation for Abstract Factory.

---

## 10. Factory Method in Spring Boot (VERY IMPORTANT)

Spring uses Factory Method **everywhere**.

### Example: Bean Creation

```java
@Bean
public DataSource dataSource() {
    return new HikariDataSource();
}
```

- Spring decides when to call the method
    
- You only define creation logic
    

---

### Example: ApplicationContext

```java
ApplicationContext context =
        new AnnotationConfigApplicationContext(AppConfig.class);

UserService service = context.getBean(UserService.class);
```

Internally:

- Spring uses factory methods
    
- You never call `new UserService()`
    

---

## 11. Factory Method + Dependency Injection

Factory Method works best with DI.

```java
@Service
public class OrderService {

    private final PaymentFactory factory;

    public OrderService(PaymentFactory factory) {
        this.factory = factory;
    }
}
```

Spring injects the correct factory implementation.

---

## 12. When to Use Factory Method

Use it when:

- Object creation logic is complex
    
- You expect multiple implementations
    
- You want to hide instantiation details
    
- You want to follow OCP and DIP
    

---

## 13. When NOT to Use Factory Method

Avoid it when:

- Only one implementation exists
    
- Object creation is trivial
    
- Over-engineering simple code
    

---

## 14. Interview Questions and Answers

**Q1. What problem does Factory Method solve?**  
It removes tight coupling between client and concrete classes.

**Q2. Is Factory Method better than `new`?**  
Yes, when flexibility and extensibility are required.

**Q3. Does Spring use Factory Method?**  
Yes, extensively for bean creation.

**Q4. Difference between Factory and Builder?**  
Factory creates objects; Builder constructs complex objects step by step.

---

## 15. Real-Life Analogy (Conceptual)

- Restaurant menu → Order interface
    
- Different chefs → Concrete creators
    
- Customer → Client (does not know cooking process)
    

---

## 16. Relation to SOLID Principles

- **Single Responsibility:** Creation logic separated
    
- **Open/Closed:** New products without modifying client
    
- **Dependency Inversion:** Depends on abstractions
    

---

## 17. Key Takeaways

- Factory Method hides object creation
    
- Promotes loose coupling
    
- Essential for scalable design
    
- Used heavily in Spring framework
    
- Foundation for many advanced patterns
    

---

## 18. One-Line Summary

> Factory Method delegates object creation to subclasses and removes direct dependency on concrete classes.

---
