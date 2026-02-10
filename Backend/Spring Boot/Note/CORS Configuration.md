## 1. What is CORS?

CORS stands for:

> **Cross-Origin Resource Sharing**

It is a browser security mechanism that controls **how web pages from one origin can request resources from another origin.**

---

## 2. What is an Origin?

An origin is defined by three things:

```
Protocol + Domain + Port
```

Example:

```
http://example.com:8080
```

If ANY of these change → it becomes a different origin.

### Example Cross-Origin Request

Frontend:

```
http://localhost:3000
```

Backend:

```
http://localhost:8080
```

Browser blocks the request by default.

Not because of Spring — but because of browser security rules.

---

## 3. Why Browsers Block Cross-Origin Requests

To prevent attacks such as:

- Data theft
    
- Unauthorized API calls
    
- Credential hijacking
    
- CSRF attacks
    

Without CORS, malicious websites could silently call your APIs.

---

## 4. How CORS Works

When a cross-origin request is made, the browser first checks:

> “Does the server allow this origin?”

The server must respond with special headers.

Example:

```
Access-Control-Allow-Origin: http://localhost:3000
```

If not present → browser blocks the response.

Important:

👉 The request reaches the server.  
👉 The browser blocks the response.

---

## 5. Enabling CORS in Spring Boot

There are multiple ways — each suited for different use cases.

---

# Method 1 — @CrossOrigin (Quick & Simple)

Apply directly on a controller.

```java
@CrossOrigin(origins = "http://localhost:3000")
@RestController
public class UserController {
}
```

Now requests from that origin are allowed.

---

## Allow Multiple Origins

```java
@CrossOrigin(origins = {
    "http://localhost:3000",
    "https://myapp.com"
})
```

---

## Allow All Origins (NOT Recommended for Production)

```java
@CrossOrigin(origins = "*")
```

Security risk.

Avoid unless building a public API.

---

# Method 2 — Global CORS Configuration (Recommended)

Best for production systems.

Create a configuration class:

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {

        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*");
    }
}
```

Now all matching endpoints support CORS.

Centralized and clean.

---

# 6. Preflight Requests (VERY IMPORTANT)

For complex requests, browsers send a **preflight request** first.

It uses:

```
OPTIONS
```

Purpose:

> “Server, are you okay with this request?”

Only if approved → real request is sent.

---

## When Preflight Happens

- Non-GET/POST methods
    
- Custom headers
    
- Authorization headers
    
- JSON content type
    

Very common in modern APIs.

---

## Required Headers in Response

```
Access-Control-Allow-Origin
Access-Control-Allow-Methods
Access-Control-Allow-Headers
```

Spring usually handles this automatically once configured.

---

# 7. Important CORS Settings

## allowedOrigins

Specifies which domains can access your API.

Never blindly allow all.

---

## allowedMethods

Restrict HTTP methods.

Example:

```java
.allowedMethods("GET", "POST")
```

Improves security.

---

## allowedHeaders

Defines acceptable headers.

Useful for:

- Authorization
    
- Custom tokens
    

---

## allowCredentials

Allows cookies/auth headers.

```java
.allowCredentials(true)
```

⚠️ Cannot use `"*"` origin with credentials.

Security rule.

---

# 8. CORS with Spring Security (Common Trap)

If Spring Security is enabled, CORS must be configured there too.

Otherwise, requests still fail.

Example:

```java
http.cors();
```

Interview favorite mistake.

---

# 9. Local Development Scenario

Most common use case:

Frontend → React/Angular on port 3000  
Backend → Spring Boot on 8080

Enable CORS for local testing.

Remove broad permissions before production.

---

# 10. Security Best Practices

## Never Use "*" in Production

Allows any website to call your APIs.

Huge risk.

---

## Restrict Origins

Allow only trusted domains.

---

## Limit Methods

Do not expose unnecessary operations.

---

## Avoid Allowing Credentials Broadly

Cookies + wide origins = vulnerability.

---

# 11. Common Developer Mistakes

### Thinking CORS is a Server Security Feature

It is enforced by browsers.

Postman ignores CORS.

---

### Over-Permissive Configuration

Convenient but dangerous.

---

### Forgetting Preflight Support

Leads to confusing failures.

---

### Not Configuring with Spring Security

Requests still blocked.

---

# 12. High-Probability Interview Questions

### What is CORS?

A browser security feature that controls cross-origin HTTP requests.

---

### Why does CORS exist?

To prevent malicious websites from accessing APIs.

---

### What is a preflight request?

An OPTIONS request sent before the real request to verify permissions.

---

### Can Postman fail due to CORS?

No — browsers enforce it.

---

### Best way to configure CORS?

Globally via WebMvcConfigurer.

---

# Quick Memory Summary

```
Different Origin → Browser Blocks
Server Must Allow → CORS Headers
OPTIONS → Preflight Check
```

### Golden Rule:

> Allow only trusted origins — never open your API to the world without reason.

---

# Final Takeaway

CORS is not just configuration — it is part of your API’s **security posture**.

Understanding it signals:

- Strong web fundamentals
    
- Security awareness
    
- Production readiness
    

### Professional Guideline:

> Be restrictive by default. Open access only when required.