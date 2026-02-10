The Spring Bean Lifecycle describes **what happens to a bean from creation to destruction** inside the container.

Understanding this topic proves you know **how Spring works internally**, not just how to use annotations.

It is a **frequently asked interview concept** because it connects DI, configuration, and container behavior.

---

# 1. What is Bean Lifecycle?

Bean lifecycle is the sequence of steps Spring performs:

> Instantiate → Configure → Initialize → Use → Destroy

Every Spring-managed bean follows this pipeline.

---

# 2. Lifecycle Flow (Step-by-Step)

## Step 1 — Bean Instantiation

Spring creates the bean object.

Typically done via:

- Constructor
    
- Factory method
    
- `@Bean` method
    

At this point, the object exists but is not ready.

---

## Step 2 — Dependency Injection

Spring injects all required dependencies.

The bean becomes structurally complete but may still need setup.

---

## Step 3 — Aware Interfaces (Optional Advanced Step)

If a bean implements special interfaces, Spring provides container-related metadata.

Examples:

- `BeanNameAware` → receives bean name
    
- `ApplicationContextAware` → receives context
    
- `BeanFactoryAware` → receives factory
    

Used rarely — mostly in framework-level code.

---

## Step 4 — BeanPostProcessor (Before Initialization)

Spring allows modification of beans before initialization.

This is how features like:

- AOP proxies
    
- Security
    
- Transactions
    

are attached.

You typically don’t write these unless building frameworks.

---

## Step 5 — Initialization Phase

Now the bean performs startup logic.

There are **three ways** to define this.

### Option 1 — `@PostConstruct` (Most Common)

```java
@PostConstruct
public void init() {
    System.out.println("Bean initialized");
}
```

Runs once after dependencies are injected.

Typical uses:

- Open connections
    
- Load cache
    
- Validate config
    
- Precompute data
    

---

### Option 2 — InitializingBean

```java
public class MyBean implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        // initialization logic
    }
}
```

Avoid in modern apps — it couples code to Spring.

Prefer `@PostConstruct`.

---

### Option 3 — initMethod (Java Config)

```java
@Bean(initMethod = "start")
public Server server() {
    return new Server();
}
```

Good when configuring third-party classes.

---

## Step 6 — Bean Ready for Use

The bean is now fully initialized and stored in the container.

Controllers, services, and other beans can safely use it.

Most of your application runs during this phase.

---

## Step 7 — Destruction Phase

Triggered when:

- Application shuts down
    
- Context closes
    

Spring calls cleanup methods.

---

### Option 1 — `@PreDestroy`

```java
@PreDestroy
public void cleanup() {
    System.out.println("Releasing resources...");
}
```

Common uses:

- Close DB connections
    
- Stop threads
    
- Flush buffers
    
- Release file handles
    

---

### Option 2 — DisposableBean

```java
public class MyBean implements DisposableBean {

    @Override
    public void destroy() {
    }
}
```

Again, avoid — tightly couples code to Spring.

---

### Option 3 — destroyMethod

```java
@Bean(destroyMethod = "stop")
public Server server() {
    return new Server();
}
```

Best for external libraries.

---

# 3. Full Lifecycle in One View

```
1. Instantiate bean
2. Inject dependencies
3. Set aware interfaces
4. BeanPostProcessor (before)
5. Initialization (@PostConstruct)
6. Bean ready
7. BeanPostProcessor (after)
8. Destroy (@PreDestroy)
```

Memorizing this flow is extremely useful for interviews.

---

# 4. Very Important Scope Behavior

### Singleton Beans

Spring manages the FULL lifecycle:

- Creation
    
- Initialization
    
- Destruction
    

---

### Prototype Beans

Spring manages ONLY:

- Creation
    
- Injection
    

NOT destruction.

You must clean them manually if needed.

This is a favorite interview trap.

---

# 5. Why Lifecycle Matters in Production

Correct lifecycle handling prevents:

- Memory leaks
    
- Connection exhaustion
    
- Zombie threads
    
- Resource locking
    

Example mistake:

Opening a thread pool without shutting it down can crash servers.

---

# 6. BeanPostProcessor — High-Level Understanding

Acts like a **hook** to intercept beans.

Runs:

- Before initialization
    
- After initialization
    

Used internally by Spring to create:

- Proxy objects
    
- Transaction wrappers
    
- Security interceptors
    

You rarely implement it unless building infrastructure.

But knowing it shows advanced understanding.

---

# 7. Common Lifecycle Use Cases

### Initialization

- Validate environment variables
    
- Warm up caches
    
- Preload ML models
    
- Start schedulers
    

### Destruction

- Close sockets
    
- Stop executors
    
- Disconnect messaging clients
    

---

# 8. Common Developer Mistakes

### Putting Heavy Logic in Constructors

Bad practice.

Constructor should only assign values.

Use `@PostConstruct` for real setup.

---

### Forgetting Cleanup

Leads to resource leaks.

Always release:

- Threads
    
- Streams
    
- Connections
    

---

### Using Spring Interfaces Directly

Prefer annotations (`@PostConstruct`, `@PreDestroy`) to keep code framework-agnostic.

---

# 9. High-Probability Interview Questions

### What is the bean lifecycle?

The sequence from bean creation to destruction managed by the container.

---

### When does `@PostConstruct` run?

After dependency injection but before the bean is used.

---

### When does `@PreDestroy` run?

During application shutdown.

---

### Does Spring destroy prototype beans?

No.

---

### Why avoid heavy constructors?

The bean is not fully initialized yet.

---

# Quick Memory Trick

```
Create → Inject → Initialize → Ready → Destroy
```

---

# Final Takeaway

Bean lifecycle knowledge signals **real Spring expertise**.

It shows you understand:

- Container internals
    
- Resource management
    
- Application stability
    
- Production safety
    

### Golden Rule:

> Keep constructors light, initialize properly, and always clean up resources.