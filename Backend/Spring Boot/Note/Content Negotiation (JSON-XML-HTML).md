## 1. What is Content Negotiation?

Content negotiation is the mechanism that allows a server to:

> Return the **same resource in different formats** based on what the client requests.

Example:

```
GET /users/10
```

Can return:

- JSON (most common)
    
- XML
    
- HTML (rare in REST APIs)
    

The **resource is the same**, only the representation changes.

---

## 2. Why It Exists

Different clients prefer different formats.

Examples:

- Web apps → JSON
    
- Legacy enterprise systems → XML
    
- Browsers → HTML
    
- Mobile apps → JSON
    

Content negotiation ensures compatibility without creating multiple endpoints.

---

## 3. How the Client Requests a Format

Primarily through the **Accept header**.

### Example Request

```
GET /users/10
Accept: application/json
```

Server returns JSON.

---

Another example:

```
Accept: application/xml
```

Server returns XML (if supported).

---

## 4. How Spring Chooses the Format

Spring checks:

```
1. Accept header
2. Supported message converters
3. Controller "produces" setting
```

Then selects the best match.

If no match exists:

```
406 Not Acceptable
```

---

## 5. Default Behavior in Spring Boot

Out of the box:

👉 JSON is the default response format.

Why?

Because the Jackson library is auto-configured.

Most modern APIs rely exclusively on JSON.

---

## 6. Producing Specific Formats

You can explicitly declare supported formats.

### JSON Only

```java
@GetMapping(value="/users/{id}",
            produces="application/json")
```

---

### XML Only

```java
@GetMapping(value="/users/{id}",
            produces="application/xml")
```

Requires an XML serializer dependency.

---

### Multiple Formats

```java
@GetMapping(
    value="/users/{id}",
    produces = {"application/json", "application/xml"}
)
```

Spring decides based on the Accept header.

---

## 7. Message Converters — The Engine Behind It

Content negotiation works through **HTTP Message Converters**.

They transform:

```
Java Object ⇄ HTTP Response Format
```

### Example Converters

|Converter|Format|
|---|---|
|Jackson|JSON|
|Jackson XML|XML|
|String converter|Plain text|
|ByteArray|Files|

You usually don’t configure these manually.

Spring auto-registers them.

---

## 8. Returning HTML (Less Common)

Typical REST APIs don’t return HTML.

But MVC apps can.

Example:

```java
@GetMapping(value="/home",
            produces="text/html")
```

Used in server-side rendered apps.

Not common in microservices.

---

## 9. What Happens If Client Sends No Accept Header?

Spring defaults to:

```
application/json
```

This is why most APIs return JSON automatically.

---

## 10. Handling Unsupported Formats

If the client asks for something unsupported:

```
Accept: application/pdf
```

Spring returns:

```
406 Not Acceptable
```

Correct REST behavior.

---

## 11. Can Clients Force a Format via URL?

Older approach:

```
/users/10.xml
/users/10.json
```

Called **path extension negotiation**.

Modern APIs discourage this because:

- Security concerns
    
- Less flexible
    
- Harder to maintain
    

Accept header is preferred.

---

## 12. Should You Support XML Today?

### Usually NO.

JSON dominates modern backend systems because it is:

- Lightweight
    
- Faster to parse
    
- JavaScript-friendly
    
- Human-readable
    

Support XML only when:

- Integrating with legacy systems
    
- Working in banking / telecom
    
- Required by enterprise clients
    

Otherwise, JSON is enough.

---

## 13. Common Production Strategy

Most companies enforce:

```
application/json only
```

This simplifies:

- Testing
    
- Documentation
    
- Client SDKs
    
- Maintenance
    

Over-supporting formats increases complexity.

---

## 14. Content Negotiation vs Produces

### Accept Header → Client preference

### Produces → Server capability

Spring matches both.

Think:

```
Client asks → Server confirms → Format selected
```

---

## 15. Frequent Developer Mistakes

### Supporting Too Many Formats

Creates unnecessary maintenance overhead.

---

### Ignoring Accept Header

Breaks REST standards.

---

### Returning Different Structures per Format

The structure should remain consistent.

Only representation changes.

---

## 16. Interview-Favorite Questions

### What is content negotiation?

The process of selecting a response format based on client preference.

---

### Which header is used?

Accept header.

---

### Default format in Spring Boot?

JSON.

---

### What happens if no compatible format exists?

406 Not Acceptable.

---

### Should modern APIs support XML?

Only when required.

---

## Quick Memory Summary

```
Client → Accept header
Server → Message converter
Result → Proper format
```

### One-Line Rule:

> Same resource, multiple representations.

---

## Final Takeaway

Content negotiation ensures your API can communicate with diverse clients while keeping endpoints clean.

Understanding it signals:

- Strong HTTP knowledge
    
- Awareness of serialization
    
- Professional API design
    

### Professional Guideline:

> Default to JSON. Support additional formats only when there is a real business need.