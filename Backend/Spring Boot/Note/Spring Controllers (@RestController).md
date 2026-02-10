## 1. What is a Controller?

A controller is the **entry point of an HTTP request** in a Spring web application.

It is responsible for:

- Receiving client requests
    
- Delegating work to the service layer
    
- Returning HTTP responses
    

Think of it as the **bridge between the client and the backend logic**.

---

## 2. What is `@RestController`?

`@RestController` is a specialized annotation used to build **REST APIs**.

It is equivalent to combining:

```
@Controller + @ResponseBody
```

Meaning:

> Every method automatically returns data directly in the HTTP response body (usually JSON).

No view rendering.

No templates.

Pure API behavior.

---

## 3. Why Use @RestController Instead of @Controller?

### `@Controller`

Used for traditional web apps that return:

- HTML pages
    
- JSP
    
- Thymeleaf templates
    

Example return:

```
login.html
```

---

### `@RestController`

Used for backend services that return:

- JSON
    
- XML
    
- API responses
    

Example return:

```json
{
  "id": 1,
  "name": "Anupam"
}
```

Most modern systems use `@RestController`.

---

## 4. Basic Example

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}
```

### What Happens Internally?

1. Client sends request
    
2. Spring routes it to controller
    
3. Method executes
    
4. Object converted to JSON
    
5. Response sent
    

All automatically.

---

## 5. Key Responsibilities of Controllers

### Accept Requests

Using mapping annotations.

---

### Validate Input

Often combined with validation annotations.

---

### Call Service Layer

Controllers should never hold business logic.

---

### Return Proper Responses

Including status codes and structured data.

---

## 6. What Controllers Should NOT Do

### ❌ Business Logic

Wrong:

```java
if(orderAmount > 10000){
    applyDiscount();
}
```

Move this to the service.

---

### ❌ Database Calls

Never access repositories directly.

Always go through services.

---

### ❌ Heavy Processing

Controllers must stay lightweight.

### Golden Rule:

> Controllers should orchestrate, not compute.

---

## 7. Request Mapping at Controller Level

You can define a base path:

```java
@RequestMapping("/api/users")
```

Now all endpoints start with:

```
/api/users
```

Example:

```
GET /api/users/1
```

Improves organization.

---

## 8. Returning Objects

Spring automatically serializes objects.

```java
@GetMapping
public List<User> getAllUsers() {
    return userService.getAll();
}
```

Response becomes JSON.

No manual conversion needed.

---

## 9. ResponseEntity (Recommended Practice)

Although returning objects works, professional APIs often use `ResponseEntity`.

Why?

- Control status codes
    
- Customize headers
    
- Improve API clarity
    

Example:

```java
@GetMapping("/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {

    User user = userService.findById(id);

    return ResponseEntity.ok(user);
}
```

---

## 10. Common Controller Annotations

### Base Mapping

```
@RequestMapping
```

### HTTP-Specific

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

These improve readability over generic mappings.

---

## 11. Thin Controller Pattern (Industry Standard)

A strong backend always keeps controllers thin.

### Good Flow:

```
Controller → Service → Repository
```

### Bad Flow:

```
Controller → Repository
```

Skipping layers breaks architecture.

---

## 12. DTO Usage (Very Important)

Controllers should expose DTOs, NOT entities.

Why?

- Prevent schema leaks
    
- Avoid exposing sensitive fields
    
- Maintain API stability
    

Professional pattern:

```
Request DTO → Service → Response DTO
```

---

## 13. Error Handling

Controllers should NOT manually handle exceptions with try-catch everywhere.

Use global exception handlers instead.

Keeps controllers clean.

---

## 14. REST Controller Best Practices

### Use Resource-Based URLs

```
/users
/orders
```

Not:

```
/getUsers
```

---

### Keep Methods Small

Each endpoint should do one thing.

---

### Return Meaningful Status Codes

Avoid always returning success.

---

### Version Your APIs

Example:

```
/api/v1/users
```

Prevents breaking clients.

---

## 15. Performance Insight

Controllers should remain fast because they sit on the **request path**.

Heavy logic increases latency.

Let services handle computation.

---

## 16. Security Consideration

Controllers are public-facing.

Always assume input is untrusted.

Combine with:

- Validation
    
- Authentication
    
- Authorization
    

Never trust client data blindly.

---

## 17. Common Developer Mistakes

### Fat Controllers

Most frequent beginner error.

---

### Returning Entities Directly

Creates tight coupling.

---

### Skipping Service Layer

Leads to messy architecture.

---

### Too Many Responsibilities

Controllers should not transform, compute, persist, and validate all at once.

---

## 18. High-Probability Interview Questions

### What does @RestController do?

Marks a class as a REST controller where every method returns data directly in the response body.

---

### Difference between @Controller and @RestController?

@Controller returns views.  
@RestController returns data.

---

### Should controllers contain business logic?

No — delegate to services.

---

### Why use ResponseEntity?

To control HTTP status and headers.

---

### Can a controller talk directly to a repository?

Technically yes, architecturally wrong.

---

# Quick Memory Summary

```
@RestController =
Request Handling
+ Service Delegation
+ JSON Response
```

### One-Line Rule:

> Controllers handle HTTP — Services handle logic.

---

# Final Takeaway

Controllers define the **external contract** of your backend.

Well-designed controllers signal:

- Clean architecture
    
- Strong REST knowledge
    
- Production readiness
    

### Professional Guideline:

> Keep controllers thin, predictable, and focused purely on request-response handling.