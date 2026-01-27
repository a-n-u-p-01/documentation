## 1. Overview

**Category:** Object-Oriented Design / Best Practice  
**Main Goal:** Create objects whose state **cannot change after creation**  
**Core Idea:**

> An immutable object’s data is fixed at construction time and never changes afterward.

---

## 2. What Is an Immutable Class?

> An immutable class is a class whose instances cannot be modified once they are created.

This means:

- No setter methods
    
- Fields do not change
    
- Any “change” results in a **new object**
    

---

## 3. Why Immutability Is Important

### Problems with Mutable Objects

- Unexpected side effects
    
- Thread-safety issues
    
- Hard-to-debug bugs
    
- Data inconsistency in concurrent systems
    

### What Immutability Solves

- Thread safety without synchronization
    
- Safer sharing between components
    
- Predictable behavior
    
- Easier reasoning and testing
    

---

## 4. Where Immutability Is Used in Java

Java uses immutability extensively:

- `String`
    
- `Integer`, `Long`, `Boolean`
    
- `LocalDate`, `LocalDateTime`
    
- `BigDecimal`
    
- Records (Java 16+)
    

This is a strong indicator of its importance.

---

## 5. Rules to Create an Immutable Class (MANDATORY)

To design an immutable class, follow **all rules strictly**.

---

### Rule 1: Make the Class `final`

Prevents subclassing, which could introduce mutability.

```java
public final class User {
}
```

---

### Rule 2: Make All Fields `private final`

Ensures fields are set only once.

```java
private final String name;
private final int age;
```

---

### Rule 3: Initialize Fields via Constructor Only

```java
public User(String name, int age) {
    this.name = name;
    this.age = age;
}
```

---

### Rule 4: No Setter Methods

Do not provide any methods that modify state.

```java
// NO setName(), setAge()
```

---

### Rule 5: Return Defensive Copies for Mutable Fields

If the class contains mutable objects (like `Date`, `List`, `Map`), never expose them directly.

---

## 6. Simple Immutable Class Example

```java
public final class User {

    private final String name;
    private final int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

This class is fully immutable.

---

## 7. Immutability with Mutable Fields (IMPORTANT)

### ❌ Wrong Design

```java
public final class Order {

    private final List<String> items;

    public Order(List<String> items) {
        this.items = items; // reference leak
    }

    public List<String> getItems() {
        return items; // exposes internal state
    }
}
```

Caller can modify the list.

---

### ✅ Correct Design (Defensive Copy)

```java
public final class Order {

    private final List<String> items;

    public Order(List<String> items) {
        this.items = new ArrayList<>(items);
    }

    public List<String> getItems() {
        return new ArrayList<>(items);
    }
}
```

Now the internal state is protected.

---

## 8. Immutable Object Modification Pattern

Immutable objects are “modified” by **creating new objects**.

```java
public User withAge(int newAge) {
    return new User(this.name, newAge);
}
```

Original object remains unchanged.

---

## 9. Immutability and Thread Safety

Immutable objects are:

- Naturally thread-safe
    
- Shareable across threads
    
- Do not require synchronization
    

This is why immutable objects are preferred in:

- Multithreading
    
- Microservices
    
- Functional programming
    
- Event-driven systems
    

---

## 10. Immutable Class vs Mutable Class

|Aspect|Immutable|Mutable|
|---|---|---|
|State change|Not allowed|Allowed|
|Thread safety|Yes|No (by default)|
|Side effects|None|Possible|
|Debugging|Easy|Hard|
|Performance|Good (safe sharing)|Risky|

---

## 11. Immutability in Spring Boot

### Where It Is Used

- DTOs
    
- API responses
    
- Configuration properties
    
- Value objects
    
- Domain events
    

---

### Example: Immutable DTO

```java
public final class ApiResponse {

    private final String status;
    private final String message;

    public ApiResponse(String status, String message) {
        this.status = status;
        this.message = message;
    }

    public String getStatus() {
        return status;
    }

    public String getMessage() {
        return message;
    }
}
```

Safe to share across controllers and services.

---

## 12. Java Records (Modern Immutability)

```java
public record User(String name, int age) {
}
```

Features:

- Implicitly final
    
- Fields are private final
    
- Constructor auto-generated
    
- Immutable by design
    

Best choice for DTOs in modern Java.

---

## 13. Immutability vs Builder Pattern

|Builder|Immutable|
|---|---|
|Helps construction|Defines behavior|
|Builder is mutable|Final object immutable|
|Used during creation|Used during lifetime|

They work **together**, not against each other.

---

## 14. Common Interview Questions

**Q1. Why is String immutable?**  
For security, caching, thread safety, and performance.

**Q2. Can an immutable class have mutable fields?**  
Yes, but only with defensive copying.

**Q3. Is immutability mandatory for thread safety?**  
No, but it is the safest and simplest approach.

**Q4. Are records immutable?**  
Yes, by design.

---

## 15. Common Mistakes

- Exposing mutable internal objects
    
- Forgetting defensive copies
    
- Allowing subclassing
    
- Adding setters accidentally
    

---

## 16. Relation to SOLID Principles

- **Single Responsibility:** Clear state ownership
    
- **Open/Closed:** State cannot be altered unexpectedly
    
- **Liskov Substitution:** Behavior remains predictable
    

---

## 17. When NOT to Use Immutability

Avoid immutability when:

- Object state changes frequently
    
- Performance requires in-place updates
    
- Large objects updated very often
    

In such cases, use controlled mutability.

---

## 18. Key Takeaways

- Immutability improves safety and clarity
    
- Essential for concurrent systems
    
- Widely used in Java core libraries
    
- Highly preferred in modern backend design
    
- Works best with Builder and Factory patterns
    

---

## 19. One-Line Summary

> An immutable class guarantees that its state never changes after creation, making it safe, predictable, and thread-friendly.
