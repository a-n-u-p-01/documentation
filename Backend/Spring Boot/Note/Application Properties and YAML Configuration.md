## 1. Overview

Spring Boot uses configuration files to control application behavior without changing the source code. This approach is called **externalized configuration**.

Instead of hardcoding values like database URLs, ports, API keys, or secrets inside Java classes, they are stored outside the application and loaded at runtime.

### Why it matters

- Enables environment-specific setups (dev, test, prod)
    
- Improves security by separating secrets from code
    
- Simplifies deployment
    
- Supports cloud-native architectures
    
- Makes applications easier to maintain and scale
    

---

## 2. Default Configuration Files

Spring Boot automatically looks for configuration inside:

```
src/main/resources
```

Supported formats:

```
application.properties
application.yml (or application.yaml)
```

Both are fully supported and functionally equivalent.

---

## 3. application.properties

Uses a flat key-value format.

Example:

```properties
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/appdb
spring.datasource.username=root
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Advantages

- Simple
    
- Easy to understand
    
- Good for small applications
    

### Disadvantages

- Hard to manage in large projects
    
- No visible hierarchy
    
- Becomes cluttered quickly
    

---

## 4. application.yml (YAML)

YAML provides hierarchical structure using indentation.

Example:

```yaml
server:
  port: 8081

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/appdb
    username: root
    password: secret

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### Advantages

- Highly readable
    
- Cleaner structure
    
- Ideal for microservices
    
- Reduces duplication
    

### Important Rule

YAML is indentation-sensitive.

- Use spaces only (commonly 2 spaces)
    
- Never use tabs
    
- Incorrect indentation prevents application startup
    

---

## 5. Properties vs YAML — Which One to Choose?

Both are valid; neither is deprecated.

**Use properties when:**

- Project is small
    
- Configuration is minimal
    
- Team prefers traditional format
    

**Use YAML when:**

- Configuration is complex
    
- You need hierarchy
    
- Building distributed systems
    

**Best Practice:**  
Do not mix formats unnecessarily within the same service.

---

## 6. Configuration Loading Priority (Precedence)

When the same property appears in multiple places, Spring Boot applies the highest-priority value.

Typical order:

1. Command-line arguments
    
2. Environment variables
    
3. Profile-specific files (`application-prod.yml`)
    
4. application.yml / application.properties
    
5. Default framework values
    

Example:

```
java -jar app.jar --server.port=9090
```

This overrides the port defined in the config file.

Understanding precedence is critical for debugging configuration conflicts.

---

## 7. Common Production Configurations

### Change Server Port

```properties
server.port=9090
```

### Set Context Path

```properties
server.servlet.context-path=/api
```

Endpoint becomes:

```
http://localhost:9090/api/users
```

### Database Configuration

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/appdb
    username: admin
    password: strongpassword
```

**Never commit real credentials to version control.**

Use environment variables instead.

---

## 8. Logging Configuration

Logging is essential for observability.

```properties
logging.level.root=INFO
logging.level.org.springframework.web=DEBUG
```

Typical usage:

|Level|Usage|
|---|---|
|DEBUG|Development|
|INFO|Production default|
|WARN|Unexpected situations|
|ERROR|Failures|

Avoid DEBUG in production unless troubleshooting.

---

## 9. Hibernate / JPA Configuration

```properties
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
```

### ddl-auto options

|Value|Behavior|
|---|---|
|none|No schema changes|
|validate|Verifies schema|
|update|Updates schema safely|
|create|Drops and recreates tables|
|create-drop|Drops schema on shutdown|

**Production Recommendation:**  
Use `validate` + migration tools like Flyway/Liquibase.

Never use `create` or `create-drop` in production.

---

## 10. Injecting Configuration into Java

### Using `@Value`

Best for single properties.

```java
@Value("${server.port}")
private String port;
```

With default value:

```java
@Value("${timeout:30}")
private int timeout;
```

If the property is missing, `30` is used.

**Limitation:** Not ideal for grouped configurations.

---

## 11. Type-Safe Configuration with `@ConfigurationProperties` (Recommended)

Better for structured configuration.

Example YAML:

```yaml
app:
  jwt:
    secret: mySecret
    expiration: 3600
```

Java binding:

```java
@ConfigurationProperties(prefix = "app.jwt")
@Component
public class JwtConfig {

    private String secret;
    private int expiration;

    // getters and setters
}
```

### Benefits

- Type-safe
    
- Cleaner code
    
- Easier testing
    
- Scales well in large systems
    

Preferred in production applications.

---

## 12. Profiles (Environment-Based Configuration)

Profiles allow different configurations per environment.

Example files:

```
application-dev.yml
application-test.yml
application-prod.yml
```

Activate a profile:

```properties
spring.profiles.active=prod
```

Spring loads the matching configuration automatically.

### Example Use Cases

- Dev → local DB
    
- Test → container DB
    
- Prod → managed cloud DB
    

Profiles are considered essential in professional systems.

---

## 13. Handling Secrets Securely

Never store sensitive data directly in config files.

Avoid:

```
password=admin123
jwt.secret=mysecretkey
```

Instead use environment variables:

```properties
spring.datasource.password=${DB_PASSWORD}
```

Spring resolves the value from the OS.

### Common Secret Managers

- HashiCorp Vault
    
- AWS Secrets Manager
    
- Azure Key Vault
    

---

## 14. Best Practices

- Keep configuration minimal and organized.
    
- Group related properties.
    
- Use profiles for environments.
    
- Prefer YAML for complex systems.
    
- Use `@ConfigurationProperties` over excessive `@Value`.
    
- Never expose secrets.
    
- Validate configuration during startup.
    
- Document important properties for team members.
    

---

## 15. Common Developer Mistakes

- Hardcoding credentials
    
- Ignoring profiles
    
- Using `create-drop` in production
    
- Mixing YAML and properties randomly
    
- Creating oversized configuration files
    
- Not understanding precedence rules
    

These mistakes often cause deployment failures.

---

## 16. Interview-Focused Questions

**1. Can Spring Boot support both properties and YAML?**  
Yes, both are fully supported.

**2. Which property source has the highest priority?**  
Command-line arguments.

**3. How do you inject configuration into a class?**  
Using `@Value` or `@ConfigurationProperties`.

**4. Why avoid hardcoded configuration?**  
For flexibility, security, and environment portability.

**5. What is the difference between `update` and `validate` in JPA?**  
`update` modifies schema; `validate` only checks compatibility.

---

## Final Takeaway

Understanding configuration is not just a beginner topic — it reflects production readiness.

A developer who properly manages configuration demonstrates:

- Strong fundamentals
    
- Deployment awareness
    
- Security consciousness
    
- Real-world engineering practices
    