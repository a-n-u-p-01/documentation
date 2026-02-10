Spring stereotype annotations are used to **mark classes as Spring-managed beans** so that the container can automatically detect and register them during component scanning.

They are a core part of building layered Spring applications and directly support **Dependency Injection**.

---

## 1. What Are Stereotype Annotations?

Stereotype annotations tell Spring:

> “This class should be managed by the IoC container.”

Once registered as a bean, the class becomes eligible for:

- Dependency Injection
    
- Lifecycle management
    
- AOP (transactions, logging, security)
    
- Exception translation
    

These annotations belong to the **component scanning mechanism**.

When Spring Boot starts, it scans packages (by default, the package of the main application class and its subpackages) and automatically registers annotated classes.

---

## 2. @Component — The Base Annotation

### Definition

`@Component` is the **generic stereotype annotation** used to declare a Spring bean.

```java
@Component
public class EmailUtility {
    public void sendEmail() {
        System.out.println("Sending email...");
    }
}
```

Spring detects it and creates a bean automatically.

### When to Use

Use `@Component` for classes that:

- Do not clearly belong to service or repository layers
    
- Provide helper functionality
    
- Represent utilities
    
- Handle cross-cutting concerns
    

Examples:

- Email helpers
    
- File processors
    
- Token generators
    
- Mappers (sometimes)
    

### Important Insight

`@Component` is the **parent annotation** of:

- `@Service`
    
- `@Repository`
    
- `@Controller`
    
- `@RestController`
    

Internally, they all behave like `@Component` but add semantic meaning.

---

## 3. @Service — Business Logic Layer

### Definition

`@Service` marks a class that contains **business logic**.

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### Purpose

It communicates that this class:

- Implements application rules
    
- Coordinates workflows
    
- Performs calculations
    
- Enforces domain constraints
    

### Why Not Just Use @Component?

Because `@Service` improves:

#### Readability

Developers instantly know this class belongs to the service layer.

#### Maintainability

Layered architecture becomes clearer.

#### AOP Compatibility

Services are common targets for:

- Transactions (`@Transactional`)
    
- Security
    
- Logging
    

### Interview Insight

There is **no major functional difference** between `@Component` and `@Service`.

The difference is **semantic** — it expresses intent.

**Good Interview Line:**

> "@Service is a specialization of @Component used to represent business logic."

---

## 4. @Repository — Persistence Layer

### Definition

`@Repository` marks classes responsible for **database operations**.

```java
@Repository
public class UserRepository {
    // database logic
}
```

In modern Spring Boot apps, interfaces extending `JpaRepository` are automatically treated as repositories — you often don’t need to annotate them manually.

Example:

```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```

Spring registers it automatically.

---

## Special Capability: Exception Translation

This is what makes `@Repository` unique.

Spring converts low-level database exceptions into **Spring’s DataAccessException hierarchy**.

Example:

Instead of throwing vendor-specific exceptions like:

- SQLException
    
- HibernateException
    

Spring throws consistent runtime exceptions such as:

- DataIntegrityViolationException
    
- DuplicateKeyException
    

### Why This Matters

Your service layer stays database-agnostic.

Switching from MySQL to PostgreSQL will not break exception handling logic.

### Interview Gold Statement:

> "@Repository provides automatic exception translation for persistence errors."

---

## 5. How Component Scanning Works

Spring Boot automatically scans from the package containing the main class.

```
@SpringBootApplication
```

This annotation internally includes:

```
@ComponentScan
```

### Example Structure

```
com.app
 ├── controller
 ├── service
 ├── repository
```

All subpackages are scanned automatically.

### Common Mistake

Placing classes outside the base package prevents them from being detected.

Fix by either:

- Moving the class
    
- Or using explicit `@ComponentScan`
    

---

## 6. @Component vs @Service vs @Repository (Comparison)

|Feature|@Component|@Service|@Repository|
|---|---|---|---|
|Purpose|Generic bean|Business logic|Data access|
|Semantic Meaning|None|Service layer|Persistence layer|
|Exception Translation|No|No|Yes|
|Readability|Lower|High|High|
|Typical Usage|Utilities|Application logic|Database operations|

**Key Point:**  
Use the most specific annotation available.

---

## 7. Typical Layered Architecture

A standard Spring Boot structure looks like:

```
Controller → Service → Repository → Database
```

### Flow:

- Controller receives request
    
- Service processes business logic
    
- Repository interacts with DB
    

Using proper stereotypes enforces this separation.

---

## 8. Best Practices

### Prefer Specific Annotations

Use:

- `@Service` instead of `@Component`
    
- `@Repository` for persistence
    

This improves code clarity.

---

### Keep Responsibilities Separate

Do not mix layers.

Bad practice:

- Database logic inside service
    
- Business logic inside repository
    

Follow **Single Responsibility Principle**.

---

### Combine with Constructor Injection

Most production code follows:

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

---

## 9. Common Interview Questions

### What are stereotype annotations?

Annotations that mark classes as Spring beans for automatic detection.

---

### Is @Service different from @Component?

Functionally similar, but `@Service` adds semantic clarity for the business layer.

---

### What is special about @Repository?

Automatic exception translation into Spring’s data access exceptions.

---

### Can we replace @Service with @Component?

Yes, but it is discouraged because it reduces readability and architectural clarity.

---

### Do we need @Repository with Spring Data JPA?

Usually not — Spring auto-detects repository interfaces.

---

## 10. Common Developer Mistakes

- Using `@Component` everywhere instead of specialized annotations
    
- Mixing business logic with persistence
    
- Not understanding exception translation
    
- Placing beans outside scanned packages
    
- Overloading services with too many responsibilities
    

These issues often lead to poor architecture.

---

## Final Takeaway

Stereotype annotations are more than markers — they help enforce clean architecture.

Understanding them signals that you know how to structure a professional Spring application.

**Core Rule:**

- `@Component` → generic
    
- `@Service` → business logic
    
- `@Repository` → data layer
    

Use the most specific annotation possible to build readable, maintainable, production-ready systems.