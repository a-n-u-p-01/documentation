Spring Cloud Config Server provides centralized configuration management for microservices.

Instead of storing configuration inside each service, all configuration is stored in a central Git repository.

Benefits:

- Centralized management
    
- Environment-specific configuration
    
- No rebuild required for config changes
    
- Version-controlled configuration
    
- Easy rollback
    

---

## 2. Architecture

Components:

1. Config Server  
    Reads configuration from Git.
    
2. Config Client (Microservices)  
    Fetch configuration at startup.
    

Flow:

Git Repository  
↓  
Config Server  
↓  
Microservices (Config Clients)

---

## 3. How It Works

When a microservice starts:

1. It contacts Config Server.
    
2. Requests configuration using:
    
    - application name
        
    - active profile
        
3. Config Server fetches properties from Git.
    
4. Returns configuration before application fully starts.
    

Request Format:

```
http://localhost:8888/{application}/{profile}
```

Example:

```
http://localhost:8888/order-service/dev
```

---

## 4. Setting Up Config Server

### Step 1: Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

### Step 2: Enable Config Server

```java
@EnableConfigServer
@SpringBootApplication
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

### Step 3: Configure Git Repository

application.properties:

```properties
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/your-repo/config-repo
```

---

## 5. Git Repository Structure

Example:

```
order-service-dev.properties
order-service-prod.properties
user-service-dev.properties
```

Naming Pattern:

```
{application-name}-{profile}.properties
```

Example:

```
order-service-dev.properties
```

---

## 6. Config Client Setup (Microservice)

Add dependency:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

Configure bootstrap.properties (or application.properties in latest versions):

```properties
spring.application.name=order-service
spring.config.import=configserver:http://localhost:8888
spring.profiles.active=dev
```

On startup:  
Service loads config from Config Server.

---

## 7. Refreshing Configuration

If configuration changes in Git:

- Services do not restart automatically.
    

To refresh:

Add Actuator:

```xml
spring-boot-starter-actuator
```

Expose refresh endpoint:

```properties
management.endpoints.web.exposure.include=refresh
```

Call:

```
POST /actuator/refresh
```

This reloads configuration without restart.

---

## 8. Security Considerations

- Secure Git repository
    
- Protect Config Server using Spring Security
    
- Do not expose publicly
    
- Encrypt sensitive properties
    

Spring Cloud supports encryption using symmetric or asymmetric keys.

---

## 9. Production Best Practices

- Store config in private Git
    
- Separate config per environment
    
- Use encrypted values for secrets
    
- Combine with service discovery
    
- Monitor Config Server health
    

---

# Interview Questions and Answers

1. What is Spring Cloud Config Server?  
    It is a centralized configuration management system for microservices.
    
2. Where does Config Server store configuration?  
    Typically in a Git repository.
    
3. How does a client fetch configuration?  
    At startup using spring.config.import=configserver.
    
4. What is the naming convention of config files?  
    {application-name}-{profile}.properties
    
5. How do you refresh configuration without restarting service?  
    By calling POST /actuator/refresh.
    
6. Why is Config Server important in microservices?  
    It avoids duplicate configuration and allows centralized management across environments.