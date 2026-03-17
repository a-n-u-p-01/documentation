# 1. Introduction

Spring Cloud Gateway is a lightweight API Gateway built on Spring WebFlux.

It is used in microservices architecture to:

- Route external requests to internal services
    
- Apply filters (authentication, logging, rate limiting)
    
- Centralize cross-cutting concerns
    
- Hide internal service structure
    

It acts as the single entry point for client requests.

---

# 2. Why API Gateway Is Needed

Without API Gateway:

Client → Calls each microservice directly  
Problems:

- Security configuration duplicated
    
- No centralized authentication
    
- Hard to manage rate limiting
    
- Hard to log and monitor
    
- Exposes internal services publicly
    

With API Gateway:

Client → Gateway → Internal Services

Only Gateway is exposed publicly.

---

# 3. Architecture

Internet  
↓  
Spring Cloud Gateway  
↓  
order-service  
user-service  
payment-service

Gateway handles:

- Routing
    
- Security
    
- Logging
    
- Transformation
    
- Filtering
    

---

# 4. Adding Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

Spring Cloud Gateway is reactive (WebFlux-based).

---

# 5. Routing

Routing determines where a request should go.

Example:

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: order-route
          uri: http://order-service
          predicates:
            - Path=/orders/**
```

Meaning:

If request path matches:

```
/orders/**
```

Forward to:

```
http://order-service
```

If using Eureka:

```
uri: lb://ORDER-SERVICE
```

`lb://` means use service discovery.

---

# 6. Predicates

Predicates define routing conditions.

Common predicates:

- Path
    
- Method
    
- Header
    
- Host
    
- Query
    

Example:

```yaml
predicates:
  - Path=/orders/**
  - Method=GET
```

Route applies only to GET requests on /orders.

---

# 7. Filters

Filters modify requests or responses.

Two types:

1. Pre-filters (before request is sent)
    
2. Post-filters (after response is received)
    

---

## Built-in Filters Examples

Add header:

```yaml
filters:
  - AddRequestHeader=X-Request-Source, Gateway
```

Rewrite path:

```yaml
filters:
  - RewritePath=/orders/(?<segment>.*), /${segment}
```

Strip prefix:

```yaml
filters:
  - StripPrefix=1
```

---

# 8. Custom Filter

Create custom filter:

```java
@Component
public class LoggingFilter implements GlobalFilter {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("Incoming request: " + exchange.getRequest().getURI());
        return chain.filter(exchange);
    }
}
```

GlobalFilter applies to all routes.

---

# 9. Integration with Service Discovery

With Eureka:

```yaml
uri: lb://ORDER-SERVICE
```

Gateway:

1. Queries registry
    
2. Gets instance list
    
3. Load balances request
    
4. Forwards to selected instance
    

No hardcoded IP required.

---

# 10. Common Production Use Cases

Authentication (JWT validation)  
Rate limiting  
Request logging  
CORS handling  
Path rewriting  
API versioning  
Circuit breaking  
Header manipulation

Gateway centralizes all of these.

---

# 11. Security Best Practice

- Expose only Gateway publicly.
    
- Keep internal services as private (ClusterIP).
    
- Apply authentication at Gateway.
    
- Validate JWT before forwarding request.
    

---

# 12. Difference: Gateway vs Load Balancer

Load Balancer:

- Distributes traffic
    
- Infrastructure-level
    

API Gateway:

- Routes requests
    
- Applies filters
    
- Handles authentication
    
- Application-level logic
    

Gateway can use load balancer internally.

---

# 13. Real Production Flow

User  
↓  
DNS  
↓  
Load Balancer  
↓  
Spring Cloud Gateway  
↓  
order-service (ClusterIP)

Internal services never exposed.

---

# 14. Advantages

- Centralized routing
    
- Reduced duplication
    
- Improved security
    
- Better observability
    
- Scalable architecture
    

---

# Interview Questions and Answers

1. What is Spring Cloud Gateway?  
    It is a reactive API Gateway used to route requests and apply filters in microservices architecture.
    
2. What is the difference between predicates and filters?  
    Predicates define routing conditions. Filters modify request or response.
    
3. What does lb:// mean in Gateway?  
    It indicates load-balanced routing using service discovery.
    
4. Why is API Gateway important?  
    It centralizes authentication, logging, routing, and cross-cutting concerns.
    
5. Should internal services be exposed publicly?  
    No. Only the Gateway should be exposed.
    
6. Is Spring Cloud Gateway blocking or non-blocking?  
    It is reactive and non-blocking (WebFlux-based).
    

---
