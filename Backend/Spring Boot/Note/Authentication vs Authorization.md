Authentication and authorization are two fundamental concepts in application security. They work together to protect backend systems, but they serve very different purposes. Understanding the distinction between them is critical for designing secure applications and is one of the most frequently tested concepts in backend interviews.

At a high level:

- **Authentication verifies identity.**
    
- **Authorization determines permissions.**
    

Both must be implemented correctly to build a secure system.

---

## What is Authentication?

Authentication is the process of verifying the identity of a user or system attempting to access an application.

It answers the question:

**“Who is making this request?”**

Before a system grants access, it must confirm that the requester is genuine.

---

## How Authentication Works

Authentication typically involves validating credentials against trusted data stored in the system.

Basic flow:

1. A user submits credentials (for example, username and password).
    
2. The server validates those credentials against stored records.
    
3. If valid, the system creates an authenticated identity.
    
4. The authenticated user is stored for the duration of the request (commonly in a security context).
    
5. The request proceeds to the next security step.
    

If the credentials are invalid, the request is rejected immediately.

No authorization checks occur if identity cannot be verified.

---

## Common Authentication Methods

### Username and Password

The most traditional method of authentication.

Important practices:

- Passwords must never be stored in plain text.
    
- They should be hashed using strong algorithms such as BCrypt.
    
- During login, the entered password is hashed and compared with the stored hash.
    

---

### Token-Based Authentication

Widely used in REST APIs.

Instead of verifying credentials on every request:

1. The user logs in once.
    
2. The server generates a token.
    
3. The client sends the token with future requests.
    
4. The server validates the token before allowing access.
    

This enables stateless architecture and improves scalability.

---

### Third-Party Authentication (OAuth Providers)

Authentication can be delegated to trusted providers such as Google or GitHub.

The application trusts the identity verification performed by the provider, reducing the need to manage passwords directly.

---

## What Happens After Authentication?

After successful authentication:

- The system recognizes the requester.
    
- User details become available to the application.
    
- Authorization checks can now be performed.
    

Authentication alone does not grant full access. It only establishes identity.

---

# What is Authorization?

Authorization is the process of determining what an authenticated user is allowed to do within the system.

It answers the question:

**“What actions or resources is this user permitted to access?”**

Even if a user is authenticated, they may not have permission to perform certain operations.

---

## How Authorization Works

Authorization evaluates permissions using roles, privileges, or policies.

Typical flow:

1. An authenticated user attempts to access a resource.
    
2. The system checks the user’s assigned roles or authorities.
    
3. Permissions are evaluated.
    
4. Access is either granted or denied.
    

---

## Example Scenario

Consider an application with two roles:

- USER
    
- ADMIN
    

Both users can successfully log in.

However:

- A USER can view data.
    
- An ADMIN can delete data.
    

If a USER attempts to delete records, the system denies the request even though authentication succeeded.

This demonstrates that authentication and authorization are separate controls.

---

## Role-Based Access Control (RBAC)

One of the most common authorization models is Role-Based Access Control.

Users are assigned roles, and roles define permissions.

Example:

|Role|Permissions|
|---|---|
|USER|Read-only access|
|ADMIN|Full access|
|MANAGER|Approve operations|

Frameworks such as Spring Security commonly enforce authorization using roles at the endpoint or method level.

---

## Where Authorization is Applied

Authorization can be enforced at multiple layers of an application:

### URL Level

Restrict access to specific endpoints based on roles.

Example concept:  
Only administrators can access `/admin/**`.

---

### Method Level

Restrict execution of specific service or controller methods using security annotations.

---

### Object Level (Advanced)

Restrict access based on ownership.

For example:  
A user can modify only their own profile, not someone else's.

---

# Key Differences Between Authentication and Authorization

|Aspect|Authentication|Authorization|
|---|---|---|
|Purpose|Verify identity|Determine permissions|
|Main Question|Who are you?|What can you do?|
|Occurs When|First|After authentication|
|Failure Result|Access denied immediately|Access denied to resource|
|Example|Logging in|Accessing an admin endpoint|

---

# Order of Execution

Authentication always happens before authorization.

Correct sequence:

```
Request
 → Authentication
 → Authorization
 → Resource Access
```

The system cannot evaluate permissions without first knowing the identity of the requester.

---

# Real-World Analogy

Consider airport security:

- Authentication is showing your passport to confirm your identity.
    
- Authorization is your boarding pass determining which flight you are allowed to board.
    

Having a valid passport does not grant access to every flight.

---

# How They Work Together in a Secure System

When a request enters a secured backend:

1. Security mechanisms verify the identity (authentication).
    
2. The system stores the authenticated user.
    
3. Permissions are evaluated (authorization).
    
4. Access is granted only if both checks succeed.
    

This layered approach significantly strengthens application security.

---

# Common Developer Mistakes

### Treating Login as Complete Security

Authentication alone is not sufficient. Authorization must always be enforced.

---

### Hardcoding Permission Checks

Manually checking roles inside controllers leads to poor maintainability and scattered security logic.

Centralized security is preferred.

---

### Granting Excessive Permissions

Systems should follow the principle of least privilege — users should receive only the permissions necessary to perform their tasks.

---

# Why This Distinction is Architecturally Important

Separating authentication from authorization provides:

- Cleaner system design
    
- Flexible permission management
    
- Better scalability
    
- Stronger protection against misuse
    

It also allows integration with external identity providers without rewriting authorization rules.

---

# Interview-Ready Summary

Authentication is the process of verifying the identity of a user or system, while authorization determines what actions that authenticated entity is permitted to perform. Authentication always occurs before authorization, ensuring that permissions are evaluated only after identity has been established.

---

# Memory Anchor

Authentication establishes identity.  
Authorization grants or denies access.