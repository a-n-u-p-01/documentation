# 1. What is a Derived Query?

A derived query is a method where:

> The method name defines the database query.

Example:

```java
List<User> findByEmail(String email);
```

Spring interprets it as:

```sql
SELECT * FROM users WHERE email = ?
```

No manual query needed.

---

# 2. How It Works Internally

When the app starts:

Spring scans repository methods → parses names → generates queries → creates proxy implementations.

All at runtime.

You just call:

```java
userRepository.findByEmail("test@mail.com");
```

And it works.

Huge developer acceleration.

---

# 3. Basic Syntax (Must Memorize)

## Pattern:

```
findBy + FieldName
```

Example:

```java
findByName(String name)
```

Field name MUST match the entity property.

Not the column name — the Java field.

Very common mistake.

---

# 4. Multiple Conditions

Spring supports chaining.

```java
findByEmailAndStatus(String email, Status status);
```

Generates:

```sql
WHERE email=? AND status=?
```

---

## OR Condition

```java
findByNameOrEmail(String name, String email);
```

---

## Combined Example

```java
findByStatusAndAgeGreaterThan(Status status, int age);
```

Spring handles complexity easily.

---

# 5. Comparison Keywords (Very Important)

## Greater / Less

```java
findByAgeGreaterThan(int age)
findBySalaryLessThan(double salary)
```

---

## Between

```java
findByCreatedAtBetween(start, end)
```

Great for reports.

---

## Like / Contains

```java
findByNameContaining(String keyword)
```

SQL:

```
LIKE %keyword%
```

Useful for search.

---

## Starting / Ending With

```java
findByNameStartingWith("A")
findByEmailEndingWith("@gmail.com")
```

---

# 6. Null Checks

```java
findByDeletedIsNull()
findByEmailIsNotNull()
```

Helpful when working with soft deletes.

---

# 7. Boolean Queries

If entity has:

```java
private boolean active;
```

You can write:

```java
findByActiveTrue()
findByActiveFalse()
```

Clean and readable.

---

# 8. Sorting with Derived Queries

## Static Sorting

```java
findByStatusOrderByCreatedAtDesc()
```

Database handles sorting.

Efficient.

---

## Dynamic Sorting (Better)

```java
findByStatus(Status status, Sort sort);
```

Example:

```java
Sort.by("createdAt").descending();
```

More flexible.

---

# 9. Limiting Results

## Top / First

```java
findTop5ByOrderByScoreDesc()
```

Returns highest scoring rows.

Great for leaderboards.

---

# 10. Pagination (Production Must-Have)

Never fetch massive datasets.

Use:

```java
Page<User> findByStatus(Status status, Pageable pageable);
```

Database returns only a slice.

Critical for scalability.

---

# 11. Nested Property Queries (Advanced but Common)

If:

```
Order → Customer → Email
```

You can write:

```java
findByCustomerEmail(String email);
```

Spring automatically creates the JOIN.

Very powerful.

---

# 12. Return Types Matter

Spring supports multiple return types.

## List

```java
List<User>
```

Multiple results.

---

## Optional

```java
Optional<User>
```

Best for single fetch.

Avoid null.

---

## Page

Supports pagination metadata.

Preferred in APIs.

---

## Exists Query (VERY Efficient)

```java
boolean existsByEmail(String email);
```

Better than fetching the entire entity.

Performance-aware coding.

---

# 13. When Derived Queries Become Bad

Avoid method names like:

```
findByStatusAndTypeAndRegionAndCreatedAtBetweenAndActiveTrue...
```

Too long = unreadable.

At that point:

👉 Use custom queries.

Senior-level decision making.

---

# 14. Performance Insight

Derived queries are great, but:

> Always think about indexes.

If you search by:

```
email
```

Add a DB index.

Otherwise queries slow down.

ORM does NOT replace database tuning.

---

# 15. Common Developer Mistakes

### Using Column Names Instead of Fields

Spring reads Java fields.

Not DB columns.

---

### Ignoring Pagination

Leads to memory issues.

---

### Writing Extremely Long Methods

Hurts readability.

---

### Forgetting Indexes

Destroys performance.

---

# ⭐ Interview Power Statements

Drop these and you sound experienced:

> "Derived queries are ideal for simple conditions, but complex queries should use JPQL or native SQL."

And:

> "Method names must align with entity field names."

Strong signal.

---

# High-Probability Interview Questions

### What is a derived query?

A query generated from a repository method name.

---

### Does Spring parse column names?

No — entity fields.

---

### When should you avoid derived queries?

When method names become long or logic is complex.

---

### How do you limit results?

Top / First keywords.

---

### Best return type for single entity?

Optional.

---

# Quick Memory Summary

```
findBy → Query generator
And/Or → Combine conditions
Top → Limit
Pageable → Scale
existsBy → Fast check
```

### Golden Rule:

> Use derived queries for simplicity — switch to custom queries for complexity.

---

# Final Takeaway

Derived queries dramatically reduce boilerplate while keeping repositories clean.

Mastering them signals:

- Strong Spring Data skills
    
- Clean coding style
    
- Database awareness
    

### Professional Guideline:

> Keep method names readable, leverage pagination, and always think about query performance.

---

Say **“next”** — the next HIGH-impact topic is:

👉 **Custom Queries (JPQL vs Native SQL)**

Extremely important for interviews.