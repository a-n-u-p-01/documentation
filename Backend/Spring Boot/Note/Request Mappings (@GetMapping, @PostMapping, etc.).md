Request mappings define **how incoming HTTP requests are routed to controller methods**.

Without mappings, Spring would not know:

> Which method should handle which URL?

They are a core part of building REST APIs and are heavily tested in interviews.

---

# 1. What is Request Mapping?

Request mapping is the mechanism used by Spring to:

- Match URL paths
    
- Match HTTP methods
    
- Invoke the correct controller method
    

Example flow:

```
Client → /users/10 → Spring → Correct Controller Method
```

This routing is handled internally by the MVC dispatcher.

---

# 2. Types of Mapping Annotations

Spring provides specialized annotations for each HTTP method.

## GET — Fetch Data

Used for retrieving resources.

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```

### Characteristics:

- Safe (does not modify data)
    
- Idempotent
    
- Cacheable
    

**Never use GET to update data.**

---

## POST — Create Resource

```java
@PostMapping("/users")
public User createUser(@RequestBody UserRequest request) {
    return userService.create(request);
}
```

### Characteristics:

- Not idempotent
    
- Usually changes server state
    
- Often returns **201 Created**
    

Used for:

- Creating users
    
- Placing orders
    
- Uploading data
    

---

## PUT — Full Update

Replaces the entire resource.

```java
@PutMapping("/users/{id}")
public User updateUser(@PathVariable Long id,
                       @RequestBody UserRequest request) {
    return userService.update(id, request);
}
```

### Important Rule:

Client must send the full object.

If a field is missing → it may be overwritten.

PUT is idempotent.

---

## PATCH — Partial Update

Updates only selected fields.

```java
@PatchMapping("/users/{id}")
public User updateEmail(@PathVariable Long id,
                        @RequestBody Map<String, Object> updates) {
}
```

Better for:

- Status changes
    
- Email updates
    
- Small modifications
    

Not always implemented, but good to know.

---

## DELETE — Remove Resource

```java
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable Long id) {
    userService.delete(id);
}
```

Usually returns:

```
204 No Content
```

DELETE is idempotent.

Deleting the same resource multiple times → same result.

---

# 3. Generic Mapping — @RequestMapping

Before specialized annotations existed, everything used:

```java
@RequestMapping
```

Example:

```java
@RequestMapping(value="/users", method=RequestMethod.GET)
```

Now replaced by cleaner alternatives like `@GetMapping`.

Still useful for class-level mapping.

---

# 4. Class-Level Mapping (Best Practice)

Define a base path once.

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
}
```

Endpoints become:

```
GET /api/users
GET /api/users/{id}
```

Keeps URLs organized.

---

# 5. Mapping Multiple Paths

```java
@GetMapping({"/users", "/members"})
```

One method handles multiple routes.

Useful during API migrations.

---

# 6. Path Variables vs Query Parameters

## Path Variable → Identify Resource

```
GET /users/10
```

```java
@PathVariable Long id
```

Use when value is mandatory.

---

## Query Parameter → Filtering / Optional Data

```
GET /users?role=ADMIN
```

```java
@RequestParam String role
```

Use for:

- Filters
    
- Pagination
    
- Sorting
    

### Interview Favorite:

> Path → resource identity  
> Query → modifiers

---

# 7. Restricting by Content Type

## Consumes (What API Accepts)

```java
@PostMapping(value="/users",
             consumes="application/json")
```

Rejects other formats.

---

## Produces (What API Returns)

```java
@GetMapping(value="/users",
            produces="application/json")
```

Important for content negotiation.

---

# 8. Mapping Based on Headers

Rare but powerful.

```java
@GetMapping(value="/users",
            headers="X-Version=1")
```

Used in advanced API versioning.

---

# 9. Idempotency — Must Understand

|Method|Idempotent?|
|---|---|
|GET|Yes|
|PUT|Yes|
|DELETE|Yes|
|POST|No|
|PATCH|Usually no|

Interviewers LOVE this table.

---

# 10. Common REST Mapping Conventions

## Use Nouns, Not Verbs

Good:

```
POST /orders
```

Bad:

```
POST /createOrder
```

---

## Use Plurals

```
/users
/products
```

Industry standard.

---

## Keep URLs Predictable

Good APIs should feel guessable.

---

# 11. Ambiguous Mapping Error

If two methods map the same route:

Spring fails at startup.

Example mistake:

```
@GetMapping("/users")
@GetMapping("/users")
```

This fail-fast behavior prevents runtime chaos.

---

# 12. Advanced Tip — Method Overloading with Params

```java
@GetMapping(value="/users", params="email")
```

Handles:

```
/users?email=test@mail.com
```

Useful for search endpoints.

---

# 13. Performance Insight

Mapping resolution is optimized internally using handler mappings.

Even apps with thousands of endpoints perform efficiently.

You rarely need to worry about routing performance.

---

# 14. Common Developer Mistakes

### Using GET for Mutations

Breaks HTTP semantics.

---

### Ignoring Status Codes

Mapping and response should align.

---

### Overloading One Endpoint

Avoid writing one massive method handling multiple operations.

---

### Deep URL Nesting

Makes APIs hard to maintain.

---

# 15. High-Probability Interview Questions

### Difference between PUT and PATCH?

PUT replaces the entire resource.  
PATCH updates partially.

---

### Which methods are idempotent?

GET, PUT, DELETE.

---

### Difference between @PathVariable and @RequestParam?

Path → required resource identifier.  
Param → optional filter.

---

### Why use specialized mappings instead of @RequestMapping?

Better readability and intent.

---

# Quick Memory Summary

```
@GetMapping → Read
@PostMapping → Create
@PutMapping → Replace
@PatchMapping → Modify
@DeleteMapping → Remove
```

### Golden Rule:

> Correct HTTP method = Good API design.

---

# Final Takeaway

Request mappings define your **API surface**.

Mastering them shows:

- Strong REST understanding
    
- Clean endpoint design
    
- Professional backend skills
    

### Professional Guideline:

> Design predictable URLs, use correct HTTP methods, and let mappings clearly express intent.