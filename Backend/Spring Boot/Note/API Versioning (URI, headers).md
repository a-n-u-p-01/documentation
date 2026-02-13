## 1. What is API Versioning?

API versioning is the practice of managing changes to an API **without breaking existing clients**.

As applications evolve, APIs change due to:

- New fields
    
- Removed fields
    
- Behavior changes
    
- Performance improvements
    
- Security updates
    

Versioning ensures:

> Old clients continue working while new clients use the updated API.

This is critical in production systems.

---

## 2. Why Versioning is Necessary

Imagine releasing:

```
GET /users
```

Mobile apps integrate with it.

Later you change the response structure.

Without versioning → older apps crash.

Production rule:

> Never break client integrations.

Versioning protects backward compatibility.

---

## 3. When Should You Version an API?

Version ONLY when you introduce **breaking changes**.

### Breaking Change Examples

- Removing a field
    
- Renaming properties
    
- Changing response format
    
- Modifying authentication
    
- Changing status codes
    

### Non-Breaking Changes

Usually safe:

- Adding new optional fields
    
- Improving performance
    
- Internal refactoring
    

Avoid versioning too frequently.

---

# 4. Most Popular Versioning Strategies

There are multiple approaches, but two dominate interviews and real systems:

## URI Versioning (Most Common)

## Header Versioning (More Advanced)

---

# URI Versioning

## What It Looks Like

```
/api/v1/users
/api/v2/users
```

Version becomes part of the URL.

---

## Example in Spring

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserControllerV1 {
}
```

New version:

```java
@RequestMapping("/api/v2/users")
```

Simple and explicit.

---

## Advantages

### Extremely Clear

Developers instantly know which version they use.

---

### Easy to Test

Just change the URL.

---

### Works Everywhere

- Browsers
    
- Postman
    
- Gateways
    
- Proxies
    

No special headers needed.

---

### Easy Caching

Different URLs cache independently.

---

## Disadvantages

### URL Pollution

Endpoints grow:

```
/v1/
/v2/
/v3/
```

---

### Slightly Less REST-Pure

Some architects argue versions should not be in resource paths.

(But most companies still use it.)

---

## Industry Reality

👉 URI versioning is the **most widely used approach**.

Especially in:

- Startups
    
- Microservices
    
- Public APIs
    

If unsure → choose URI.

---

# Header Versioning

Instead of changing the URL, version is sent via headers.

---

## Example Request

```
GET /users
API-Version: 2
```

or

```
Accept: application/vnd.company.v2+json
```

---

## Spring Example

```java
@GetMapping(
    value="/users",
    headers="API-Version=2"
)
```

Spring routes based on the header.

---

## Advantages

### Cleaner URLs

```
/users
```

Never changes.

---

### More RESTful

Resources remain stable.

Only representation evolves.

---

### Better for Long-Term API Evolution

Used in mature platforms.

---

## Disadvantages

### Harder to Test

Browsers don’t easily send custom headers.

---

### Less Discoverable

Developers cannot see the version in the URL.

---

### Debugging Becomes Harder

Requires inspecting headers.

---

## Where Header Versioning is Common

- Large enterprises
    
- Public platforms
    
- API-first companies
    

Examples include major tech platforms.

---

# URI vs Header — Quick Comparison

|Feature|URI|Header|
|---|---|---|
|Simplicity|Very easy|Moderate|
|Visibility|High|Low|
|REST purity|Lower|Higher|
|Testing|Easy|Harder|
|Industry usage|Very high|Growing|

---

# Which Should You Choose?

## For Most Systems:

👉 URI versioning is the safest choice.

## For Large, Mature APIs:

👉 Header versioning scales better.

---

# 5. Best Practices for Versioning

## Never Delete Old Versions Immediately

Give clients time to migrate.

Common practice:

- Deprecate
    
- Announce
    
- Sunset later
    

---

## Avoid Too Many Versions

Maintaining many versions increases complexity.

Try designing flexible APIs early.

---

## Document Version Changes Clearly

Clients must know what changed.

Use changelogs.

---

## Prefer Backward Compatibility

Add optional fields instead of breaking contracts.

---

# 6. Alternative Strategy (Good to Know)

## Media-Type Versioning

```
Accept: application/vnd.company.v2+json
```

Very RESTful but more complex.

Mostly seen in large-scale platforms.

Not required knowledge for beginners but impressive in interviews.

---

# 7. Common Developer Mistakes

### Versioning Too Late

Add versioning early in production APIs.

---

### Breaking APIs Without Version Bump

Destroys client trust.

---

### Maintaining Too Many Versions

Creates operational overhead.

---

### Forgetting Deprecation Strategy

Old APIs should not live forever.

---

# 8. High-Probability Interview Questions

### What is API versioning?

A strategy to evolve APIs without breaking existing clients.

---

### Most common versioning method?

URI versioning.

---

### Which is more RESTful?

Header versioning.

---

### When should you version?

When introducing breaking changes.

---

### Should you version from day one?

For production APIs — yes.

---

# Memory Shortcut

```
URI → Visible & Simple
Header → Clean & Flexible
```

### Golden Rule:

> Protect clients from breaking changes.

---

# Final Takeaway

API versioning is a hallmark of **professional backend design**.

It demonstrates:

- Long-term thinking
    
- Client awareness
    
- Stability mindset
    
- Production maturity
    

### Professional Guideline:

> Version early, avoid breaking changes, and treat your API as a long-term contract.