Readiness and Liveness probes are health-check mechanisms used by Kubernetes to monitor application health.

They help Kubernetes:

- Restart unhealthy containers
    
- Control traffic flow
    
- Ensure zero-downtime deployments
    

Used heavily in Spring Boot microservices.

---

## 2. Liveness Probe

Purpose:  
Determines whether a container is alive.

If liveness probe fails:  
Kubernetes restarts the container.

Used for:

- Deadlocks
    
- Infinite loops
    
- Application stuck state
    

Example:

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 2
  failureThreshold: 3
```

Meaning:

- initialDelaySeconds: Wait before first check
    
- periodSeconds: Check interval
    
- failureThreshold: Restart after consecutive failures
    

---

## 3. Readiness Probe

Purpose:  
Determines whether a container is ready to accept traffic.

If readiness probe fails:

- Pod is removed from Service
    
- No traffic is sent
    
- Container is NOT restarted
    

Used for:

- Waiting for DB connection
    
- Waiting for cache warmup
    
- Startup delays
    

Example:

```yaml
readinessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 20
  periodSeconds: 5
```

---

## 4. Difference Between Liveness and Readiness

|Feature|Liveness|Readiness|
|---|---|---|
|Purpose|Check if app is alive|Check if app is ready|
|On Failure|Restart container|Remove from load balancer|
|Use Case|Deadlock detection|DB not connected|

---

## 5. Spring Boot Integration

Spring Boot Actuator provides health endpoints.

Enable probes support:

```properties
management.endpoint.health.probes.enabled=true
management.endpoints.web.exposure.include=health
```

Spring Boot automatically exposes:

- /actuator/health/liveness
    
- /actuator/health/readiness
    

Better Kubernetes configuration:

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 8080

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 8080
```

---

## 6. Types of Probes

1. HTTP Probe  
    Calls an HTTP endpoint.
    
2. TCP Probe  
    Checks if a port is open.
    
3. Exec Probe  
    Executes a command inside container.
    

Example (Exec):

```yaml
livenessProbe:
  exec:
    command:
      - cat
      - /tmp/healthy
```

---

## 7. Production Importance

- Prevents sending traffic to broken pods
    
- Enables zero-downtime rolling updates
    
- Improves system reliability
    
- Essential for cloud-native architecture
    

---

# Interview Questions and Answers

1. What is a liveness probe?  
    It checks whether the container is alive. If it fails, Kubernetes restarts the container.
    
2. What is a readiness probe?  
    It checks whether the application is ready to serve traffic. If it fails, traffic is stopped but container is not restarted.
    
3. Why use Actuator with Kubernetes probes?  
    Actuator provides health endpoints that Kubernetes can use to determine application health.
    
4. What happens if readiness fails but liveness succeeds?  
    The pod stays running but is removed from the Service load balancer.
    
5. Can we use the same endpoint for both probes?  
    Technically yes, but best practice is to use separate endpoints for accurate health reporting.
    
6. Which probe helps in zero-downtime deployment?  
    Readiness probe.