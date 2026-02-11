Both **Filters** and **Interceptors** are used to process requests **before they reach the controller** and **after the response is generated**.

They are essential for cross-cutting concerns like:

- Logging
    
- Authentication
    
- Rate limiting
    
- Request tracking
    
- Security checks
    

Understanding the difference is a **very popular interview topic**.

---

# 1. Where They Sit in the Request Flow

Visualizing the pipeline makes this topic easy:

```
Client
 ↓
Filters
 ↓
DispatcherServlet
 ↓
Interceptors
 ↓
Controller
 ↓
Service
 ↓
Response
```

👉 Filters run earlier.  
👉 Interceptors run closer to the controller.

This is the biggest conceptual difference.

---

# 2. Filters — Servlet-Level Processing

## What is a Filter?

A Filter is part of the **Servlet specification**, not Spring itself.

It operates at the **web container level**.

Meaning it can process **every HTTP request** — even before Spring is involved.

---

## Common Uses

Filters are ideal for **generic web concerns**:

### Authentication checks

### CORS handling

### Compression

### Logging

### Encoding (UTF-8)

### Security layers

Spring Security itself heavily relies on filters.

---

## How a Filter Works

It intercepts requests using a chain:

```
Request → Filter → Next Filter → Target Resource
```

Each filter decides whether the request should proceed.

---

## Example Filter

```java
@Component
public class LoggingFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request,
                         ServletResponse response,
                         FilterChain chain)
            throws IOException, ServletException {

        System.out.println("Request received");

        chain.doFilter(request, response); // continue flow

        System.out.println("Response sent");
    }
}
```

### Critical Line:

```
chain.doFilter()
```

If you forget it → request stops.

Major beginner mistake.

---

## Characteristics of Filters

- Work on raw HTTP requests
    
- Not Spring-specific
    
- Can modify request/response
    
- Execute before DispatcherServlet
    
- Apply to all routes by default
    

Think of them as **global guards**.

---

# 3. Interceptors — Spring MVC Level

## What is an Interceptor?

An Interceptor is a **Spring MVC feature** that intercepts requests **after they enter the Spring ecosystem but before hitting the controller.**

More Spring-aware than filters.

---

## Typical Use Cases

Perfect when logic is related to:

- Controllers
    
- Business flow
    
- Authorization
    
- Request timing
    
- Audit trails
    

Less ideal for low-level HTTP manipulation.

---

## Lifecycle Methods

Interceptors provide three hooks.

### preHandle()

Runs BEFORE controller execution.

Return `true` → continue  
Return `false` → stop request

```java
public boolean preHandle(...) {
    return true;
}
```

---

### postHandle()

Runs AFTER controller but BEFORE response is sent.

Great for modifying response data.

---

### afterCompletion()

Runs AFTER the entire request finishes.

Used for:

- Logging
    
- Cleanup
    
- Metrics
    

---

## Example Interceptor

```java
@Component
public class AuthInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler) {

        System.out.println("Checking auth...");
        return true;
    }
}
```

---

## Registering an Interceptor

Unlike filters, interceptors must be registered.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new AuthInterceptor());
    }
}
```

You can also restrict paths.

---

## Path Targeting Example

```java
registry.addInterceptor(new AuthInterceptor())
        .addPathPatterns("/api/**")
        .excludePathPatterns("/login");
```

Very useful in real systems.

---

# 4. Key Differences (Interview Favorite)

|Feature|Filter|Interceptor|
|---|---|---|
|Level|Servlet|Spring MVC|
|Runs Before|DispatcherServlet|Controller|
|Spring Aware|No|Yes|
|Modify Request|Yes|Limited|
|Path Control|Harder|Easier|
|Typical Usage|Security, encoding|Auth, logging|
|Configuration|Web container|Spring config|

---

# 5. When to Use Which?

## Use Filters When:

- Working with raw HTTP
    
- Building security layers
    
- Implementing CORS
    
- Performing request wrapping
    
- Needing global coverage
    

👉 Think infrastructure-level.

---

## Use Interceptors When:

- Logic relates to controllers
    
- You need handler info
    
- Path-based execution matters
    
- Implementing audit logs
    
- Measuring request time
    

👉 Think application-level.

---

# 6. Execution Order (Important)

```
Filter → Interceptor → Controller → Interceptor → Filter
```

Filters wrap the entire request lifecycle.

Interceptors wrap controller execution.

Interviewers love this.

---

# 7. Performance Perspective

Both are lightweight.

But:

👉 Too many filters can slow every request.  
👉 Too many interceptors clutter controller flow.

Use only when necessary.

---

# 8. Common Developer Mistakes

### Using Interceptor for Security Instead of Spring Security

Reinventing the wheel is dangerous.

---

### Forgetting chain.doFilter()

Stops request processing.

---

### Putting Business Logic Inside Filters

Filters should stay generic.

---

### Overusing Interceptors

Not every feature needs interception.

---

# 9. Advanced Insight (High Value)

## Why Spring Security Uses Filters

Security must run **before Spring MVC**.

Otherwise unauthorized requests could reach controllers.

Hence:

👉 Security lives in the filter chain.

This is a strong interview statement.

---

# 10. High-Probability Interview Questions

### Difference between Filter and Interceptor?

Filter operates at the servlet level; interceptor operates at the Spring MVC level.

---

### Which runs first?

Filter.

---

### Which is Spring-specific?

Interceptor.

---

### Which is better for authentication?

Typically filters (especially via Spring Security).

---

### Can interceptors modify requests?

Not as deeply as filters.

---

# Quick Memory Rule

```
Filter → Infrastructure
Interceptor → Application
```

Or even simpler:

> Filters guard the gate.  
> Interceptors watch the controller.

---

# Final Takeaway

Knowing when to use filters vs interceptors signals **architectural maturity**.

It shows you understand:

- Request lifecycle
    
- Layered processing
    
- Security placement
    
- Framework internals
    

### Professional Guideline:

> Use filters for low-level concerns, interceptors for controller-aware logic, and avoid mixing responsibilities.