## 1. What is Rate Limiting?

Rate limiting is a technique used to **control how many requests a client can make within a specific time window.**

Example rule:

> “Allow 100 requests per minute per user.”

If exceeded → requests are rejected.

---

## 2. Why Rate Limiting is Critical

Without it, your API is vulnerable to:

- Traffic spikes
    
- Bot abuse
    
- Brute-force attacks
    
- Denial-of-Service (DoS)
    
- Resource exhaustion
    

Even a small API can crash if thousands of requests hit simultaneously.

**Production systems ALWAYS implement rate limiting.**

---

## 3. Where Rate Limiting is Applied

You can apply it at multiple layers:

|Layer|Common?|Notes|
|---|---|---|
|API Gateway|⭐⭐⭐⭐⭐|Best place|
|Load Balancer|⭐⭐⭐⭐|Infrastructure level|
|Controller|⭐⭐⭐|Good fallback|
|Service|⭐⭐|Rare|

### Industry Insight:

> Gateway-level limiting is preferred, but controller-level protection is still valuable.

---

## 4. What is Bucket4j?

Bucket4j is a **Java library** that implements rate limiting using the:

> **Token Bucket Algorithm**

It is:

- Lightweight
    
- Thread-safe
    
- High-performance
    
- Cloud-friendly
    

Widely used in Spring applications.

---

## 5. Token Bucket Algorithm (Understand This Clearly)

Imagine a bucket filled with tokens.

Each request consumes **one token**.

```
Bucket capacity = 10 tokens
Refill rate = 10 tokens per minute
```

### Flow:

```
Request arrives
 ↓
Token available? → Allow
No token? → Reject (429)
```

Tokens refill gradually.

This prevents sudden bursts from overwhelming the system.

---

## 6. Key Concepts

### Capacity

Maximum tokens in the bucket.

Controls burst traffic.

---

### Refill Rate

How quickly tokens regenerate.

Controls long-term usage.

---

### Consumption

Each request removes tokens.

---

### Bandwidth

Combination of capacity + refill strategy.

---

## 7. Adding Bucket4j to Spring Boot

### Maven Dependency

```xml
<dependency>
    <groupId>com.bucket4j</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>8.0.1</version>
</dependency>
```

---

## 8. Creating a Bucket

Example configuration:

```java
Bandwidth limit = Bandwidth.classic(
        10,
        Refill.greedy(10, Duration.ofMinutes(1))
);

Bucket bucket = Bucket.builder()
        .addLimit(limit)
        .build();
```

Meaning:

```
10 requests per minute allowed.
```

---

## 9. Applying Rate Limiting in a Controller

Basic example:

```java
@RestController
public class ApiController {

    private final Bucket bucket;

    public ApiController() {

        Bandwidth limit = Bandwidth.classic(
                5,
                Refill.greedy(5, Duration.ofMinutes(1))
        );

        this.bucket = Bucket.builder()
                .addLimit(limit)
                .build();
    }

    @GetMapping("/api/data")
    public ResponseEntity<String> getData() {

        if(bucket.tryConsume(1)) {
            return ResponseEntity.ok("Success");
        }

        return ResponseEntity.status(429)
                .body("Too many requests");
    }
}
```

---

## Important Status Code

```
429 Too Many Requests
```

Correct REST behavior.

Interview favorite.

---

## 10. Per-User Rate Limiting (More Realistic)

Instead of one global bucket, create buckets per client.

Example strategy:

```
Map<IP, Bucket>
```

Or:

```
Map<UserId, Bucket>
```

This prevents one user from blocking everyone.

---

## 11. Advanced Storage (Production Systems)

In-memory buckets reset when the app restarts.

For distributed systems use:

- Redis
    
- Hazelcast
    
- Ignite
    

This keeps limits consistent across multiple instances.

Very important in microservices.

---

## 12. Where Should You Prefer Rate Limiting?

### Best Place → API Gateway

Examples:

- Spring Cloud Gateway
    
- Kong
    
- NGINX
    
- AWS API Gateway
    

Why?

- Protects all services
    
- Centralized control
    
- Better scalability
    

Controller limiting is secondary protection.

---

## 13. Common Rate Limiting Strategies

### Fixed Window

Example:

```
100 requests per minute
```

Simple but can allow bursts at window edges.

---

### Sliding Window

More accurate.

Spreads requests evenly.

---

### Token Bucket (Best Balance)

Allows controlled bursts while limiting sustained traffic.

Most popular approach.

---

## 14. Security Benefits

Rate limiting helps prevent:

- Credential stuffing
    
- OTP abuse
    
- API scraping
    
- Spam attacks
    

It is part of modern API security.

---

## 15. Common Developer Mistakes

### Global Limit Only

One attacker can block all users.

Always prefer per-user/IP.

---

### Returning Wrong Status Code

Use:

```
429 Too Many Requests
```

Not 403 or 500.

---

### No Retry Guidance

Better APIs include headers like:

```
Retry-After: 60
```

Tells clients when to retry.

---

### Ignoring Distributed Systems

In-memory limits fail in multi-instance deployments.

---

## 16. Interview-Favorite Questions

### What is rate limiting?

Controlling request frequency to protect system resources.

---

### Which algorithm does Bucket4j use?

Token bucket.

---

### What status code is returned when limit exceeds?

---

### Where is rate limiting ideally implemented?

API Gateway.

---

### Why not rely only on controller limits?

They don’t protect the entire architecture.

---

## Quick Memory Summary

```
Bucket → Tokens → Requests consume tokens
No tokens → 429
```

### Golden Rule:

> Allow bursts, prevent abuse.

---

## Final Takeaway

Rate limiting is a hallmark of **production-grade API design**.

Knowing it signals:

- Security awareness
    
- Scalability thinking
    
- Infrastructure knowledge
    

### Professional Guideline:

> Apply limits close to the edge (gateway), enforce fairness per client, and always return proper HTTP signals.