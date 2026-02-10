Bean scope defines **how many instances of a bean Spring creates** and **how long those instances live** inside the container.

Understanding scopes is important because it directly impacts:

- Memory usage
    
- Thread safety
    
- Performance
    
- Application design
    

---

# 1. What is Bean Scope?

Bean scope controls the **lifecycle and visibility** of a bean.

It answers two key questions:

> How many objects should Spring create?  
> Who can access them?

---

# 2. Types of Bean Scopes in Spring

Spring provides **six scopes**.

### Core Scopes (Most Important)

- Singleton (default)
    
- Prototype
    

### Web Scopes (Used in web applications)

- Request
    
- Session
    
- Application
    
- WebSocket
    

For interviews, **singleton + prototype are mandatory knowledge.**

---

# 3. Singleton Scope (Default)

## Definition

Only **ONE instance** of the bean exists for the entire Spring container.

Every request gets the same object.

---

## Behavior

```
UserService → same object → reused everywhere
```

Spring creates it once at startup and stores it in memory.

---

## Example

```java
@Service
@Scope("singleton") // optional, default
public class UserService {
}
```

Testing:

```java
UserService u1 = context.getBean(UserService.class);
UserService u2 = context.getBean(UserService.class);

System.out.println(u1 == u2);
```

Output:

```
true
```

---

## Why Singleton is Preferred

### Performance

Object creation is expensive. Singleton avoids repeated allocation.

### Memory Efficiency

Only one object is stored.

### Ideal for Stateless Beans

Most services do not hold user-specific data.

---

## Thread Safety Warning

Singleton beans are shared across threads.

Bad example:

```java
private int counter = 0;
```

Multiple threads modifying it → race conditions.

### Rule:

> Singleton beans should be stateless whenever possible.

---

## When to Use Singleton

Use for almost everything:

- Services
    
- Repositories
    
- Controllers
    
- Config classes
    
- Utility components
    
- Clients (DB, cache, HTTP)
    

**Industry Reality:**  
~90–95% of beans are singleton.

---

# 4. Prototype Scope

## Definition

Spring creates a **new instance every time the bean is requested.**

```
Request 1 → New Object  
Request 2 → New Object
```

---

## Example

```java
@Component
@Scope("prototype")
public class Engine {
}
```

Test:

```
e1 == e2 → false
```

---

## Critical Lifecycle Detail

For prototype beans, Spring:

- Creates the bean
    
- Injects dependencies
    
- Hands it to you
    

Then stops managing it.

Spring does NOT call destruction methods.

Cleanup becomes your responsibility.

---

## When Prototype Makes Sense

Use it for:

### Stateful Objects

Example:

- Shopping carts
    
- Temporary processors
    
- Per-user data
    

### Non-thread-safe Components

Each usage gets a fresh object.

---

## Major Interview Trap

### Injecting Prototype into Singleton

If a prototype bean is injected into a singleton:

Spring resolves it only once during singleton creation.

Result → same instance reused.

### How to Fix

Use lazy retrieval:

- ObjectProvider
    
- Provider
    
- ApplicationContext
    

This ensures a new instance each time.

---

# 5. Request Scope (Web Only)

## Definition

One bean instance per HTTP request.

Created at request start → destroyed after response.

---

## Example

```java
@Component
@Scope("request")
public class RequestTracker {
}
```

Each API call gets a separate object.

---

## Typical Use Cases

- Request metadata
    
- Authentication details
    
- Correlation IDs
    
- Request logging
    

---

# 6. Session Scope

## Definition

One bean per **user session**.

Shared across multiple requests from the same user.

Destroyed when the session expires.

---

## Example Uses

- Shopping cart
    
- User preferences
    
- Session cache
    

---

## Warning

Consumes more memory than singleton.

Avoid storing large objects.

---

# 7. Application Scope

## Definition

One bean per **ServletContext**.

Shared across the entire web application.

Similar to singleton but tied specifically to the web context.

Rarely used.

---

# 8. WebSocket Scope

## Definition

One bean per WebSocket connection.

Used in real-time apps like:

- Chat systems
    
- Live dashboards
    
- Multiplayer games
    

Not common in traditional REST systems.

---

# 9. Scope Comparison Table

| Scope       | Instances            | Lifecycle           | Typical Usage          |     |
| ----------- | -------------------- | ------------------- | ---------------------- | --- |
| Singleton   | One per container    | App lifetime        | Services, repositories |     |
| Prototype   | New every request    | Not fully managed   | Stateful objects       |     |
| Request     | One per HTTP request | Request duration    | Request data           |     |
| Session     | One per user         | Session lifetime    | User state             |     |
| Application | One per web app      | App lifetime        | Shared web resources   |     |
| WebSocket   | One per connection   | Connection lifetime | Real-time apps         |     |

---

# 10. How to Define Scope

### Using Annotation

```java
@Scope("prototype")
```

### Recommended Safer Version

```java
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
```

Prevents string typos.

---

# 11. Performance Perspective

### Singleton

- Low memory
    
- Fast
    
- Highly scalable
    

### Prototype

- Higher GC pressure
    
- More allocations
    
- Use carefully
    

Overusing prototype harms performance.

---

# 12. Design Guidelines (Very Important)

### Default Strategy:

> Start with singleton.

Change scope only when required.

---

### Keep Singleton Stateless

Avoid mutable shared fields.

---

### Avoid Prototype for Heavy Objects

Example:

- Database clients
    
- Thread pools
    

These should never be recreated repeatedly.

---

# 13. Common Developer Mistakes

### Making Singleton Stateful

Leads to concurrency bugs.

---

### Overusing Prototype

Creates memory pressure.

---

### Forgetting Lifecycle Responsibility

Spring does not destroy prototype beans.

---

### Storing Large Objects in Session Scope

Can crash servers under load.

---

# 14. High-Probability Interview Questions

### What is the default scope?

Singleton.

---

### Which scope creates a new bean every time?

Prototype.

---

### Does Spring manage prototype destruction?

No.

---

### Which scope is best for stateless services?

Singleton.

---

### Biggest risk with singleton?

Thread safety.

---

# Memory Shortcut

```
Singleton → Shared
Prototype → Fresh
Request → Per API call
Session → Per user
```

---

# Final Takeaway

Bean scope is fundamentally about **object lifecycle strategy**.

Professional Spring systems follow a simple rule:

> Prefer singleton. Use other scopes only when architecture demands it.

Mastering scopes signals that you understand:

- Container behavior
    
- Concurrency considerations
    
- Resource management
    
- Backend scalability