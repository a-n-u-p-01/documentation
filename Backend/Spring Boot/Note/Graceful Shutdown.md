Graceful shutdown allows a Spring Boot application to complete ongoing requests before shutting down.

Without graceful shutdown:

- Active requests are abruptly terminated
    
- Clients may receive errors
    
- Data inconsistency may occur
    

With graceful shutdown:

- Stops accepting new requests
    
- Waits for existing requests to complete
    
- Then shuts down safely
    

Important in:

- Kubernetes rolling updates
    
- Microservices
    
- Production systems
    

---

## 2. Enabling Graceful Shutdown

Spring Boot (2.3+)

Enable in application.properties:

```properties
server.shutdown=graceful
```

Configure timeout:

```properties
spring.lifecycle.timeout-per-shutdown-phase=30s
```

Meaning:

- App will wait up to 30 seconds before force shutdown.
    

---

## 3. How It Works Internally

1. Kubernetes sends SIGTERM to container.
    
2. Spring Boot stops accepting new requests.
    
3. Existing requests continue processing.
    
4. After timeout, app shuts down.
    

---

## 4. Kubernetes Integration

In Deployment YAML:

```yaml
terminationGracePeriodSeconds: 30
```

This must be >= Spring Boot shutdown timeout.

Optional preStop hook:

```yaml
lifecycle:
  preStop:
    exec:
      command: ["sleep", "10"]
```

Purpose:  
Give load balancer time to stop sending traffic.

---

## 5. Thread Pool Consideration

If using async tasks or custom thread pools:

Ensure:

- Executor services are properly shutdown
    
- @PreDestroy methods clean resources
    

Example:

```java
@PreDestroy
public void cleanUp() {
    executorService.shutdown();
}
```

---

## 6. Common Problems Without Graceful Shutdown

- 502/503 errors during deployment
    
- Interrupted DB transactions
    
- Incomplete API responses
    
- Data corruption in distributed systems
    

---

## 7. Production Best Practices

- Always enable graceful shutdown
    
- Combine with readiness probe
    
- Ensure terminationGracePeriodSeconds matches timeout
    
- Close DB connections and thread pools properly
    

---

# Interview Questions and Answers

1. What is graceful shutdown?  
    Graceful shutdown allows an application to finish processing ongoing requests before shutting down.
    
2. How do you enable graceful shutdown in Spring Boot?  
    Set server.shutdown=graceful in application properties.
    
3. Why is graceful shutdown important in Kubernetes?  
    During rolling updates, Kubernetes terminates pods. Graceful shutdown prevents request failures.
    
4. What signal does Kubernetes send during pod termination?  
    SIGTERM.
    
5. What happens if terminationGracePeriodSeconds is less than Spring timeout?  
    Kubernetes may force kill the container before requests finish.
    
6. Does graceful shutdown stop new requests?  
    Yes. It stops accepting new requests but completes existing ones.