# 1. Introduction

Feign Client is a declarative HTTP client provided by Spring Cloud.

It allows microservices to call other microservices using simple Java interfaces instead of manually writing REST calls.

Instead of using:

- RestTemplate
    
- WebClient
    

You define an interface and Feign handles the implementation automatically.

---

# 2. Why Feign Is Needed

Without Feign (using RestTemplate):

```java
RestTemplate restTemplate = new RestTemplate();
Order order = restTemplate.getForObject(
    "http://order-service/orders/1",
    Order.class
);
```

Problems:

- Hardcoded URLs
    
- Manual error handling
    
- Boilerplate code
    
- Harder to maintain
    

With Feign:

You just define an interface.

---

# 3. Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

Enable Feign:

```java
@EnableFeignClients
@SpringBootApplication
public class Application {
}
```

---

# 4. Basic Feign Client Example

```java
@FeignClient(name = "order-service")
public interface OrderClient {

    @GetMapping("/orders/{id}")
    Order getOrderById(@PathVariable Long id);
}
```

Usage:

```java
@Autowired
private OrderClient orderClient;

Order order = orderClient.getOrderById(1L);
```

Feign automatically:

- Resolves service name
    
- Performs HTTP call
    
- Converts response to object
    

---

# 5. How It Works Internally

When you call:

```java
orderClient.getOrderById(1L);
```

Feign:

1. Builds HTTP request
    
2. Resolves service name (via Eureka or Kubernetes DNS)
    
3. Sends request
    
4. Converts JSON response to Java object
    
5. Returns result
    

You never see the HTTP logic.

---

# 6. Feign with Service Discovery

With Eureka:

```java
@FeignClient(name = "ORDER-SERVICE")
```

Feign:

- Queries registry
    
- Gets instance list
    
- Load balances call
    

With Kubernetes:

```java
@FeignClient(name = "order-service")
```

Feign:

- Uses internal DNS
    
- Calls [http://order-service](http://order-service/)
    

No Eureka needed.

---

# 7. Load Balancing with Feign

Feign integrates with Spring Cloud LoadBalancer.

If multiple instances exist:

- Instance 1
    
- Instance 2
    
- Instance 3
    

Requests are distributed automatically.

Default strategy:  
Round Robin.

---

# 8. Custom Configuration

You can customize:

- Timeout
    
- Retry logic
    
- Error handling
    
- Interceptors
    

Example timeout config:

```properties
spring.cloud.openfeign.client.config.default.connectTimeout=5000
spring.cloud.openfeign.client.config.default.readTimeout=5000
```

---

# 9. Feign Interceptor (Add Headers Automatically)

Example:

```java
@Component
public class AuthInterceptor implements RequestInterceptor {

    @Override
    public void apply(RequestTemplate template) {
        template.header("Authorization", "Bearer token");
    }
}
```

Used for:

- JWT forwarding
    
- Correlation IDs
    
- Logging
    

---

# 10. Error Handling

Custom error decoder:

```java
@Bean
public ErrorDecoder errorDecoder() {
    return new CustomErrorDecoder();
}
```

Allows:

- Mapping HTTP errors to custom exceptions.
    

---

# 11. Feign + Circuit Breaker (Resilience)

Feign can integrate with Resilience4j.

If service fails:

- Fallback method can be defined.
    

Example:

```java
@FeignClient(name = "order-service", fallback = OrderFallback.class)
```

Improves fault tolerance.

---

# 12. Advantages

- Declarative style
    
- Clean code
    
- Less boilerplate
    
- Built-in load balancing
    
- Service discovery integration
    
- Easy error handling
    

---

# 13. When to Use Feign

Use Feign when:

- Calling internal microservices
    
- Using Spring Cloud ecosystem
    
- Want simple declarative HTTP calls
    

Do not use Feign for:

- High-performance reactive streaming (use WebClient)
    

---

# 14. Real-World Flow in Kubernetes

user-service  
↓  
Feign Client  
↓  
[http://order-service](http://order-service/)  
↓  
Kubernetes Service  
↓  
order pod

No hardcoded IP.  
No manual load balancing.

---

# Interview Questions and Answers

1. What is Feign Client?  
    Feign is a declarative HTTP client that simplifies service-to-service communication in microservices.
    
2. How does Feign work with service discovery?  
    It resolves service names using Eureka or Kubernetes DNS and performs load-balanced calls.
    
3. What is the advantage of Feign over RestTemplate?  
    Less boilerplate, declarative approach, built-in load balancing.
    
4. Can Feign handle multiple service instances?  
    Yes, through Spring Cloud LoadBalancer.
    
5. How do you add headers in Feign automatically?  
    Using a RequestInterceptor.
    
6. Is Feign reactive?  
    No. Feign is blocking. For reactive systems, use WebClient.
    

---
