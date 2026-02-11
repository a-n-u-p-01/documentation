Derived queries are powerful — but they have limits.

When queries become complex, you must write them manually.

This is where **custom queries** come in.

Understanding this signals **serious backend maturity.**

Interviewers often escalate to this topic after derived queries.

---

# 1. What is a Custom Query?

A custom query is a manually written database query using:

👉 JPQL (preferred)  
👉 Native SQL (when necessary)

Defined using:

```
@Query
```

Example:

```java
@Query("SELECT u FROM User u WHERE u.email = :email")
Optional<User> findUserByEmail(String email);
```

---

# 2. Why Custom Queries Are Needed

Derived queries struggle with:

- Complex joins
    
- Aggregations
    
- Subqueries
    
- Grouping
    
- Performance tuning
    

Instead of forcing ugly method names, write the query directly.

### Senior Insight:

> Derived queries for simple logic — custom queries for complex logic.

---

# 3. JPQL — Java Persistence Query Language

## What is JPQL?

JPQL is **object-oriented SQL.**

You query:

👉 Entities  
NOT tables.

---

## Example

### JPQL:

```java
@Query("SELECT u FROM User u WHERE u.status = 'ACTIVE'")
List<User> findActiveUsers();
```

Notice:

```
User → Entity
```

NOT:

```
users → table
```

Hibernate converts JPQL → SQL.

---

## Key Advantage

Database independence.

Your query works across:

- MySQL
    
- PostgreSQL
    
- Oracle
    

Huge enterprise benefit.

---

# 4. Named Parameters (Best Practice)

Avoid positional parameters.

Bad:

```java
WHERE u.email = ?1
```

Good:

```java
WHERE u.email = :email
```

Example:

```java
@Query("SELECT u FROM User u WHERE u.email = :email")
Optional<User> findByEmail(@Param("email") String email);
```

More readable and safer.

---

# 5. Partial Column Fetching (Performance Boost)

Instead of loading entire entities:

Fetch only needed fields.

## Example:

```java
@Query("""
SELECT u.name 
FROM User u
WHERE u.status='ACTIVE'
""")
List<String> findActiveUserNames();
```

Less memory.  
Faster queries.

Professional optimization.

---

# 6. JOIN Queries (VERY IMPORTANT)

Most real systems require joins.

## Example:

```java
@Query("""
SELECT o
FROM Order o
JOIN o.customer c
WHERE c.email = :email
""")
List<Order> findOrdersByCustomerEmail(String email);
```

Hibernate builds the SQL join automatically.

---

# ⭐ Interview Gold Line:

> JPQL uses entity relationships instead of foreign keys.

Shows deep ORM understanding.

---

# 7. Aggregation Queries

Example:

```java
@Query("""
SELECT COUNT(u)
FROM User u
WHERE u.active = true
""")
long countActiveUsers();
```

Works exactly like SQL aggregation.

---

# 8. Updating with @Modifying

By default, queries are read-only.

For updates:

👉 Add `@Modifying`

```java
@Modifying
@Query("""
UPDATE User u
SET u.status='INACTIVE'
WHERE u.lastLogin < :date
""")
int deactivateOldUsers(LocalDate date);
```

Returns rows affected.

---

## CRITICAL:

Always combine with transaction support.

Otherwise update may fail.

---

# 9. Native SQL — When You Need Raw Power

Sometimes JPQL is not enough.

Use native SQL.

## Example:

```java
@Query(
 value = "SELECT * FROM users WHERE email = ?",
 nativeQuery = true
)
User findByEmailNative(String email);
```

Now you're writing real SQL.

---

# When Native SQL is Justified

## Complex Reporting Queries

Heavy joins, window functions.

---

## Database-Specific Features

Example:

- JSON columns
    
- CTEs
    
- Stored procedures
    

---

## Performance-Critical Queries

Hand-tuned SQL can outperform ORM.

---

# Tradeoff

Native SQL reduces portability.

Switching databases becomes harder.

Use wisely.

---

# 10. JPQL vs Native — Quick Comparison

|Feature|JPQL|Native SQL|
|---|---|---|
|Works on entities|✅|❌|
|DB independent|✅|❌|
|Uses ORM|✅|❌|
|Supports DB features|❌|✅|
|Easier maintenance|✅|⚠️|

---

# ⭐ Strong Interview Answer

If asked:

### "Which should you prefer?"

Say:

> Prefer JPQL for portability and ORM integration. Use native SQL only when necessary for performance or advanced database features.

Immediate senior signal.

---

# 11. Avoid This Massive Mistake

## Fetching Entire Entities When Not Needed

Bad:

```
SELECT u FROM User u
```

Good:

```
SELECT u.email
```

Reduces:

- Memory usage
    
- Network load
    
- Persistence context size
    

Performance mindset.

---

# 12. Common Developer Mistakes

### Overusing Native Queries

Kills portability.

---

### Forgetting @Modifying

Updates silently fail.

---

### Ignoring Indexes

Even perfect queries need DB tuning.

---

### Loading Huge Object Graphs

Triggers performance disasters.

---

# 13. Advanced Insight — Projection + Custom Query

You can return DTOs directly.

Example:

```java
@Query("""
SELECT new com.app.UserDTO(u.id, u.name)
FROM User u
""")
List<UserDTO> fetchUserDTOs();
```

No entity creation.

Extremely efficient.

Senior-level optimization.

---

# High-Probability Interview Questions

### What is JPQL?

An object-oriented query language operating on entities.

---

### Difference between JPQL and SQL?

JPQL uses entities; SQL uses tables.

---

### When use native queries?

When JPQL cannot handle complexity or performance needs.

---

### Why use @Modifying?

To enable update/delete queries.

---

### Which is preferred?

JPQL.

---

# Quick Memory Summary

```
Derived → Simple queries
JPQL → Complex ORM queries
Native → Raw DB power
```

### Golden Rule:

> Start with JPQL — drop to native only when required.

---

# Final Takeaway

Custom queries give you **precision control** over database operations.

Mastering them signals:

- Advanced persistence knowledge
    
- Query optimization awareness
    
- Production readiness
    

### Professional Guideline:

> Let ORM handle the common path — take manual control when performance or complexity demands it.

---

