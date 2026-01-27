## 1. What is Spring MVC?

**Spring MVC** is a **web framework** provided by the Spring Framework for building **web applications and RESTful services** using the **MVC design pattern**.

> Spring MVC separates request handling, business logic, and presentation to build clean and maintainable web applications.

Spring Boot **uses Spring MVC internally** when you add `spring-boot-starter-web`.

---

## 2. Full Form of MVC

**MVC = Model – View – Controller**

|Component|Meaning|Responsibility|
|---|---|---|
|**Model**|Data|Holds application data and state|
|**View**|UI|Renders response (HTML, JSON, etc.)|
|**Controller**|Request handler|Handles HTTP requests and responses|

---

## 3. Why Spring MVC Is Needed

Without MVC:

- Business logic and UI logic mix together
    
- Code becomes hard to maintain
    
- Testing becomes difficult
    
- Application does not scale well
    

Spring MVC solves this by:

- Separating concerns
    
- Providing a structured request lifecycle
    
- Supporting REST and traditional web apps
    

---

## 4. Spring MVC Architecture (High-Level)

Spring MVC follows the **Front Controller Pattern**.

### Core Component

**DispatcherServlet** (Front Controller)

All incoming HTTP requests go through **DispatcherServlet**.

---

## 5. Spring MVC Internal Flow (Correct & Interview-Ready)

### Step-by-step flow:

1. Client sends HTTP request
    
2. **DispatcherServlet** receives request
    
3. **HandlerMapping** finds matching controller
    
4. **HandlerAdapter** invokes controller method
    
5. Controller processes request
    
6. Controller returns response
    
7. Response is resolved via:
    
    - **ViewResolver** (MVC)
        
    - **HttpMessageConverter** (REST)
        
8. DispatcherServlet sends response to client
    

---

## 6. Key Spring MVC Components

### 6.1 DispatcherServlet

- Central front controller
    
- Manages request lifecycle
    
- Created automatically in Spring Boot
    

---

### 6.2 HandlerMapping

- Maps URL + HTTP method to controller method
    
- Uses annotations like `@GetMapping`
    

---

### 6.3 HandlerAdapter

- Invokes controller methods
    
- Abstracts different handler types
    

---

### 6.4 Controller

- Handles request
    
- Calls service layer
    
- Returns view name or data
    

---

### 6.5 ViewResolver

- Resolves logical view names to actual views
    
- Used only in MVC (HTML)
    

---

### 6.6 HttpMessageConverter

- Converts Java objects ↔ JSON/XML
    
- Used in REST APIs
    

---

## 7. Spring MVC vs REST in Spring Boot

Spring MVC supports **both**.

### MVC (Server-side rendering)

```java
@Controller
public class HomeController {

    @GetMapping("/home")
    public String home(Model model) {
        model.addAttribute("msg", "Hello");
        return "home";
    }
}
```

- Returns view name
    
- Uses Thymeleaf/JSP
    
- ViewResolver involved
    

---

### REST (API-based)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User get(@PathVariable Long id) {
        return service.getUser(id);
    }
}
```

- Returns JSON
    
- Uses HttpMessageConverter
    
- No ViewResolver
    

---

## 8. Important Spring MVC Annotations

### Controller Annotations

|Annotation|Purpose|
|---|---|
|`@Controller`|MVC controller|
|`@RestController`|REST controller|
|`@RequestMapping`|Map request|
|`@GetMapping`|HTTP GET|
|`@PostMapping`|HTTP POST|
|`@PutMapping`|HTTP PUT|
|`@DeleteMapping`|HTTP DELETE|

---

### Request Handling

|Annotation|Purpose|
|---|---|
|`@PathVariable`|URL variable|
|`@RequestParam`|Query parameter|
|`@RequestBody`|JSON request|
|`@RequestHeader`|HTTP headers|

---

### Response Handling

|Annotation|Purpose|
|---|---|
|`@ResponseBody`|Return data|
|`ResponseEntity`|Control status + headers|

---

## 9. Model in Spring MVC

```java
@GetMapping("/page")
public String page(Model model) {
    model.addAttribute("name", "Anupam");
    return "page";
}
```

- Model stores data for the view
    
- Used only in MVC (HTML)
    

---

## 10. Validation in Spring MVC

```java
@PostMapping("/users")
public ResponseEntity<?> create(@Valid @RequestBody UserDto dto) {
}
```

- Uses Bean Validation
    
- Errors handled automatically or via `@ControllerAdvice`
    

---

## 11. Exception Handling in Spring MVC

### Local Exception Handling

```java
@ExceptionHandler(RuntimeException.class)
public ResponseEntity<String> handle(RuntimeException ex) {
    return ResponseEntity.badRequest().body(ex.getMessage());
}
```

---

### Global Exception Handling

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
}
```

---

## 12. Filters vs Interceptors (MVC Related)

|Filter|Interceptor|
|---|---|
|Servlet-level|Spring MVC-level|
|Runs before DispatcherServlet|Runs around controller|
|Good for security/logging|Good for request logic|

---

## 13. Spring MVC in Spring Boot

Spring Boot:

- Auto-configures DispatcherServlet
    
- Auto-registers message converters
    
- Reduces manual configuration
    
- Simplifies MVC setup
    

You rarely configure Spring MVC manually in Spring Boot.

---

## 14. Common Interview Questions

**Q1. What is DispatcherServlet?**  
Front controller that handles all requests.

**Q2. Difference between @Controller and @RestController?**  
@Controller returns views, @RestController returns data.

**Q3. Does Spring MVC support REST?**  
Yes, REST is built on Spring MVC.

**Q4. ViewResolver used in REST?**  
No.

---

## 15. When to Use Spring MVC

Use Spring MVC when:

- Building REST APIs
    
- Building server-side rendered apps
    
- Using Spring Boot Web
    
- Creating layered backend systems
    

---

## 16. Key Takeaways

- Spring MVC is a web framework
    
- Based on MVC + Front Controller pattern
    
- DispatcherServlet is the core
    
- Supports both REST and MVC
    
- Foundation of Spring Boot Web
    

---

## 17. One-Line Summary

> Spring MVC is a web framework that handles HTTP requests using the MVC pattern and a centralized DispatcherServlet.

---

### Next Logical Topics

- Spring MVC Interceptors vs Filters
    
- REST API Best Practices
    
- Content Negotiation
    
- Spring WebFlux vs Spring MVC
    

Tell me what you want next.