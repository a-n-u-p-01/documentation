# 1. What is a Repository?

A repository is a **data access abstraction** that lets you interact with the database without writing boilerplate code.

Instead of writing DAO classes manually, you define an interface.

Spring automatically generates the implementation at runtime.

## Example

```java
public interface UserRepository 
        extends JpaRepository<User, Long> {
}
```

That’s it.

You instantly get:

- Save
    
- Delete
    
- Find
    
- Pagination
    
- Sorting
    

Zero implementation code.

Huge productivity boost.

---

# 2. Where Repository Fits in Architecture

Standard backend flow:

```
Controller
 ↓
Service
 ↓
Repository
 ↓
Hibernate
 ↓
Database
```

## Important Rule:

> Controllers should NEVER call repositories directly.

Always go through the service layer.

Interviewers watch for this.

---

# 3. CrudRepository — The Foundation

`CrudRepository` provides basic CRUD operations.

## Core Methods

```java
save(entity)
findById(id)
findAll()
deleteById(id)
existsById(id)
count()
```

Enough for simple applications.

---

## Why It Exists

It represents the **minimal contract** for persistence.

Other repositories extend it.

---

# 4. JpaRepository — The Industry Standard

Most real-world applications use:

> ✅ **JpaRepository**

Because it extends:

```
CrudRepository
    ↑
PagingAndSortingRepository
    ↑
JpaRepository
```

So you get everything — plus more.

---

# 5. What JpaRepository Adds

## Pagination (VERY Important)

```java
Page<User> users = repo.findAll(pageable);
```

Critical for large datasets.

Never load millions of rows.

---

## Sorting

```java
repo.findAll(Sort.by("name"));
```

Database handles ordering efficiently.

---

## Batch Operations

```java
saveAll(list)
deleteAll()
```

Better performance for bulk actions.

---

## Flush Control

```java
repo.flush();
```

Forces Hibernate to execute SQL immediately.

Useful in rare cases.

---

# ⭐ Interview Power Line

> JpaRepository should be your default choice unless you have a strong reason otherwise.

Memorize this.

---

# 6. Why Not Always CrudRepository?

Because modern apps need:

- Pagination
    
- Sorting
    
- Batch operations
    

CrudRepository is too minimal.

JpaRepository is production-ready.

---

# 7. Generic Parameters (Understand This)

```java
JpaRepository<Entity, ID>
```

Example:

```java
JpaRepository<User, Long>
```

Means:

```
User → Entity
Long → Primary Key type
```

Common mistake is mismatching ID type.

---

# 8. How Spring Generates Implementations

At startup, Spring creates a **proxy object** for the repository.

So when you call:

```java
userRepository.findById(1);
```

Spring delegates to Hibernate automatically.

You never see the implementation.

Shows the power of Spring proxies.

---

# 9. Should You Write Custom Repository Code?

Usually — NO.

Spring Data handles most cases.

Write custom logic only when:

- Query is very complex
    
- Needs optimization
    
- Requires native SQL
    

Avoid reinventing CRUD.

---

# 10. Repository Best Practices

## Keep Them Thin

Repositories should focus ONLY on persistence.

Avoid:

- Business logic
    
- Calculations
    
- External calls
    

That belongs in services.

---

## Return Optional for Single Fetch

```java
Optional<User> findById(Long id);
```

Prevents NullPointerExceptions.

Modern Java practice.

---

## Prefer Pagination Over findAll()

Bad:

```
findAll() → millions of rows
```

Always think about scale.

---

# 11. Transaction Awareness (Quick Insight)

Repositories participate in transactions automatically when called from transactional services.

You typically do NOT annotate repositories with transactions.

Service layer controls it.

Good architecture signal.

---

# 12. Common Developer Mistakes

### Injecting Repository Into Controller

Breaks layering.

---

### Writing Business Logic in Repository

Creates messy code.

---

### Overusing findAll()

Kills memory.

---

### Ignoring Pagination

Major scalability issue.

---

### Returning Entities Directly to API

Use DTOs instead.

---

# 13. CrudRepository vs JpaRepository — Quick Table

|Feature|CrudRepository|JpaRepository|
|---|---|---|
|Basic CRUD|✅|✅|
|Pagination|❌|✅|
|Sorting|❌|✅|
|Batch operations|❌|✅|
|Flush control|❌|✅|
|Production ready|⚠️ Limited|✅ Yes|

---

# ⭐ What Interviewers Secretly Want to Hear

If asked:

### “Which repository should you use?”

Strong answer:

> JpaRepository, because it provides pagination, sorting, and batch capabilities required in production systems.

Immediate positive signal.

---

# 14. Advanced Insight (High Value)

Repositories are interfaces — not classes — because Spring uses **dynamic proxies**.

This enables:

- Automatic query generation
    
- Transaction integration
    
- AOP support
    

You don’t need to implement anything.

Powerful abstraction.

---

# High-Probability Interview Questions

### What is JpaRepository?

An extension of CrudRepository that adds JPA-specific features like pagination and flushing.

---

### Should controllers access repositories?

No — use the service layer.

---

### Why is pagination important?

Prevents loading massive datasets into memory.

---

### What does save() do — insert or update?

Both.

Hibernate decides based on entity state.

(VERY common question.)

---

# Quick Memory Summary

```
CrudRepository → Basic CRUD
JpaRepository → Production-ready
```

### Golden Rule:

> Default to JpaRepository.

---

# Final Takeaway

Repositories eliminate boilerplate and let you focus on domain logic.

Mastering them signals:

- Professional Spring usage
    
- Clean layering
    
- Database awareness
    

### Professional Guideline:

> Keep repositories simple, push logic to services, and always design with scalability in mind.

---
