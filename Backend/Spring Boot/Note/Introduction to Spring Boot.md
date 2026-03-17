## 1. What is Spring Boot?

**Spring Boot** is a framework built on top of the **Spring Framework** that simplifies the development of **[Standalone and production ready](Standalone%20and%20production%20ready.md) Java applications**.

> Spring Boot focuses on **convention over configuration**, allowing developers to build applications quickly with minimal setup.

In simple words:

- Spring Boot = Spring + Auto Configuration + Embedded Server + Production readiness
    

---

## 2. Why Spring Boot Was Introduced

### Problems with Traditional Spring (Before Spring Boot)

Before Spring Boot, developers faced:

- Heavy XML configuration
    
- Manual bean definitions
    
- External application servers (Tomcat, JBoss)
    
- Complex dependency management
    
- Long project setup time
    
- Difficult onboarding for new developers
    

Even a simple REST API required:

- web.xml
    
- dispatcher-servlet.xml
    
- applicationContext.xml
    
- server configuration
    

---

### How Spring Boot Solves These Problems

Spring Boot:

- Eliminates XML configuration
    
- Automatically configures beans
    
- Provides embedded servers
    
- Manages dependencies
    
- Allows application to run using a simple `main` method
    

---

## 3. Core Philosophy of Spring Boot

### 3.1 Convention Over Configuration

Spring Boot provides **default behavior** based on best practices.

You only configure:

- What is different from default
    
- What your application specifically needs
    

Example:

- No need to configure DispatcherServlet
    
- No need to configure Jackson manually
    
- No need to configure Tomcat
    

---

### 3.2 Opinionated Defaults

Spring Boot makes **smart decisions** for you.

Example:

- Tomcat as default server
    
- Jackson for JSON
    
- HikariCP for connection pooling
    
- Logback for logging
    

You can override defaults when required.

---

## 4. Key Features of Spring Boot

### 4.1 Auto Configuration

Spring Boot automatically configures components based on:

- Dependencies in classpath
    
- Application properties
    
- Existing beans
    

Example:  
If `spring-boot-starter-web` is present:

- DispatcherServlet is configured
    
- Embedded Tomcat is started
    
- MVC support is enabled
    

---

### 4.2 Starter Dependencies

Starters are **predefined dependency groups**.

Example:

```xml
spring-boot-starter-web
```

Includes:

- [Spring MVC](Spring%20MVC.md)
    
- Embedded Tomcat
    
- Jackson
    
- Validation
    

This removes dependency version conflicts.

---

### 4.3 Embedded Servers

Spring Boot applications run as **standalone JARs**.

Supported servers:

- Tomcat (default)
    
- Jetty
    
- Undertow
    

No external server deployment required.

Run command:

```bash
java -jar app.jar
```

---

### 4.4 Production-Ready Features

Spring Boot provides:

- Health checks
    
- Metrics
    
- Monitoring
    
- Externalized configuration
    

Handled using:

- Spring Boot Actuator
    

---

## 5. Spring Boot vs Spring Framework

|Aspect|Spring|Spring Boot|
|---|---|---|
|Configuration|Manual|Auto|
|XML usage|Heavy|Minimal / None|
|Server|External|Embedded|
|Startup|Complex|Simple|
|Focus|Framework|Productivity|

Spring Boot **does not replace Spring**, it **uses Spring internally**.

---

## 6. Basic Spring Boot Application Structure

```text
project
 └── src
     └── main
         ├── java
         │   └── com.example.demo
         │       └── DemoApplication.java
         └── resources
             ├── application.properties
             └── static / templates
```

---

## 7. The Entry Point of Spring Boot Application

```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

### What `@SpringBootApplication` Means

It is a combination of:

- `@Configuration` – Java-based config
    
- `@EnableAutoConfiguration` – Auto setup
    
- `@ComponentScan` – Scans beans
    

---

## 8. How Spring Boot Application Starts (High-Level)

1. JVM starts
    
2. `main()` method runs
    
3. SpringApplication creates ApplicationContext
    
4. Auto-configuration happens
    
5. Embedded server starts
    
6. Application is ready to accept requests
    

---

## 9. What Spring Boot Is Used For

Spring Boot is used to build:

- REST APIs
    
- Microservices
    
- Web applications
    
- Backend for mobile apps
    
- Cloud-native services
    

---

## 10. Common Misconceptions

- Spring Boot is not a replacement for Spring
    
- Spring Boot is not only for REST APIs
    
- Spring Boot does not remove Spring concepts
    
- Spring Boot still uses Dependency Injection
    

---

## 11. Advantages of Spring Boot

- Faster development
    
- Reduced boilerplate
    
- Easy testing
    
- Easy deployment
    
- Strong community support
    
- Production readiness
    

---

## 12. When NOT to Use Spring Boot

Avoid Spring Boot when:

- Extremely lightweight applications are required
    
- Memory footprint must be minimal
    
- No Spring ecosystem is needed
    

---

## 13. One-Line Summary

> Spring Boot simplifies Spring application development by providing auto-configuration, embedded servers, and production-ready features.

---

### What Comes Next in Your Roadmap

Next logical notes:

1. **Spring vs Spring Boot**
    
2. **Spring Boot Starter Dependencies**
    
3. **Spring Boot Auto Configuration**
    
4. **Spring Boot Core Annotations**
    

