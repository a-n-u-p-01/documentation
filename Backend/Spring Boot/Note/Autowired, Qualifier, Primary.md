These annotations control **how Spring injects dependencies** when multiple beans are available.  
Understanding them is critical because ambiguity in dependency injection is a very common real-world issue.

---

# 1. @Autowired

## Definition

`@Autowired` tells Spring to **automatically inject a dependency** into a class.

Spring resolves the dependency primarily **by type**.

---

## Example

```java
@Service
public class OrderService {

    @Autowired
    private PaymentService paymentService;
}
```

Spring searches for a bean of type `PaymentService` and injects it.

---

## Where Can @Autowired Be Used?

### Constructor Injection (Recommended)

```java
@Service
public class UserService {

    private final UserRepository repository;

    @Autowired   // Optional if single constructor
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Important:

If a class has **only one constructor**, Spring automatically injects it even without `@Autowired`.

Modern best practice often omits it.

---

### Setter Injection

```java
@Autowired
public void setPaymentService(PaymentService paymentService) {
    this.paymentService = paymentService;
}
```

Used when dependency is optional or changeable.

---

### Field Injection (Not Recommended)

```java
@Autowired
private PaymentService paymentService;
```

Why avoid it?

- Hard to unit test
    
- Breaks immutability
    
- Uses reflection
    
- Encourages poor design
    

**Industry standard:** Prefer constructor injection.

---

## How Spring Resolves Dependencies

Spring follows this order:

1. Resolve by **type**
    
2. If multiple beans exist → resolve by **name**
    
3. If still ambiguous → throw exception
    

Example exception:

```
NoUniqueBeanDefinitionException
```

This happens when Spring finds more than one bean of the same type.

That is where `@Qualifier` and `@Primary` become important.

---

# 2. @Qualifier

## Problem Scenario

```java
@Component
class StripePayment implements PaymentService {}

@Component
class PaypalPayment implements PaymentService {}
```

Now Spring sees **two beans** of type `PaymentService`.

Injection fails.

---

## Solution: @Qualifier

`@Qualifier` tells Spring **exactly which bean to inject.**

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(@Qualifier("stripePayment") 
                        PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Spring injects the bean named `"stripePayment"`.

---

## How Bean Names Are Created

Default naming:

```
Class name → camelCase
StripePayment → stripePayment
```

You can customize it:

```java
@Component("stripe")
class StripePayment implements PaymentService {}
```

Then:

```java
@Qualifier("stripe")
```

---

## When to Use @Qualifier

Use it when:

- Multiple implementations exist
    
- You need precise control
    
- Strategy pattern is used
    
- Different environments require different beans
    

Example:

- Multiple payment gateways
    
- Multiple notification providers
    
- Multiple cache strategies
    

---

# 3. @Primary

## Definition

`@Primary` marks one bean as the **default choice** when multiple beans of the same type exist.

---

## Example

```java
@Component
@Primary
class StripePayment implements PaymentService {}

@Component
class PaypalPayment implements PaymentService {}
```

Now Spring automatically injects `StripePayment`.

No qualifier needed.

---

## How Spring Decides

If multiple beans exist:

1. Check for `@Primary`
    
2. If one primary exists → inject it
    
3. If multiple primaries exist → error
    
4. If no primary → require `@Qualifier`
    

---

## When to Use @Primary

Use it when:

- One implementation is used most of the time
    
- You want a safe default
    
- You want cleaner injection without qualifiers everywhere
    

Typical example:

- Default payment gateway
    
- Default cache provider
    
- Default email service
    

---

# @Primary vs @Qualifier

|Feature|@Primary|@Qualifier|
|---|---|---|
|Purpose|Default bean|Specific bean|
|Control Level|Global|Local|
|Usage|On bean class|At injection point|
|Overrides|Can be overridden by qualifier|Always wins|

### Important Rule:

If both are present → **@Qualifier takes precedence.**

---

# Example Combining Both

```java
@Component
@Primary
class StripePayment implements PaymentService {}

@Component
class PaypalPayment implements PaymentService {}
```

Default injection → Stripe.

But you can override:

```java
public OrderService(@Qualifier("paypalPayment") 
                    PaymentService paymentService) {}
```

Now Paypal is injected despite Stripe being primary.

---

# Common Interview Questions

## What does @Autowired do?

Automatically injects dependencies by type from the Spring container.

---

## Is @Autowired required on constructors?

No — if there is only one constructor.

---

## What problem does @Qualifier solve?

It removes ambiguity when multiple beans of the same type exist.

---

## When should you use @Primary?

When one bean should act as the default implementation.

---

## Which is stronger — @Primary or @Qualifier?

`@Qualifier` overrides `@Primary`.

---

# Common Developer Mistakes

### Using Field Injection

Leads to poor testability and hidden dependencies.

---

### Creating Beans with `new`

Bypasses Spring and breaks DI.

Wrong:

```java
PaymentService service = new StripePayment();
```

---

### Overusing @Qualifier

If one implementation dominates, prefer `@Primary`.

---

### Multiple @Primary Beans

This causes startup failure.

---

# Best Practices

- Prefer constructor injection.
    
- Use `@Primary` for default behavior.
    
- Use `@Qualifier` for precise selection.
    
- Avoid field injection.
    
- Design using interfaces, not concrete classes.
    

---

# Internal Insight (Advanced but Valuable)

Spring resolves dependencies during **context initialization**.

Steps:

1. Scan components
    
2. Register beans
    
3. Detect injection points
    
4. Resolve dependencies
    
5. Inject beans
    

If ambiguity exists and is unresolved → application fails fast at startup.

This is intentional and prevents runtime surprises.

---

# Quick Memory Summary

**@Autowired** → Inject dependency automatically  
**@Qualifier** → Choose a specific bean  
**@Primary** → Set a default bean

### Golden Rule:

> Use constructor injection + @Primary for defaults + @Qualifier when precision is required.

---

## Final Takeaway

Mastering these three annotations means you understand **how Spring resolves dependencies**, which is a core skill for building scalable backend systems.

Developers who understand this well typically write:

- Loosely coupled services
    
- Easily testable code
    
- Clean architecture
    
- Production-ready applications