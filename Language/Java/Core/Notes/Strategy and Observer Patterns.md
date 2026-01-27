## **Strategy Design Pattern**

written for **core Java, Spring Boot, interviews, and real-world systems**.  
No emoticons are used.
## 1. Overview

**Design Pattern Type:** Behavioral  
**Main Goal:** Define a family of algorithms, encapsulate each one, and make them interchangeable.  
**Core Idea:**

> The behavior of a class can be changed at runtime without modifying its code.

In simple terms:  
**Strategy pattern lets you select an algorithm dynamically.**

---

## 2. The Problem Strategy Pattern Solves

### 2.1 Problem with Conditional Logic

```java
class PaymentService {

    public void pay(String type, double amount) {
        if (type.equals("CARD")) {
            // credit card logic
        } else if (type.equals("UPI")) {
            // upi logic
        } else if (type.equals("NETBANKING")) {
            // net banking logic
        }
    }
}
```

Problems:

- Too many `if-else` or `switch` statements
    
- Violates Open/Closed Principle
    
- Difficult to add new behavior
    
- Hard to test individual strategies
    
- Code becomes fragile and complex
    

---

## 3. What Strategy Pattern Solves

Strategy Pattern:

- Eliminates conditional logic
    
- Encapsulates each algorithm in a separate class
    
- Makes algorithms interchangeable
    
- Improves readability and testability
    
- Follows SOLID principles
    

---

## 4. Definition (Interview-Ready)

> Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable so that the algorithm can vary independently from the client that uses it.

---

## 5. When to Use Strategy Pattern

Use Strategy when:

- You have multiple ways to perform an operation
    
- You see large `if-else` or `switch` blocks
    
- You want to change behavior at runtime
    
- You want to follow Open/Closed Principle
    

---

## 6. Structure of Strategy Pattern

### Key Components

1. **Strategy Interface** – Common behavior
    
2. **Concrete Strategies** – Different implementations
    
3. **Context** – Uses a strategy
    
4. **Client** – Chooses the strategy
    

---

## 7. Strategy Pattern (Step-by-Step Example)

### Step 1: Strategy Interface

```java
public interface PaymentStrategy {
    void pay(double amount);
}
```

---

### Step 2: Concrete Strategies

```java
public class CreditCardPaymentStrategy implements PaymentStrategy {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}
```

```java
public class UpiPaymentStrategy implements PaymentStrategy {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}
```

---

### Step 3: Context Class

```java
public class PaymentContext {

    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void executePayment(double amount) {
        paymentStrategy.pay(amount);
    }
}
```

---

### Step 4: Client Code

```java
PaymentContext context = new PaymentContext();

context.setPaymentStrategy(new UpiPaymentStrategy());
context.executePayment(500);

context.setPaymentStrategy(new CreditCardPaymentStrategy());
context.executePayment(1000);
```

Behavior changes **at runtime** without modifying `PaymentContext`.

---

## 8. Key Characteristics of Strategy Pattern

- Uses composition, not inheritance
    
- Behavior is interchangeable
    
- Open for extension, closed for modification
    
- Eliminates conditional logic
    
- Promotes loose coupling
    

---

## 9. Strategy Pattern with Dependency Injection (Spring Boot)

### Strategy Interface

```java
public interface NotificationStrategy {
    void send(String message);
}
```

---

### Implementations

```java
@Service("email")
public class EmailNotificationStrategy implements NotificationStrategy {
    public void send(String message) {
        System.out.println("Email sent: " + message);
    }
}
```

```java
@Service("sms")
public class SmsNotificationStrategy implements NotificationStrategy {
    public void send(String message) {
        System.out.println("SMS sent: " + message);
    }
}
```

---

### Strategy Resolver (Context)

```java
@Service
public class NotificationService {

    private final Map<String, NotificationStrategy> strategies;

    public NotificationService(Map<String, NotificationStrategy> strategies) {
        this.strategies = strategies;
    }

    public void notify(String type, String message) {
        NotificationStrategy strategy = strategies.get(type);
        strategy.send(message);
    }
}
```

This is a **real-world Spring Boot Strategy Pattern**.

---

## 10. Strategy Pattern vs Factory Method

|Strategy Pattern|Factory Method|
|---|---|
|Chooses behavior|Chooses object|
|Runtime behavior change|Object creation|
|Focus on algorithms|Focus on instantiation|
|Used during execution|Used during creation|

Often **Factory creates Strategy objects**.

---

## 11. Strategy Pattern vs State Pattern (Interview Trap)

|Strategy|State|
|---|---|
|Client selects strategy|State changes internally|
|Stateless|State-driven|
|External control|Internal transitions|

---

## 12. Advantages

- Removes conditional complexity
    
- Encourages clean architecture
    
- Easy to add new strategies
    
- Easier unit testing
    
- Strong alignment with SOLID
    

---

## 13. Disadvantages

- Increases number of classes
    
- Client must know which strategy to choose
    
- Slightly more complex design
    

---

## 14. When NOT to Use Strategy Pattern

Avoid Strategy when:

- Only one algorithm exists
    
- Behavior never changes
    
- Over-engineering simple logic
    

---

## 15. Interview Questions and Answers

**Q1. What problem does Strategy Pattern solve?**  
It removes conditional logic and allows dynamic behavior selection.

**Q2. How is Strategy different from Factory?**  
Factory creates objects; Strategy changes behavior.

**Q3. Does Spring use Strategy Pattern?**  
Yes. Examples include `HandlerMapping`, `ViewResolver`, `AuthenticationProvider`.

**Q4. Is Strategy runtime or compile-time?**  
Runtime behavior selection.

---

## 16. Relation to SOLID Principles

- **Single Responsibility:** Each strategy handles one behavior
    
- **Open/Closed:** New strategies added without modifying context
    
- **Dependency Inversion:** Context depends on interface
    

---

## 17. Real-World Use Cases

- Payment processing
    
- Sorting algorithms
    
- Compression algorithms
    
- Authentication mechanisms
    
- Notification systems
    

---

## 18. Key Takeaways

- Strategy pattern encapsulates algorithms
    
- Promotes composition over inheritance
    
- Enables runtime behavior change
    
- Widely used in Spring Boot
    
- Essential for clean and scalable systems
    

---

## 19. One-Line Summary

> Strategy Pattern allows selecting an algorithm dynamically without changing the client code.

---

Below is a **complete, very detailed, and clear note** on the

## **Observer Design Pattern**

written for **core Java, Spring Boot, interviews, and real-world event-driven systems**.  
No emoticons are used.

---

# Observer Design Pattern (Complete & Detailed Note)

## 1. Overview

**Design Pattern Type:** Behavioral  
**Main Goal:** Define a one-to-many dependency so that when one object changes state, all its dependents are notified automatically.  
**Core Idea:**

> Observers subscribe to a subject, and the subject notifies all observers when something changes.

In simple terms:  
**Observer enables event-driven communication without tight coupling.**

---

## 2. The Problem Observer Pattern Solves

### 2.1 Problem with Tight Coupling

```java
class OrderService {

    public void placeOrder() {
        sendEmail();
        sendSms();
        updateAnalytics();
        generateInvoice();
    }
}
```

Problems:

- OrderService knows too much
    
- Every new action requires modifying OrderService
    
- Violates Open/Closed Principle
    
- Hard to test and maintain
    
- Poor scalability
    

---

## 3. What Observer Pattern Solves

Observer Pattern:

- Decouples event producer from event consumers
    
- Allows multiple listeners to react independently
    
- Makes system extensible
    
- Supports event-driven architecture
    
- Improves maintainability
    

---

## 4. Definition (Interview-Ready)

> Observer Pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependent observers are notified automatically.

---

## 5. When to Use Observer Pattern

Use Observer when:

- One change should trigger multiple actions
    
- You want loose coupling
    
- Event-driven behavior is required
    
- Listeners may change over time
    
- You want to follow Open/Closed Principle
    

---

## 6. Structure of Observer Pattern

### Key Components

1. **Subject** – Maintains observers and notifies them
    
2. **Observer** – Interface for notification
    
3. **Concrete Subject** – Implements subject logic
    
4. **Concrete Observer** – Reacts to updates
    

---

## 7. Observer Pattern (Step-by-Step Example)

### Step 1: Observer Interface

```java
public interface Observer {
    void update(String message);
}
```

---

### Step 2: Subject Interface

```java
public interface Subject {
    void register(Observer observer);
    void unregister(Observer observer);
    void notifyObservers(String message);
}
```

---

### Step 3: Concrete Subject

```java
import java.util.ArrayList;
import java.util.List;

public class OrderSubject implements Subject {

    private final List<Observer> observers = new ArrayList<>();

    @Override
    public void register(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void unregister(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void orderPlaced() {
        notifyObservers("Order has been placed");
    }
}
```

---

### Step 4: Concrete Observers

```java
public class EmailObserver implements Observer {
    public void update(String message) {
        System.out.println("Email sent: " + message);
    }
}
```

```java
public class SmsObserver implements Observer {
    public void update(String message) {
        System.out.println("SMS sent: " + message);
    }
}
```

---

### Step 5: Client Code

```java
OrderSubject subject = new OrderSubject();

subject.register(new EmailObserver());
subject.register(new SmsObserver());

subject.orderPlaced();
```

OrderSubject does not know _what_ observers do.

---

## 8. Key Characteristics of Observer Pattern

- Loose coupling
    
- One-to-many relationship
    
- Push-based notification
    
- Dynamic subscription
    
- Event-driven behavior
    

---

## 9. Observer Pattern in Java Standard Library

### java.util.Observer (Deprecated)

```java
Observable observable;
Observer observer;
```

Deprecated because:

- Inheritance-based
    
- Poor flexibility
    
- Thread-safety issues
    

Modern systems use **custom observer or event frameworks**.

---

## 10. Observer Pattern in Spring Boot (VERY IMPORTANT)

Spring heavily uses Observer pattern via **Events**.

---

### 10.1 Spring Event Model

- **Publisher** → Event
    
- **Listener** → Reacts to event
    
- Fully decoupled
    

---

### Step 1: Custom Event

```java
public class OrderPlacedEvent {
    private final String orderId;

    public OrderPlacedEvent(String orderId) {
        this.orderId = orderId;
    }

    public String getOrderId() {
        return orderId;
    }
}
```

---

### Step 2: Event Publisher

```java
@Service
public class OrderService {

    private final ApplicationEventPublisher publisher;

    public OrderService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void placeOrder(String orderId) {
        publisher.publishEvent(new OrderPlacedEvent(orderId));
    }
}
```

---

### Step 3: Event Listeners (Observers)

```java
@Component
public class EmailListener {

    @EventListener
    public void handleOrder(OrderPlacedEvent event) {
        System.out.println("Email sent for order " + event.getOrderId());
    }
}
```

```java
@Component
public class AnalyticsListener {

    @EventListener
    public void handleOrder(OrderPlacedEvent event) {
        System.out.println("Analytics updated for order " + event.getOrderId());
    }
}
```

No change needed in OrderService to add new listeners.

---

## 11. Synchronous vs Asynchronous Observers

### Default: Synchronous

- Listeners execute in publisher thread
    

### Asynchronous (Recommended for heavy tasks)

```java
@EnableAsync
@Component
public class SmsListener {

    @Async
    @EventListener
    public void handleOrder(OrderPlacedEvent event) {
        System.out.println("SMS sent asynchronously");
    }
}
```

---

## 12. Observer Pattern vs Strategy Pattern

|Observer|Strategy|
|---|---|
|One-to-many|One-to-one|
|Event-driven|Behavior-driven|
|All observers notified|One strategy chosen|
|Passive listeners|Active execution|

---

## 13. Advantages

- Loose coupling
    
- Open for extension
    
- Scalable event handling
    
- Clean architecture
    
- Easy to test listeners
    

---

## 14. Disadvantages

- Debugging can be harder
    
- Execution order is not guaranteed
    
- Too many observers may impact performance
    

---

## 15. When NOT to Use Observer Pattern

Avoid Observer when:

- Only one dependent exists
    
- Behavior must be strictly ordered
    
- Simple method calls suffice
    
- Overuse causes complexity
    

---

## 16. Interview Questions and Answers

**Q1. What problem does Observer solve?**  
It decouples event producers from consumers.

**Q2. Is Observer synchronous or asynchronous?**  
Both; depends on implementation.

**Q3. Does Spring use Observer pattern?**  
Yes, via Application Events.

**Q4. Difference between Observer and Pub-Sub?**  
Observer is in-process; Pub-Sub is distributed.

---

## 17. Relation to SOLID Principles

- **Single Responsibility:** Observers handle single concern
    
- **Open/Closed:** New observers without modifying subject
    
- **Dependency Inversion:** Depends on abstractions
    

---

## 18. Real-World Use Cases

- Order lifecycle events
    
- Notification systems
    
- Audit logging
    
- Metrics and monitoring
    
- Domain-driven design events
    

---

## 19. Key Takeaways

- Observer enables event-driven design
    
- Reduces coupling
    
- Widely used in Spring Boot
    
- Supports scalability and extensibility
    
- Essential for modern backend systems
    

---

## 20. One-Line Summary

> Observer Pattern allows multiple independent objects to react automatically to changes in another object.

---
