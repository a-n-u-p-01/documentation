## 1. Overview

Spring Boot Admin is a monitoring dashboard for managing and observing multiple Spring Boot applications.

It provides:

- Centralized monitoring
    
- UI for Actuator endpoints
    
- Health status overview
    
- Metrics visualization
    
- Log level management
    
- Notification support
    

Used mainly in microservices architecture.

---

## 2. Architecture

There are two components:

1. Admin Server
    
2. Admin Client (your Spring Boot applications)
    

All client applications register with the Admin Server.

---

## 3. Setup

### Step 1: Add Admin Server

Dependency:

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>3.2.0</version>
</dependency>
```

Enable server:

```java
@EnableAdminServer
@SpringBootApplication
public class AdminServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(AdminServerApplication.class, args);
    }
}
```

---

### Step 2: Add Admin Client (in monitored app)

Dependency:

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>3.2.0</version>
</dependency>
```

Configure client:

```properties
spring.boot.admin.client.url=http://localhost:8080
management.endpoints.web.exposure.include=*
```

---

## 4. Features

1. Health Monitoring  
    Shows UP / DOWN status of all services.
    
2. Metrics  
    JVM memory, CPU, HTTP requests, GC, threads.
    
3. Environment Viewer  
    View environment properties.
    
4. Log Level Management  
    Change logging level at runtime.
    
5. Thread Dump & Heap Dump  
    Diagnose performance issues.
    
6. Notifications  
    Email, Slack, etc. when service goes DOWN.
    

---

## 5. Security

In production:

- Secure admin server using Spring Security.
    
- Do not expose it publicly.
    
- Protect actuator endpoints.
    

---

## 6. Use Cases

- Microservices monitoring
    
- Central dashboard for 10–50 services
    
- Debug production failures
    
- Observability in cloud environments
    

---

# Interview Questions and Answers

1. What is Spring Boot Admin?  
    Spring Boot Admin is a monitoring tool that provides a centralized dashboard for managing multiple Spring Boot applications.
    
2. How does Spring Boot Admin work?  
    It consists of an Admin Server and multiple Admin Clients. Clients register themselves with the server using Actuator endpoints.
    
3. What dependency is required for monitoring applications?  
    spring-boot-admin-starter-client and spring-boot-starter-actuator.
    
4. Is Spring Boot Admin mandatory for monitoring?  
    No. Actuator is enough, but Spring Boot Admin provides a UI dashboard for centralized monitoring.
    
5. How is Spring Boot Admin used in microservices?  
    Each microservice registers with the Admin Server, allowing centralized health, metrics, and log management.
    
6. How do you secure Spring Boot Admin in production?  
    Use Spring Security, restrict actuator exposure, and avoid public access.