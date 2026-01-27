## 1. Overview

**Design Pattern Type:** Creational  
**Main Goal:** Construct complex objects step by step  
**Core Idea:**

> Separate the construction of a complex object from its representation so that the same construction process can create different representations.

In simple words:  
**Builder helps when an object has many optional fields and complex construction logic.**

---

## 2. The Problem Builder Solves

### 2.1 Problem with Telescoping Constructors

```java
class User {
    String name;
    int age;
    String email;
    String phone;
    String address;

    User(String name) { }
    User(String name, int age) { }
    User(String name, int age, String email) { }
    User(String name, int age, String email, String phone) { }
}
```

Problems:

- Too many constructors
    
- Hard to read and maintain
    
- Error-prone parameter order
    
- Poor readability
    

---

### 2.2 Problem with Setters

```java
User user = new User();
user.setName("Anupam");
user.setAge(25);
user.setEmail("x@y.com");
```

Problems:

- Object can be in an **incomplete state**
    
- Mutability issues
    
- No guarantee required fields are set
    

---

## 3. What Builder Pattern Solves

Builder Pattern:

- Avoids constructor explosion
    
- Makes object creation readable
    
- Allows immutability
    
- Enforces required fields
    
- Builds objects step by step
    

---

## 4. Definition (Interview-Ready)

> Builder Pattern separates the construction of a complex object from its representation, allowing the same construction process to create different types of objects.

---

## 5. When to Use Builder Pattern

Use Builder when:

- Object has many optional parameters
    
- You want immutable objects
    
- Construction logic is complex
    
- Readability matters
    
- You want to avoid setters
    

---

## 6. Basic Builder Pattern Structure

### Components

1. **Product** – The object being built
    
2. **Builder** – Defines steps to build the product
    
3. **Concrete Builder** – Implements builder steps
    
4. **Director (Optional)** – Controls construction order
    

In Java, **Director is often skipped**.

---

## 7. Simple Builder Example (Step by Step)

### Step 1: Product Class

```java
class User {

    private final String name;
    private final int age;
    private final String email;
    private final String phone;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {
        private final String name;   // required
        private int age;             // optional
        private String email;
        private String phone;

        public Builder(String name) {
            this.name = name;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

---

### Step 2: Usage

```java
User user = new User.Builder("Anupam")
        .age(25)
        .email("x@y.com")
        .build();
```

Benefits:

- Clear
    
- Readable
    
- No constructor confusion
    
- Immutable object
    

---

## 8. Key Characteristics of Builder Pattern

- Fluent API
    
- Method chaining
    
- Step-by-step object creation
    
- Immutable final object
    
- Optional fields supported
    

---

## 9. Builder vs Constructor vs Setter

|Feature|Constructor|Setter|Builder|
|---|---|---|---|
|Readability|Poor|Medium|Excellent|
|Optional fields|Hard|Easy|Easy|
|Immutability|Yes|No|Yes|
|Validation|Limited|Weak|Strong|
|Object consistency|Yes|No|Yes|

---

## 10. Builder Pattern with Validation

```java
public User build() {
    if (age < 0) {
        throw new IllegalStateException("Age cannot be negative");
    }
    return new User(this);
}
```

Builder is the **best place** for validation logic.

---

## 11. Builder Pattern in Real Java APIs

### Example: StringBuilder

```java
String result = new StringBuilder()
        .append("Hello")
        .append(" ")
        .append("World")
        .toString();
```

This is a classic Builder pattern.

---

### Example: Lombok @Builder

```java
@Builder
class User {
    String name;
    int age;
    String email;
}
```

Usage:

```java
User user = User.builder()
        .name("Anupam")
        .age(25)
        .build();
```

Internally, Lombok generates Builder code.

---

## 12. Builder Pattern in Spring Boot

### Common Use Cases

- DTO creation
    
- API response objects
    
- Configuration objects
    
- Immutable domain models
    

Example:

```java
ResponseEntity<ResponseDto> response =
        ResponseEntity.ok(
                ResponseDto.builder()
                        .status("SUCCESS")
                        .message("Order placed")
                        .build()
        );
```

---

## 13. Builder vs Factory Method (Important Difference)

|Builder|Factory Method|
|---|---|
|Builds complex objects|Creates objects|
|Step-by-step|One-step|
|Focus on how to build|Focus on what to build|
|Handles many parameters|Chooses implementation|

Often, **Factory + Builder** are used together.

---

## 14. When NOT to Use Builder Pattern

Avoid Builder when:

- Object has very few fields
    
- All fields are mandatory
    
- Object creation is simple
    
- Over-engineering small classes
    

---

## 15. Common Interview Questions

**Q1. Why Builder instead of constructor?**  
To handle optional parameters cleanly and safely.

**Q2. Is Builder immutable?**  
The final object is immutable; builder itself is mutable.

**Q3. Difference between Builder and Prototype?**  
Builder constructs step by step; Prototype clones objects.

**Q4. Does Builder violate SRP?**  
No. Builder handles construction; product handles behavior.

---

## 16. Relation to SOLID Principles

- **Single Responsibility:** Construction separated
    
- **Open/Closed:** New build steps without breaking usage
    
- **Dependency Inversion:** Client depends on abstraction, not construction
    

---

## 17. Key Takeaways

- Builder is best for complex objects
    
- Improves readability and safety
    
- Encourages immutability
    
- Widely used in Java APIs and Spring Boot
    
- Prevents constructor explosion
    

---

## 18. One-Line Summary

> Builder Pattern constructs complex objects step by step while keeping them immutable and readable.

---
