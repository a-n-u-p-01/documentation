Handling request limits is essential for **API stability and security**.

Without limits, a single bad request can:

- Exhaust memory
    
- Block threads
    
- Slow down the server
    
- Cause outages
    

Production APIs always enforce these safeguards.

---

# 1. What is Request Size Limiting?

Request size limiting restricts how large an incoming request can be.

Example threats:

- Uploading a 5GB file
    
- Massive JSON payloads
    
- Malicious memory attacks
    

Goal:

> Protect server resources from overload.

---

## Default Behavior

Spring Boot relies on the embedded server (usually Tomcat) to enforce limits.

But you SHOULD configure them explicitly.

Never depend on defaults.

---

# 2. Limiting File Upload Size

Configure in **application.properties**:

```properties
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```

### Difference:

|Property|Controls|
|---|---|
|max-file-size|Size per file|
|max-request-size|Total request size|

Example:

```
5 files × 3MB = 15MB
```

Will fail if request limit is 10MB.

---

## What Happens When Limit Exceeds?

Spring throws:

```
MaxUploadSizeExceededException
```

Should be handled globally for clean API responses.

Return:

```
413 Payload Too Large
```

Correct HTTP behavior.

---

# 3. Limiting JSON / Normal Request Size

Multipart limits only affect file uploads.

To control overall request size, configure the server.

---

## Tomcat Configuration

```properties
server.tomcat.max-http-form-post-size=2MB
```

Restricts POST body size.

---

## Another Important Setting

```properties
server.tomcat.max-swallow-size=2MB
```

Prevents Tomcat from silently consuming huge failed uploads.

Good security measure.

---

# 4. What is Timeout Handling?

Timeout defines:

> How long the server waits before terminating a slow request.

Prevents threads from being stuck forever.

Example issues:

- Slow clients
    
- Network stalls
    
- Hanging database calls
    
- Infinite loops
    

Without timeouts → thread pool exhaustion.

Server stops responding.

---

# 5. Configure Server Timeout

## Tomcat Connection Timeout

```properties
server.connection-timeout=5s
```

If the client doesn’t send data in time → connection closes.

---

## Async Request Timeout

For long operations:

```properties
spring.mvc.async.request-timeout=30s
```

If exceeded → request terminated.

Useful for:

- Report generation
    
- External API calls
    
- File processing
    

---

# 6. Timeout at Reverse Proxy (Very Important)

In production, APIs usually sit behind:

- NGINX
    
- API Gateway
    
- Load balancers
    

Timeout must be configured there too.

Otherwise:

Proxy may timeout BEFORE your app.

Causing confusing failures.

---

# 7. Why Size + Timeout Together Matter

These protect two different resources:

### Size → Memory

### Timeout → Threads

Both are critical.

Protecting one but ignoring the other is dangerous.

---

# 8. Recommended Production Limits

(Varies by system, but good starting points.)

### JSON APIs:

```
1MB – 5MB
```

### File Upload:

```
10MB – 50MB
```

### Timeout:

```
5–30 seconds
```

Always tune based on workload.

---

# 9. Handling Large Payload APIs (Advanced Insight)

If clients must send huge data:

Avoid single massive requests.

Prefer:

### Chunking

Split into smaller uploads.

---

### Streaming

Process data gradually instead of loading into memory.

---

### Pre-Signed URLs (Industry Standard)

Upload directly to cloud storage.

Your API never touches the file.

Massive scalability gain.

---

# 10. Common Developer Mistakes

### No Request Limits

Invites denial-of-service attacks.

---

### Very High Limits

Large payloads can crash JVM memory.

---

### Ignoring Proxy Timeouts

Creates inconsistent behavior.

---

### Blocking Calls Without Timeout

Threads get stuck.

---

# 11. Error Codes You Should Return

|Scenario|Status|
|---|---|
|Payload too large|413|
|Request timeout|408|
|Gateway timeout|504|

Returning proper codes improves client retry logic.

---

# 12. High-Probability Interview Questions

### Why limit request size?

To protect memory and prevent abuse.

---

### What happens if a request exceeds the size?

413 Payload Too Large.

---

### Why are timeouts important?

To prevent thread exhaustion.

---

### Should limits be configured only in Spring?

No — also at gateway/load balancer.

---

### What resource does timeout protect?

Threads.

---

# Quick Memory Summary

```
Size limit → Protect memory
Timeout → Protect threads
Both → Protect server
```

### Golden Rule:

> Reject oversized or slow requests early before they damage system health.

---

# Final Takeaway

Request limits are part of **defensive backend design**.

Knowing this signals:

- Production awareness
    
- Reliability mindset
    
- Security thinking
    

### Professional Guideline:

> Set strict limits, fail fast, and protect server resources proactively.