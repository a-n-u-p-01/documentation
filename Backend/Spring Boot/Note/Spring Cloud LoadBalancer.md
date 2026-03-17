# 1. Introduction

Spring Cloud LoadBalancer is a client-side load balancing library used in Spring Cloud applications.

It distributes requests across multiple instances of a service.

It replaces Netflix Ribbon (which is now deprecated).

It works with:

- Eureka
    
- Consul
    
- Kubernetes
    
- Static service lists
    

---

# 2. Why Load Balancer Is Needed

In microservices:

order-service has:

- Instance 1
    
- Instance 2
    
- Instance 3
    

When another service calls order-service:

Request must be distributed among instances.

Without load balancing:

- All traffic may go to one instance
    
- Performance bottleneck occurs
    
- Risk of failure increases
    

LoadBalancer distributes traffic evenly.

---

# 3. Client-Side vs Server-Side Load Balancing

Client-Side Load Balancing:

- Client fetches list of service instances.
    
- Client selects instance.
    
- Client sends request.
    

Example:  
Spring Cloud LoadBalancer with Eureka.

Server-Side Load Balancing:

- Client sends request to load balancer.
    
- Load balancer selects instance.
    

Example:  
Kubernetes Service.

---

# 4. How Spring Cloud LoadBalancer Works

Step 1:  
Client requests service instance list from DiscoveryClient.

Step 2:  
LoadBalancer selects one instance using algorithm.

Step 3:  
Request is sent to selected instance.

Default algorithm:  
Round Robin.

---

# 5. Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>
```

Usually included automatically with:

- spring-cloud-starter-openfeign
    
- spring-cloud-starter-gateway
    

---

# 6. Using with RestTemplate

```java
@Bean
@LoadBalanced
public RestTemplate restTemplate() {
    return new RestTemplate();
}
```

Call service:

```java
restTemplate.getForObject("http://order-service/orders", String.class);
```

Spring automatically:

- Resolves service name
    
- Load balances request
    

---

# 7. Using with Feign

Feign automatically integrates with Spring Cloud LoadBalancer.

Example:

```java
@FeignClient(name = "order-service")
public interface OrderClient {
    @GetMapping("/orders/{id}")
    Order getOrder(@PathVariable Long id);
}
```

If multiple instances exist:  
LoadBalancer distributes calls.

---

# 8. Load Balancing Algorithms

Default:  
Round Robin

Other possible strategies:

- Random
    
- Custom selection logic
    

Custom example:

```java
@Bean
public ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(
        Environment environment,
        LoadBalancerClientFactory factory) {
    String serviceId = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);
    return new RandomLoadBalancer(
            factory.getLazyProvider(serviceId, ServiceInstanceListSupplier.class),
            serviceId);
}
```

---

# 9. How It Works with Eureka

Flow:

Feign / RestTemplate  
↓  
Spring Cloud LoadBalancer  
↓  
Eureka → Get instance list  
↓  
Select instance  
↓  
Send request

---

# 10. How It Works in Kubernetes

In Kubernetes:

If you use:

```
http://order-service
```

Kubernetes Service already performs load balancing.

So Spring Cloud LoadBalancer is often unnecessary.

However, it may still operate if using DiscoveryClient.

Best practice in Kubernetes:  
Let Kubernetes handle load balancing.

---

# 11. Difference: LoadBalancer vs API Gateway

LoadBalancer:  
Distributes traffic among instances of one service.

API Gateway:  
Routes traffic to different services and applies filters.

LoadBalancer is internal.  
Gateway is external entry point.

---

# 12. Advantages

- Automatic distribution of traffic
    
- No hardcoded IPs
    
- Works with service discovery
    
- Reduces bottlenecks
    
- Improves availability
    

---

# 13. When to Use

Use Spring Cloud LoadBalancer when:

- Using Eureka or Consul
    
- Running microservices outside Kubernetes
    
- Using Feign or RestTemplate
    

Do not rely on it when:  
Kubernetes Service already provides load balancing.

---

# Interview Questions and Answers

1. What is Spring Cloud LoadBalancer?  
    It is a client-side load balancing library that distributes requests across service instances.
    
2. What replaced Netflix Ribbon?  
    Spring Cloud LoadBalancer.
    
3. What is the default load balancing strategy?  
    Round Robin.
    
4. How does it work with Feign?  
    Feign integrates automatically and uses it to distribute requests.
    
5. Do we need it in Kubernetes?  
    Usually no, because Kubernetes Service provides server-side load balancing.
    
6. What is the difference between client-side and server-side load balancing?  
    Client-side selects instance before sending request. Server-side load balancer selects instance after receiving request.
    

---
