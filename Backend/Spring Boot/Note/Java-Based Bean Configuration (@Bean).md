## 1. What is `@Bean`?

`@Bean` is used inside a **configuration class** to explicitly define an object that Spring should manage as a bean.

It is part of **Java-based configuration**, which replaces older XML configuration.

Instead of relying on component scanning, you manually instruct Spring:

> “Create this object and register it in the container.”

---

## 2. When Should You Use `@Bean`?

Use `@Bean` when you **cannot annotate the class directly**.

### Most Common Cases

### Third-Party Libraries

You cannot modify their source code.

Example:

```java
@Configuration
public class AppConfig {

    @Bean
    public ObjectMapper objectMapper() {
        return new ObjectMapper();
    }
}
```

You just registered a library class as a Spring bean.

---

### External Clients

Very common in production:

- HTTP clients
    
- Kafka producers
    
- Redis clients
    
- Payment SDKs
    

```java
@Bean
public RestTemplate restTemplate() {
    return new RestTemplate();
}
```

---

### Custom Object Creation Logic

When bean creation needs:

- Conditional logic
    
- Parameters
    
- Complex setup
    
- Factory methods
    

```java
@Bean
public ExecutorService executorService() {
    return Executors.newFixedThreadPool(10);
}
```

---

## 3. `@Configuration` — Required Companion

`@Bean` methods must live inside a class annotated with:

```java
@Configuration
```

Example:

```java
@Configuration
public class AppConfig {

    @Bean
    public EmailService emailService() {
        return new EmailService();
    }
}
```

### What `@Configuration` Does Internally

Spring enhances this class using **CGLIB proxying** to ensure:

- Beans remain singletons
    
- Duplicate creation is prevented
    
- Inter-bean calls return the managed instance
    

Without it, behavior changes (explained below).

---

## 4. Bean Naming

By default, the bean name is the **method name**.

```java
@Bean
public CacheManager cacheManager() {
    return new CacheManager();
}
```

Bean name → `"cacheManager"`

### Custom Name

```java
@Bean("redisCacheManager")
public CacheManager cacheManager() {
    return new CacheManager();
}
```

Useful when multiple beans of the same type exist.

---

## 5. Dependency Injection Between `@Bean` Methods

Spring automatically resolves dependencies between beans.

```java
@Bean
public Engine engine() {
    return new Engine();
}

@Bean
public Car car() {
    return new Car(engine());
}
```

Even though `engine()` is called manually, Spring returns the **same managed bean**, not a new object.

This is because of the proxy created by `@Configuration`.

---

## 6. Important Difference: `@Configuration` vs `@Component`

This is a favorite interview question.

### With `@Configuration`

```java
@Configuration
class AppConfig {

    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Car car() {
        return new Car(engine());
    }
}
```

Result:  
Only **ONE Engine instance** exists.

---

### With `@Component` (Wrong for configs)

```java
@Component
class AppConfig {
}
```

Now each method call creates a **new object**.

Singleton guarantee breaks.

### Rule:

> Always use `@Configuration` for bean definition classes.

---

## 7. Singleton Behavior

Beans created via `@Bean` are singleton by default.

You can change scope:

```java
@Bean
@Scope("prototype")
public Engine engine() {
    return new Engine();
}
```

(Prototype = new object every time)

Used rarely — only when stateless behavior is required.

---

## 8. Lifecycle Control

You can hook into initialization and cleanup.

```java
@Bean(initMethod = "start", destroyMethod = "stop")
public Server server() {
    return new Server();
}
```

Spring calls:

- `start()` after bean creation
    
- `stop()` before shutdown
    

Useful for:

- Connection pools
    
- Messaging clients
    
- Thread pools
    

---

## 9. Conditional Bean Creation (Production-Level)

Spring can create beans only when conditions match.

Example:

```java
@Bean
@ConditionalOnProperty(name = "cache.enabled", havingValue = "true")
public CacheManager cacheManager() {
    return new CacheManager();
}
```

Used heavily in:

- Auto-configuration
    
- Feature toggles
    
- Environment-specific setups
    

This is how Spring Boot internally works.

---

## 10. `@Bean` vs `@Component` — When to Use Which?

|Use Case|Choose|
|---|---|
|You control the class|`@Component`|
|Third-party class|`@Bean`|
|Complex creation logic|`@Bean`|
|Simple service|`@Component`|
|External client|`@Bean`|

### Industry Pattern:

Most application code → annotations  
Most infrastructure code → `@Bean`

---

## 11. Proxy Behavior (Advanced Insight)

`@Configuration` uses runtime subclassing to intercept method calls.

This ensures:

```
engine() → always returns the container-managed bean
```

Instead of:

```
engine() → new Engine()
```

This mechanism preserves singleton consistency across the app.

You are indirectly benefiting from **method interception**.

---

## 12. Common Mistakes

### Using `@Component` Instead of `@Configuration`

Breaks singleton guarantees.

---

### Creating Beans Manually with `new`

Bypasses Spring.

Always let the container manage shared objects.

---

### Overusing `@Bean`

If a class can be annotated directly — do that.

Reserve `@Bean` for infrastructure.

---

### Forgetting Lifecycle Hooks

Critical for resources like thread pools.

Can cause memory leaks.

---

## 13. High-Value Interview Questions

### What is `@Bean`?

A method-level annotation used to register objects as Spring-managed beans.

---

### Why not always use `@Component`?

Because third-party classes cannot be annotated.

---

### Why must `@Bean` be inside `@Configuration`?

To ensure proxying and singleton enforcement.

---

### What happens if `@Configuration` is removed?

Spring treats it like a normal class → multiple instances may be created.

---

### Is `@Bean` singleton?

Yes, by default.

---

## Quick Memory Summary

**`@Bean` = Explicit bean creation**  
**`@Configuration` = Ensures container-managed lifecycle**

Use it for:

- Third-party libraries
    
- Infrastructure components
    
- Complex initialization
    

Avoid it for simple application services.

---

## Final Takeaway

Understanding `@Bean` separates beginner developers from production-ready engineers.

It shows you understand:

- Container behavior
    
- Bean lifecycle
    
- Infrastructure configuration
    
- How Spring Boot auto-configuration works internally
    

**Professional Rule:**

> Annotate application classes. Configure infrastructure with `@Bean`.