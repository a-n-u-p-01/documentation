REST is the **standard architecture style** used to build modern backend APIs.

Most Spring Boot applications are simply **REST API providers**, so mastering this topic is critical for interviews and real-world development.

---

# 1. What is REST?

REST stands for:

> **Representational State Transfer**

It is an architectural approach for designing networked applications using **HTTP protocols**.

Instead of operations like:

```
/getUsers
/createUser
/deleteUser
```

REST focuses on **resources**.

Example:

```
/users
/orders
/products
```

You manipulate these resources using HTTP methods.

---

# 2. Core Principles of REST

## Resource-Based Design

Everything is treated as a resource.

Example:

```
/users/101
```

Represents a specific user.

NOT:

```
/getUserById
```

---

## Statelessness

Each request must contain **all required information**.

Server should NOT remember client state.

Good:

```
GET /orders/10
Authorization: Bearer token
```

Bad:  
Server relying on previous request memory.

### Why it matters:

- Improves scalability
    
- Easier load balancing
    
- Better fault tolerance
    

---

## Standard HTTP Methods

|Method|Purpose|Example|
|---|---|---|
|GET|Fetch resource|Get user|
|POST|Create resource|Add user|
|PUT|Replace resource|Update full user|
|PATCH|Partial update|Update email|
|DELETE|Remove resource|Delete user|

Interviewers often test this mapping.

---

## Proper Status Codes

Return meaningful responses.

|Code|Meaning|
|---|---|
|200|Success|
|201|Created|
|204|No content|
|400|Bad request|
|401|Unauthorized|
|404|Not found|
|500|Server error|

Avoid always returning `200`.

It signals poor API design.

---

# 3. CRUD Mapping to REST

CRUD = Create, Read, Update, Delete

### Create

```
POST /users
```

Creates a new user.

Response:

```
201 Created
```

---

### Read

Fetch all:

```
GET /users
```

Fetch one:

```
GET /users/{id}
```

---

### Update

Full update:

```
PUT /users/{id}
```

Partial:

```
PATCH /users/{id}
```

---

### Delete

```
DELETE /users/{id}
```

Return:

```
204 No Content
```

---

# 4. REST Endpoint Best Practices

## Use Nouns, Not Verbs

Good:

```
/orders
```

Bad:

```
/createOrder
```

---

## Use Plurals

```
/users
/products
```

Industry convention.

---

## Keep URLs Hierarchical

```
/users/{id}/orders
```

Shows relationship.

---

## Avoid Deep Nesting

Bad:

```
/users/1/orders/2/items/5/payments
```

Hard to maintain.

---

# 5. Layered Architecture (VERY IMPORTANT)

Professional Spring apps follow a layered structure.

## Standard Flow

```
Client
 ↓
Controller
 ↓
Service
 ↓
Repository
 ↓
Database
```

Understanding this is a huge interview advantage.

---

## Controller Layer

### Responsibility:

- Accept HTTP requests
    
- Validate input
    
- Return responses
    

Should NOT contain business logic.

Bad practice:  
Putting calculations here.

---

## Service Layer

### The brain of the application.

Handles:

- Business rules
    
- Transactions
    
- Data processing
    
- Orchestration
    

Example:

- Calculate discount
    
- Validate order
    
- Check inventory
    

Controllers should delegate here.

---

## Repository Layer

Responsible for:

- Database interaction
    
- Queries
    
- Persistence
    

No business logic allowed.

---

# Why Layering Matters

## Separation of Concerns

Each layer has one responsibility.

## Easier Testing

Mock service without database.

## Maintainability

Changes don’t ripple across the system.

## Scalability

Supports microservice patterns.

---

# Example Flow (Real Scenario)

### Request:

```
POST /orders
```

### Controller:

Receives request → calls service.

### Service:

Validates stock → calculates price → saves order.

### Repository:

Writes to database.

### Response:

```
201 Created
```

Clean and predictable.

---

# 6. DTO Pattern (Highly Recommended)

Never expose database entities directly.

Why?

- Security risks
    
- Tight coupling
    
- Breaking changes affect clients
    

Use:

```
Request DTO → Service → Entity → Response DTO
```

This is considered professional API design.

---

# 7. Idempotency (Advanced but Valuable)

An operation is idempotent if repeating it produces the same result.

### Idempotent:

```
PUT /users/10
DELETE /users/10
```

Calling multiple times → same outcome.

### Non-idempotent:

```
POST /users
```

Creates multiple users.

Interviewers like this concept.

---

# 8. Common REST Mistakes

## Returning Everything as 200

Destroys API clarity.

---

## Fat Controllers

Business logic belongs in services.

---

## Ignoring Validation

Leads to corrupted data.

---

## Breaking Statelessness

Avoid storing session data unless necessary.

---

## Poor Naming

Endpoints should be predictable.

---

# 9. What Interviewers Look For

Not just CRUD.

They evaluate whether you understand:

- HTTP semantics
    
- Status codes
    
- Clean architecture
    
- API consistency
    
- Resource modeling
    

Many candidates fail here.

---

# High-Probability Interview Questions

### What is REST?

An architectural style for designing stateless, resource-based APIs using HTTP.

---

### Difference between PUT and PATCH?

PUT replaces the entire resource.  
PATCH updates partially.

---

### Why layered architecture?

For separation of concerns, maintainability, and scalability.

---

### Should controllers contain business logic?

No.

---

### What makes an API RESTful?

Resource-based URLs, proper HTTP methods, statelessness, and correct status codes.

---

# Quick Memory Summary

```
REST = Resources + HTTP Methods + Statelessness
Layered = Controller → Service → Repository
CRUD → POST, GET, PUT/PATCH, DELETE
```

---

# Final Takeaway

REST + layered design is the **backbone of modern backend systems**.

If you master this, you demonstrate:

- Strong architectural thinking
    
- Production readiness
    
- API design maturity
    

### Golden Rule:

> Thin controllers, smart services, clean repositories, resource-based APIs.