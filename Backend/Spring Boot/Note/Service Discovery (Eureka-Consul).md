Service Discovery is a mechanism that allows microservices to dynamically locate and communicate with each other without hardcoding IP addresses and ports.

In microservices architecture:

- Services scale horizontally.
    
- Instances start and stop dynamically.
    
- Containers/pods get new IP addresses.
    
- Hardcoding URLs becomes unreliable.
    

Service Discovery solves this by introducing a Service Registry.

---

# 2. Why Service Discovery Is Needed

Problem Without Discovery:

Service A needs to call Service B.

If Service B runs at:

```
http://192.168.1.10:8082
```

And later scales or restarts:

```
http://192.168.1.15:8083
```

Now Service A breaks.

In distributed systems:

- IPs are dynamic
    
- Instances increase/decrease
    
- Manual updates are not feasible
    

Solution:  
Use a registry to dynamically track service locations.

---

# 3. Core Components of Service Discovery

1. Service Registry  
    Stores all available service instances.
    
2. Service Provider  
    Registers itself at startup.
    
3. Service Consumer  
    Queries registry to find service instances.
    

---

# 4. Basic Flow

Step 1:  
Service starts.

Step 2:  
Service registers with registry.

Step 3:  
Registry stores:

- Service name
    
- IP address
    
- Port
    
- Health status
    

Step 4:  
Another service queries registry using service name.

Step 5:  
Load balancer selects one instance.

Instead of calling:

```
http://localhost:8082/orders
```

You call:

```
http://ORDER-SERVICE/orders
```

The system resolves ORDER-SERVICE dynamically.

---

# 5. Two Environments (Important Distinction)

There are two different infrastructures where service discovery behaves differently.

---

# Environment 1: Traditional (VM-Based) Architecture

Examples:

- AWS EC2
    
- Bare metal servers
    
- Docker without Kubernetes
    

Here you MUST use:

- Eureka
    
- Consul
    

Because:  
There is no built-in service registry.

This is called Client-Side Service Discovery.

---

# 6. Netflix Eureka

## What is Eureka?

Eureka is a service registry developed by Netflix.

It works using:

- Registration
    
- Heartbeats
    
- Self-preservation mechanism
    

---

## Eureka Architecture

Eureka Server (Registry)

Microservices:

- Order Service
    
- User Service
    
- Payment Service
    

Each service:

- Registers at startup
    
- Sends heartbeat every 30 seconds
    
- Gets removed if heartbeat fails
    

---

## Registration Process

1. Service starts.
    
2. Registers with Eureka Server.
    
3. Sends periodic heartbeat.
    
4. If instance crashes:
    
    - Heartbeat stops
        
    - Eureka removes it.
        

---

## Multiple Instances Handling

Example:

order-service

- Instance 1 → 10.0.0.1:8081
    
- Instance 2 → 10.0.0.2:8082
    
- Instance 3 → 10.0.0.3:8083
    

Registry stores all 3.

When another service calls ORDER-SERVICE:

1. Fetch list of 3 instances.
    
2. Load balancer selects one (Round Robin by default).
    
3. Request routed.
    

If Instance 2 fails:

- Removed from registry.
    
- Traffic goes only to 1 and 3.
    

No corruption, no failure propagation.

---

# 7. Consul

## What is Consul?

Consul is a service discovery tool by HashiCorp.

It provides:

- Service registry
    
- Health checks
    
- Key-value store
    
- Multi-datacenter support
    

Unlike Eureka:

- Consul performs active health checks.
    
- Has stronger production-grade clustering.
    

---

## Consul Architecture

Consul Server Cluster  
↓  
Consul Agents (on each node)  
↓  
Services register via agents

Consul automatically:

- Checks service health
    
- Removes unhealthy instances
    

---

# 8. Eureka vs Consul

|Feature|Eureka|Consul|
|---|---|---|
|Health mechanism|Heartbeat|Active health checks|
|KV Store|No|Yes|
|Multi-datacenter|Limited|Strong|
|Complexity|Simple|More powerful|

---

# 9. Kubernetes Service Discovery (Very Important Clarification)

If you are using Kubernetes:

You normally DO NOT use Eureka.

Because Kubernetes already provides:

- Service registry
    
- DNS resolution
    
- Load balancing
    
- Health checks
    
- Auto-scaling
    
- Self-healing
    

In Kubernetes:

You create:

Deployment (replicas: 3)  
Service (ClusterIP)

Then you call:

```
http://order-service
```

Kubernetes:

- Tracks all pods
    
- Automatically load balances
    
- Removes unhealthy pods
    

This is Server-Side Service Discovery.

---

# 10. When to Use Which

Use Eureka or Consul when:

- Running on VMs
    
- No Kubernetes
    
- Need manual registry
    
- Legacy Spring Cloud setup
    

Use Kubernetes Service Discovery when:

- Running on Kubernetes (EKS, AKS, GKE)
    
- Cloud-native architecture
    

---

# 11. Common Confusion Clarified

If using Spring Boot on EC2:  
You need Eureka or Consul.

If using Spring Boot inside Kubernetes:  
You do NOT need Eureka.

Kubernetes replaces it.

---

# 12. High Availability in Service Discovery

Best Practices:

- Run multiple Eureka/Consul nodes
    
- Enable replication
    
- Secure registry endpoints
    
- Monitor health
    
- Use client-side load balancing
    

---

# 13. Interview Questions and Answers

1. What is service discovery?  
    It allows microservices to dynamically locate and communicate without hardcoded IP addresses.
    
2. What is the difference between client-side and server-side discovery?  
    Client-side: Client queries registry (Eureka).  
    Server-side: Infrastructure handles routing (Kubernetes).
    
3. When should you use Eureka?  
    When running microservices outside Kubernetes.
    
4. Why is Eureka not required in Kubernetes?  
    Because Kubernetes provides built-in service discovery and load balancing.
    
5. How are multiple instances managed?  
    Each instance registers separately and load balancer distributes traffic.
    
6. What happens if a service instance crashes?  
    Registry removes it (Eureka) or Kubernetes removes pod from service endpoints.
    
7. Which is more enterprise-ready, Eureka or Consul?  
    Consul provides more advanced features like KV store and multi-datacenter support.
    

---
