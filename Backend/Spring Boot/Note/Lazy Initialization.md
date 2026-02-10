## 1. What is Lazy Initialization?

Lazy Initialization means:

> A bean is created **only when it is first needed**, not at application startup.

By default, Spring uses **eager initialization**, where singleton beans are created during container startup.

Lazy initialization delays that creation.

---

## 2. Default Behavior (Important)

Spring normally follows:

```
Application Start → Create all singleton beans → App ready
```

This helps detect configuration errors early.

With lazy initialization:

```
Application Start → Bean NOT created
First Usage → Bean created
```

---

## 3. How to Enable Lazy Initialization

### On a Specific Bean

```java
@Component
@Lazy
public class HeavyService {
}
```

Spring waits until the bean is requested.

---

### With Java Configuration

```java
@Bean
@Lazy
public ReportService reportService() {
    return new ReportService();
}
```

---

### Injecting Lazily

You can delay dependency creation even if the main bean is initialized.

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(@Lazy PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

`PaymentService` is created only when used.

---

### Global Lazy Initialization (Spring Boot)

```properties
spring.main.lazy-initialization=true
```

Now ALL beans become lazy unless specified otherwise.

Useful for very large applications.

---

## 4. When Should You Use Lazy Initialization?

### Heavy Object Creation

Beans that consume time or resources:

- ML models
    
- Large caches
    
- External SDK clients
    
- Report engines
    

If rarely used, eager creation wastes startup time.

---

### Improve Startup Speed

Lazy initialization can significantly reduce boot time in large microservices.

Helpful in:

- Cloud deployments
    
- Auto-scaling environments
    
- Serverless setups
    

---

### Break Circular Dependencies (Situational)

Lazy injection can sometimes resolve circular references by delaying creation.

But this should NOT be your primary solution — redesign is better.

---

## 5. Advantages

### Faster Application Startup

Only critical beans load initially.

### Better Resource Usage

Unused beans never consume memory or CPU.

### Useful for Optional Features

Example:

- Admin modules
    
- Batch processors
    
- Analytics engines
    

Load only when needed.

---

## 6. Disadvantages (VERY Important for Interviews)

### Delayed Failure

Errors appear at runtime instead of startup.

Example:

- Missing configuration
    
- Invalid bean wiring
    

Your app starts successfully but crashes when the bean is used.

Startup failure is often safer.

---

### First-Use Latency

The first request may be slower because Spring must create the bean.

Bad for performance-critical APIs.

---

### Harder Debugging

Problems surface later, making them harder to trace.

---

## 7. Lazy vs Eager — Quick Comparison

|Feature|Eager|Lazy|
|---|---|---|
|Creation Time|Startup|First use|
|Startup Speed|Slower|Faster|
|Failure Detection|Early|Delayed|
|Memory Usage|Higher initially|Lower initially|
|Recommended Default|Yes|Situational|

**Industry Rule:**  
Prefer eager unless you have a clear reason.

---

## 8. Important Internal Behavior

When a lazy bean is injected, Spring often injects a **proxy object** instead of the real bean.

Flow:

```
Client → Proxy → Real Bean (created on first call)
```

The proxy triggers creation only when necessary.

This mechanism is automatic.

---

## 9. Common Use Cases in Production

### Rarely Used Services

Example:

- Data export service
    
- Backup processor
    
- Feature toggle modules
    

---

### Expensive Network Clients

Avoid connecting at startup if not immediately needed.

---

### Plugin-Based Architectures

Load modules dynamically.

---

## 10. Common Developer Mistakes

### Making Everything Lazy

This hides configuration problems.

Bad practice.

---

### Using Lazy to Fix Bad Design

Circular dependencies should be redesigned, not masked.

---

### Lazily Initializing Critical Beans

Authentication or database beans should NOT be lazy.

You want them ready immediately.

---

## 11. High-Probability Interview Questions

### What is lazy initialization?

Creating beans only when first requested rather than at startup.

---

### What is the default initialization type?

Eager.

---

### What is the biggest risk of lazy initialization?

Delayed runtime failures.

---

### How do you enable it globally?

```
spring.main.lazy-initialization=true
```

---

### Does lazy improve performance?

It improves startup time but may slow the first request.

---

## 12. When NOT to Use Lazy

Avoid it for:

- Security components
    
- Database connections
    
- Core services
    
- API-critical beans
    

These should be ready immediately.

---

# Memory Shortcut

```
Eager → Create now  
Lazy → Create later
```

---

# Final Takeaway

Lazy initialization is a **performance optimization tool**, not a default strategy.

Use it when:

- Bean creation is expensive
    
- Feature usage is rare
    
- Faster startup is required
    

Avoid it when reliability and early failure detection are more important.

**Professional Guideline:**

> Start eager. Introduce lazy only where it provides measurable benefit.