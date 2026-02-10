Security in backend systems is the practice of protecting applications, data, and resources from unauthorized access while ensuring that legitimate users can safely perform permitted actions. In modern web applications, every request that reaches the server must be verified and evaluated before it is allowed to interact with business logic.

Spring Security is built around three fundamental pillars:

- Authentication — verifying identity
    
- Authorization — determining permissions
    
- Filters — enforcing security for every request
    

Understanding these three deeply removes most confusion around Spring Security.

---

## What Problem Does Application Security Solve?

Without security:

- Anyone could access private APIs
    
- Attackers could modify data
    
- Sensitive information could leak
    
- Admin operations could be executed by normal users
    

Security ensures:

- Only verified users enter the system
    
- Users can perform only allowed actions
    
- Requests are validated before reaching controllers
    

Security is therefore not a feature — it is a foundational requirement of backend architecture.

---

# Authentication — Verifying Identity

Authentication is the process of confirming that a user or system is truly who they claim to be.

It answers the question:

**“Who is making this request?”**

Before a system grants access, it must verify identity using credentials.

---

## Common Authentication Methods

### Username and Password

The most traditional method.

Flow:

1. User submits credentials.
    
2. Server compares them with stored data (usually hashed passwords).
    
3. If they match, the user is authenticated.
    

---

### Token-Based Authentication (Modern Standard)

Instead of verifying credentials on every request:

1. User logs in once.
    
2. Server generates a token.
    
3. Client sends the token with every request.
    
4. Server validates the token.
    

This approach improves scalability and is widely used in REST APIs.

---

### OAuth / Social Login

Authentication is delegated to a trusted provider such as Google or GitHub.

The application trusts the provider’s verification rather than managing passwords itself.

---

## Authentication Flow (Conceptual)

```
Client → Sends credentials
Server → Validates credentials
Server → Creates authentication object
Security Context → Stores authenticated user
```

If authentication fails, the request is rejected immediately.

No authorization checks occur if identity cannot be verified.

---

## Important Principle

Authentication always happens **before** authorization.

The system cannot decide what a user can do until it knows who the user is.

---

# Authorization — Determining Permissions

Authorization is the process of deciding whether an authenticated user has permission to access a specific resource or perform a specific action.

It answers the question:

**“What is this user allowed to do?”**

---

## Example Scenario

A user successfully logs in.

Now they attempt to access:

```
/admin/deleteUser
```

The system evaluates their role.

- Admin → Access granted
    
- Regular user → Access denied
    

Even though both are authenticated, only one is authorized.

---

## Role-Based Access Control (Basic Model)

Most systems use roles to manage permissions.

Example:

|Role|Allowed Actions|
|---|---|
|USER|Read data|
|ADMIN|Read + delete data|
|MANAGER|Approve operations|

Authorization rules are typically enforced at:

- URL level
    
- Method level
    
- Service layer
    

---

## Key Insight

Authentication confirms identity.

Authorization enforces boundaries.

Both are mandatory for secure systems.

---

# Filters — The Enforcement Layer

Filters are the backbone of Spring Security.

They intercept **every incoming HTTP request** before it reaches the controller.

You can think of filters as a checkpoint that all traffic must pass through.

---

## Why Filters Exist

Without filters, developers would need to manually check authentication and authorization inside every controller method.

This would create:

- Duplicate code
    
- Security gaps
    
- Maintenance issues
    

Filters centralize security logic.

---

## Request Processing Flow

```
Client Request
      ↓
Security Filters
      ↓
Authentication Check
      ↓
Authorization Check
      ↓
Controller
```

If a request fails at any stage, it never reaches application logic.

---

## What Security Filters Actually Do

Security filters perform tasks such as:

- Extracting credentials or tokens from headers
    
- Validating tokens
    
- Checking session data
    
- Loading user details
    
- Creating authentication objects
    
- Blocking unauthorized requests
    

All of this happens automatically once Spring Security is configured.

---

## Example: Token-Based Request

Client sends:

```
GET /orders
Authorization: Bearer <token>
```

Filter actions:

1. Extract token from header.
    
2. Validate signature.
    
3. Check expiration.
    
4. Load user details.
    
5. Create authentication object.
    
6. Store it in the security context.
    

Only then is the request forwarded.

---

# Relationship Between Filters, Authentication, and Authorization

These components operate in a strict sequence:

1. Filters intercept the request.
    
2. Authentication verifies identity.
    
3. Security context stores the authenticated user.
    
4. Authorization checks permissions.
    
5. Request proceeds if allowed.
    

This pipeline is what secures Spring applications.

---

# SecurityContext — Why It Matters (Conceptual Awareness)

Once authentication succeeds, Spring stores the user inside a structure called the **SecurityContext**.

This allows the application to:

- Access the current user anywhere
    
- Evaluate roles
    
- Apply authorization rules
    

For example, method-level security uses this stored identity to determine access.

You do not need to manage it manually — Spring handles it.

---

# Stateless vs Stateful Security (Basic Awareness)

Although deeper study comes later, it is useful to understand the difference.

### Stateful (Session-Based)

- Server stores session data.
    
- Client sends session ID.
    
- Common in traditional web apps.
    

### Stateless (Token-Based)

- Server stores nothing.
    
- Client sends token every time.
    
- Preferred for REST APIs.
    

Most modern backend systems are stateless.

---

# Common Security Mistakes

## Treating Login as Complete Security

Authentication alone is not enough. Authorization must always follow.

---

## Writing Manual Role Checks

Avoid embedding permission logic directly in controllers. Centralized security is safer and easier to maintain.

---

## Ignoring HTTPS

Credentials transmitted over plain HTTP can be intercepted. Encryption is mandatory in production systems.

---

## Poor Password Storage

Passwords must never be stored as plain text. Always use hashing algorithms such as BCrypt.

---

# Why These Three Pillars Matter Architecturally

When implemented correctly, they provide:

- Centralized control
    
- Consistent enforcement
    
- Reduced duplication
    
- Better scalability
    
- Cleaner controllers
    

Security becomes part of the infrastructure rather than scattered across business code.

---

# Interview-Ready Summary

Security in backend applications is built on authentication, authorization, and filters. Authentication verifies the identity of the requester, authorization determines what actions they are permitted to perform, and filters intercept every request to enforce these checks before allowing access to application resources.

---

# Memory Anchor

Filters enforce security.  
Authentication establishes identity.  
Authorization grants or denies access.
