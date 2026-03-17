## 1. Overview

Spring Cloud provides tools to build distributed microservices systems.

Core features:

- Centralized configuration (Config Server)
    
- Service discovery (Eureka, Consul)
    
- Client-side load balancing
    
- API Gateway
    
- Resilience patterns
    

In microservices:

- Multiple independent services
    
- Each service runs on different ports/instances
    
- Services must discover and communicate dynamically
    

Spring Cloud solves these problems.

---

## 2. Centralized Configuration (Concept)

Problem:  
Each microservice has its own application.properties file.  
Managing configuration across environments becomes difficult.

Solution:  
Centralized configuration using Spring Cloud Config.

Architecture:

Config Server  
↓  
Microservices (Config Clients)

Benefits:

- Store config in Git
    
- Change config without rebuilding services
    
- Environment-based configuration (dev, test, prod)
    

Flow:

1. Service starts.
    
2. It fetches configuration from Config Server.
    
3. Loads external properties before application startup.
    

---

## 3. Service Discovery (Concept)

Problem:  
Microservices have dynamic IPs and ports.  
Hardcoding service URLs is not scalable.

Solution:  
Service Discovery Server.

Architecture:

Service Registry (Eureka/Consul)  
↓  
Service A registers  
Service B registers  
Clients discover services dynamically

Flow:

1. Service starts.
    
2. Registers itself with Discovery Server.
    
3. Other services query registry to get service location.
    
4. Communication happens using service name instead of IP.
    

Example:  
Instead of:

```
http://localhost:8082/order
```

Use:

```
http://ORDER-SERVICE/order
```

---

## 4. Why Config and Discovery are Core in Microservices

Without Config:

- Duplicate configuration
    
- Hard to manage environments
    
- Requires rebuild for changes
    

Without Discovery:

- Hardcoded URLs
    
- No auto-scaling support
    
- Poor fault tolerance
    

They form the foundation of Spring Cloud architecture.

---

## 5. Typical Microservices Architecture

1. Config Server (central config)
    
2. Discovery Server (service registry)
    
3. Multiple microservices
    
4. API Gateway
    
5. Database per service
    

Startup Order:

1. Config Server
    
2. Discovery Server
    
3. Microservices
    

---

## 6. Benefits in Production

- Dynamic scaling
    
- Centralized management
    
- Environment separation
    
- Loose coupling between services
    
- Cloud-native design
    

---

# Interview Questions and Answers

1. What is Spring Cloud?  
    Spring Cloud is a framework that provides tools for building distributed microservices systems.
    
2. Why do we need centralized configuration?  
    To manage environment-specific configurations in one place without rebuilding services.
    
3. What problem does service discovery solve?  
    It eliminates hardcoded service URLs and handles dynamic service instances.
    
4. How do services communicate in service discovery?  
    Using service names instead of IP addresses.
    
5. What happens if a service instance goes down?  
    Discovery server updates the registry and traffic is routed to available instances.
    
6. Which components are considered the foundation of Spring Cloud?  
    Config Server and Service Discovery.