Authentication flow describes the step-by-step process through which Spring Security verifies a user's identity before allowing access to protected resources. Understanding this flow is extremely important because it connects all major security components such as filters, authentication managers, user details, and the security context.

Many developers configure Spring Security successfully but struggle to explain how authentication actually happens internally. Once you understand this flow, Spring Security stops feeling complex and begins to look like a structured pipeline.

---

## What is Authentication Flow?

Authentication flow is the sequence of operations performed when a request requiring identity verification enters a secured application.

Its goal is simple:

- Identify the requester
    
- Verify credentials
    
- Create an authenticated identity
    
- Store it securely
    
- Allow the request to continue
    

If any step fails, the request is rejected immediately.

---

## High-Level Authentication Pipeline

When a secured request enters a Spring application, it typically follows this path:

```
Client Request
      ↓
Security Filter Chain
      ↓
Authentication Filter
      ↓
Authentication Manager
      ↓
Authentication Provider
      ↓
UserDetailsService (Fetch User)
      ↓
Password Validation
      ↓
Create Authentication Object
      ↓
Store in SecurityContext
      ↓
Proceed to Authorization
      ↓
Controller
```

This entire process usually takes only a few milliseconds.

---

## Step-by-Step Explanation

### 1. Request Enters the Security Filter Chain

Every incoming request first passes through Spring Security filters.

The system determines:

- Is this endpoint protected?
    
- Does authentication need to occur?
    

If the endpoint is public, the request proceeds without authentication.

If protected, the authentication process begins.

---

### 2. Authentication Filter Extracts Credentials

A specific filter reads authentication data from the request.

Examples:

- Username and password from a login form
    
- JWT token from the Authorization header
    
- Basic authentication header
    

The filter does not validate credentials itself.  
It delegates the job.

---

### 3. Authentication Manager Takes Control

The AuthenticationManager is responsible for coordinating the authentication process.

Think of it as a dispatcher.

Its job is to:

- Receive the authentication request
    
- Pass it to the correct AuthenticationProvider
    

It does not verify credentials directly.

---

### 4. Authentication Provider Performs Verification

The AuthenticationProvider contains the actual authentication logic.

It validates:

- Username existence
    
- Password correctness
    
- Token validity
    

If authentication fails, an exception is thrown and the request stops.

If successful, it returns a fully authenticated object.

---

### 5. UserDetailsService Loads User Data

For username/password authentication, the provider typically calls UserDetailsService.

Its responsibility:

- Fetch user data from the database
    
- Provide roles/authorities
    
- Return a UserDetails object
    

This object becomes the foundation for authentication.

---

### 6. Password Validation Occurs

The entered password is compared with the stored hashed password using a password encoder such as BCrypt.

Important principle:

Passwords are never decrypted.  
They are hashed and compared securely.

If the passwords do not match, authentication fails immediately.

---

### 7. Authentication Object is Created

Once verification succeeds, Spring creates an Authentication object containing:

- Principal (user identity)
    
- Credentials
    
- Authorities (roles/permissions)
    
- Authentication status
    

This object represents the logged-in user.

---

### 8. SecurityContext Stores the Authenticated User

The authenticated object is stored inside the SecurityContext.

This is critical because it allows the application to:

- Access the current user anywhere
    
- Perform authorization checks
    
- Apply method-level security
    

Developers usually do not manage this manually.

Spring handles it automatically.

---

### 9. Authorization Begins

Now that identity is confirmed, Spring checks whether the user has permission to access the requested resource.

If authorized → request proceeds.  
If not → access is denied.

Authentication always precedes authorization.

---

## Example: Username and Password Login

Consider a login request:

```
POST /login
username: alex
password: ********
```

Flow:

1. Filter extracts credentials.
    
2. AuthenticationManager delegates verification.
    
3. UserDetailsService loads the user.
    
4. Password is validated.
    
5. Authentication object is created.
    
6. SecurityContext stores the user.
    
7. User is now authenticated.
    

Subsequent requests can rely on this identity.

---

## Example: JWT Authentication Flow

For token-based systems:

```
GET /orders
Authorization: Bearer <token>
```

Flow:

1. Filter extracts token.
    
2. Token is validated (signature, expiration).
    
3. User information is retrieved.
    
4. Authentication object is created.
    
5. SecurityContext is populated.
    
6. Authorization checks run.
    

No password is involved after login.

This supports stateless architecture.

---

## Stateless vs Stateful Authentication Flow

### Stateful (Session-Based)

- Server stores authentication in a session.
    
- Client sends a session ID with each request.
    

Common in traditional web apps.

---

### Stateless (Token-Based)

- Server stores nothing.
    
- Every request carries authentication data.
    

Preferred for REST APIs because it scales better.

---

## What Happens When Authentication Fails?

The system immediately stops processing the request.

Common responses:

- 401 Unauthorized — identity could not be verified
    
- 403 Forbidden — authenticated but not permitted
    

The request never reaches the controller.

---

## Why Understanding Authentication Flow Matters

Knowing the flow helps you:

- Debug security issues
    
- Configure security correctly
    
- Understand where failures occur
    
- Design scalable authentication systems
    
- Perform well in interviews
    

Most confusion around Spring Security comes from not understanding this pipeline.

---

## Common Developer Mistakes

### Treating Authentication as a Single Step

It is actually a coordinated process involving filters, providers, and context storage.

---

### Not Understanding Delegation

Filters do not authenticate users directly.  
They pass responsibility to specialized components.

---

### Ignoring Password Encoding

Plain-text password comparison is insecure and unacceptable in production systems.

---

## Architectural Insight

Spring Security separates responsibilities across components:

- Filters handle interception
    
- Managers coordinate
    
- Providers verify
    
- Services fetch data
    
- Context stores identity
    

This separation improves flexibility, testability, and maintainability.

---

## Interview-Ready Summary

Authentication flow in Spring Security is the structured process through which incoming requests are intercepted by security filters, credentials are extracted and validated by authentication providers, and the authenticated identity is stored in the security context before authorization checks are applied.

---

## Memory Anchor

Filters intercept the request.  
Authentication verifies identity.  
SecurityContext stores the user.  
Authorization decides access.
