# 1. Introduction

Circuit Breaker is a resilience pattern used in microservices to prevent cascading failures when a dependent service is down or slow.

In distributed systems:

- Services depend on each other.
    
- If one service fails, others may keep retrying.
    
- This can overload the system and cause total failure.
    

Circuit Breaker protects the system by stopping calls to failing services temporarily.

Spring Boot integrates Circuit Breaker using **Resilience4j**.

---

# 2. Why Circuit Breaker Is Needed

Example:

user-service → calls → order-service  
order-service is down.

Without Circuit Breaker:

- user-service keeps calling.
    
- Threads get blocked.
    
- Connection pool gets exhausted.
    
- Entire system slows down.
    

With Circuit Breaker:

- After certain failures,
    
- Calls are stopped immediately.
    
- Fallback response is returned.
    
- System remains stable.
    

---

# 3. Circuit Breaker States

There are three states:

### 1. Closed (Normal State)

- Requests flow normally.
    
- Failures are monitored.
    

### 2. Open (Failure State)

- Too many failures detected.
    
- All calls are blocked.
    
- Fallback method executed.
    
- No real call is made.
    

### 3. Half-Open (Testing State)

- After wait time expires,
    
- A few test requests are allowed.
    
- If successful → back to Closed.
    
- If failed → back to Open.
    

---

# 4. Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
```

---

# 5. Basic Example

```java
@CircuitBreaker(name = "orderService", fallbackMethod = "fallbackOrder")
public Order getOrder(Long id) {
    return orderClient.getOrderById(id);
}
```

Fallback method:

```java
public Order fallbackOrder(Long id, Throwable ex) {
    return new Order("Default Order", 0.0);
}
```

If order-service fails:  
Fallback method is executed.

---

# 6. Configuration

application.properties:

```properties
resilience4j.circuitbreaker.instances.orderService.failure-rate-threshold=50
resilience4j.circuitbreaker.instances.orderService.minimum-number-of-calls=5
resilience4j.circuitbreaker.instances.orderService.wait-duration-in-open-state=10s
resilience4j.circuitbreaker.instances.orderService.sliding-window-size=10
```

Important parameters:

- failure-rate-threshold → percentage of failures to open circuit
    
- minimum-number-of-calls → minimum calls before calculation
    
- wait-duration-in-open-state → how long to stay open
    
- sliding-window-size → number of calls tracked
    

---

# 7. Circuit Breaker with Feign

Feign + Resilience4j works together.

If Feign call fails:  
Circuit breaker monitors failures.  
Fallback method returns safe response.

Prevents cascading failure across services.

---

# 8. Difference: Retry vs Circuit Breaker

Retry:

- Tries request again immediately.
    

Circuit Breaker:

- Stops calling completely after threshold.
    

Retry is good for temporary network glitch.  
Circuit breaker is good for service downtime.

Often used together.

---

# 9. Real-World Example

E-commerce system:

user-service → order-service → payment-service

If payment-service fails:

Circuit breaker opens.  
order-service returns fallback:  
"Payment service temporarily unavailable."

System does not crash.

---

# 10. Integration in Kubernetes

Even in Kubernetes:

- Pods may crash.
    
- Network delays may occur.
    
- Service may be slow.
    

Circuit breaker still needed because:

Kubernetes handles infrastructure failures.  
Circuit breaker handles application-level failures.

They solve different problems.

---

# 11. Monitoring Circuit Breaker

Using Actuator:

```properties
management.endpoints.web.exposure.include=health
```

Health endpoint shows:

- Circuit state
    
- Failure rate
    

Can integrate with Prometheus/Grafana.

---

# 12. Best Practices

- Always define fallback.
    
- Do not hide serious business errors silently.
    
- Tune thresholds carefully.
    
- Combine with timeout configuration.
    
- Use with Retry and Rate Limiter.
    

---

# 13. Advantages

- Prevents cascading failure.
    
- Improves system stability.
    
- Faster failure response.
    
- Protects threads and resources.
    

---

# Interview Questions and Answers

1. What problem does Circuit Breaker solve?  
    It prevents cascading failures in distributed systems by stopping calls to failing services.
    
2. What are the three states of Circuit Breaker?  
    Closed, Open, and Half-Open.
    
3. How is Circuit Breaker different from Retry?  
    Retry attempts the request again; Circuit Breaker stops calling after failure threshold.
    
4. Does Kubernetes replace Circuit Breaker?  
    No. Kubernetes handles infrastructure-level failures; Circuit Breaker handles application-level failures.
    
5. What happens in Open state?  
    All calls are blocked and fallback method is executed.
    
6. Why is fallback important?  
    It provides a safe alternative response when the dependent service is unavailable.
    

---
