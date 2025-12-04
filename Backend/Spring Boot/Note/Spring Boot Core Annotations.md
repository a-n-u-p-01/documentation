Spring Boot relies heavily on annotations to simplify configuration, enable automatic setup, and create a flexible, modular application structure. These annotations reduce boilerplate code and allow developers to focus on writing business logic instead of framework setup.

---

## 1. @SpringBootApplication

This annotation is the main entry point of a Spring Boot application.  
It is a composed annotation that includes:

- **@Configuration**
    
- **@EnableAutoConfiguration**
    
- **@ComponentScan**
    

### What it does:

- Treats the class as a configuration class.
    
- Automatically configures Spring components based on the classpath.
    
- Scans the base package and sub-packages for Spring components.
    

### Why it should be on the root package:

Spring Boot uses package scanning from the package where this annotation is placed.  
Placing it deeper in the package structure will cause missing beans.

### Example:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

## 2. @Configuration

Marks a class as a configuration class where bean definitions are declared.  
This is part of Java-based configuration and replaces the older XML configuration.

### Key points:

- Methods annotated with @Bean inside this class create Spring-managed beans.
    
- Ensures that the class is processed by Spring’s configuration processor.
    
- Uses CGLIB to create proxies for method calls (to enforce singleton behavior).
    

### Example:

```java
@Configuration
public class AppConfig {
    @Bean
    public EmailService emailService() {
        return new EmailService();
    }
}
```

---

## 3. @Bean

The @Bean annotation is used within @Configuration classes to explicitly declare a bean.

### When to use:

- When you need to configure third-party components.
    
- When you want custom initialization logic.
    
- When classpath scanning is not possible or desired.
    

### Lifecycle:

Spring controls:

- Creation
    
- Dependency injection
    
- Initialization
    
- Destruction
    

### Example:

```java
@Bean
public ModelMapper modelMapper() {
    return new ModelMapper();
}
```

---

## 4. Stereotype Annotations

These annotations mark classes as Spring components for automatic detection.

### @Component

Generic component for any Spring bean.  
Scanned automatically by @ComponentScan.

### @Service

A specialization of @Component used to define service-layer classes.  
Provides semantic meaning.

### @Repository

Used for persistence-layer classes.  
Adds **automatic exception translation**, converting low-level exceptions (JDBC, JPA) into Spring’s DataAccessException.

### @Controller

Used for MVC controllers that return view pages.

### @RestController

A combination of:

- @Controller
    
- @ResponseBody
    

It returns JSON/XML instead of rendering a view.

---

## 5. @Autowired

Used for dependency injection.  
Spring resolves and injects the required bean automatically.

### Injection Types:

- Constructor Injection (recommended)
    
- Setter Injection
    
- Field Injection (not recommended)
    

### Why constructor injection is preferred:

- Allows immutability
    
- Easier to test
    
- Avoids hidden dependencies
    
- Helps prevent circular dependencies
    

### Example:

```java
@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }
}
```

---

## 6. @Value

Injects values from properties, YAML files, or expressions.

Supports:

- Property placeholders
    
- SpEL (Spring Expression Language)
    
- Default values
    

### Example:

```java
@Value("${app.title:Default Title}")
private String title;
```

---

## 7. @ConfigurationProperties

Used for binding groups of external configuration settings into a POJO.

### Benefits:

- Type-safe configuration
    
- Cleaner and grouped properties
    
- Better validation
    

### Example:

application.yml

```yaml
app:
  name: DemoApp
  timeout: 5000
```

Binding class:

```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int timeout;
}
```

Registering:

```java
@EnableConfigurationProperties(AppProperties.class)
```

---

## 8. @EnableAutoConfiguration

Triggers Spring Boot’s auto-configuration mechanism.

### How it works:

- Loads auto-config classes from `spring.factories`
    
- Evaluates conditions like:
    
    - @ConditionalOnClass
        
    - @ConditionalOnMissingBean
        
    - @ConditionalOnProperty
        

Spring only configures components when needed.

### Example:

```java
@EnableAutoConfiguration
public class DemoApp { }
```

---

## 9. @ComponentScan

Instructs Spring to scan packages for components.

### Default behavior:

When using @SpringBootApplication, component scanning is already enabled for its package and sub-packages.

### Example:

```java
@ComponentScan("com.myapp.*")
```

---

## 10. @Profile

Used for environment-specific beans (dev, test, prod).

### Example:

```java
@Profile("dev")
@Bean
public DataSource devDatasource() { ... }
```

Uses application profiles defined in:

```
application-dev.yml
application-prod.yml
```

---

## 11. @ControllerAdvice and @RestControllerAdvice

Used for global exception handling, global data binding, and global request processing.

### Example:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<String> handle(RuntimeException ex) {
        return ResponseEntity.badRequest().body(ex.getMessage());
    }
}
```

---

## 12. @ResponseBody

Indicates that the return value of a method should be serialized to JSON/XML.  
Included by default in @RestController.

### Example:

```java
@ResponseBody
@GetMapping("/hello")
public String hello() {
    return "Hello World!";
}
```

---

## 13. HTTP Mapping Annotations

These map HTTP requests to controller methods.

- @RequestMapping
    
- @GetMapping
    
- @PostMapping
    
- @PutMapping
    
- @DeleteMapping
    
- @PatchMapping
    

Each corresponds to a specific HTTP method.

---

## 14. @EnableScheduling and @Scheduled

Used to create scheduled tasks.

### Example:

```java
@Scheduled(cron = "0 0 * * * *")
public void job() { }
```

---

## 15. @EnableAsync and @Async

Enables asynchronous execution.

### Example:

```java
@Async
public void sendEmail() {
    // runs in a separate thread
}
```

---

# Interview Questions and Answers

### 1. What combines to form @SpringBootApplication?

@Configuration, @EnableAutoConfiguration, and @ComponentScan.

### 2. Difference between @Component, @Service, and @Repository?

- @Component: generic bean
    
- @Service: business logic
    
- @Repository: data-access layer with exception translation
    

### 3. Why choose constructor injection over field injection?

It ensures immutability, easier testing, explicit dependencies, and avoids hidden or circular dependencies.

### 4. Difference between @Value and @ConfigurationProperties?

@Value injects single values, while @ConfigurationProperties binds groups of properties into a structured POJO.

### 5. What does @EnableAutoConfiguration do internally?

It reads configuration metadata from `spring.factories` and applies conditional logic to configure beans only when required.

### 6. Difference between @Controller and @RestController?

@RestController returns JSON/XML by default.  
@Controller returns views unless @ResponseBody is added.

### 7. How does @ControllerAdvice work?

It centralizes exception handling for all controllers using @ExceptionHandler methods.

### 8. When should you use @Bean instead of @Component?

Use @Bean when creating objects from external libraries or when custom initialization is required.

### 9. How does @Profile work?

It activates beans only for specific environments based on active Spring profile.

### 10. Why avoid field injection with @Autowired?

It hides dependencies, breaks immutability, complicates testing, and cannot be used with `final` fields.
