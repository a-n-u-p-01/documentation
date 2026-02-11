Pagination and sorting are mandatory for scalable backend systems.

If an API loads very large datasets at once, it can lead to:

- Memory exhaustion
    
- Slow database queries
    
- Increased response time
    
- Application instability
    

Professional backend systems always paginate data.

Interviewers frequently ask this topic because it reveals whether you understand real-world system behavior.

---

# 1. What is Pagination?

Pagination is the practice of dividing large datasets into smaller chunks called pages.

Instead of returning everything:

```
GET /users
```

You return:

```
GET /users?page=0&size=10
```

Only 10 records are fetched.

This improves performance and keeps responses predictable.

---

# 2. How Pagination Works Internally

Most databases implement pagination using:

```
LIMIT + OFFSET
```

Example SQL:

```sql
SELECT * FROM users
LIMIT 10 OFFSET 20;
```

Meaning:

- Skip first 20 rows
    
- Return next 10
    

Hibernate generates this automatically when using Spring Data JPA.

---

# 3. Pageable — Core Interface

Spring provides the `Pageable` interface to define pagination parameters.

## Repository Example

```java
Page<User> findAll(Pageable pageable);
```

---

## Creating a Pageable

```java
Pageable pageable = PageRequest.of(0, 10);
```

Where:

- First value = page number
    
- Second value = page size
    

Important: Pagination is zero-based.

Page 0 is the first page.

This is a common interview trap.

---

# 4. Understanding the Page Object

When you paginate, you usually receive a `Page<T>` instead of a simple list.

Example:

```java
Page<User> page = userRepository.findAll(pageable);
List<User> users = page.getContent();
```

## Page Provides Metadata:

- Total number of pages
    
- Total number of records
    
- Current page
    
- Page size
    
- Whether it is the last page
    

This metadata is extremely useful for frontend pagination.

---

# 5. Sorting Basics

Sorting defines the order of returned data.

Example:

```java
Sort sort = Sort.by("name").ascending();
```

Descending order:

```java
Sort.by("createdAt").descending();
```

Sorting is executed at the database level, which is far more efficient than sorting in application memory.

---

# 6. Combining Pagination and Sorting

This is a very common production pattern.

```java
Pageable pageable =
        PageRequest.of(
            0,
            10,
            Sort.by("salary").descending()
        );
```

This retrieves the highest salaries first while limiting results.

---

# 7. Sorting by Multiple Fields

```java
Sort sort = Sort.by("department")
                .and(Sort.by("salary").descending());
```

This sorts:

- First by department
    
- Then by salary within each department
    

Useful for dashboards and reporting systems.

---

# 8. Page vs Slice (Important Interview Topic)

## Page

`Page` triggers an additional query:

```sql
SELECT COUNT(*)
```

This calculates total records.

Use it when the UI needs page numbers.

---

## Slice

`Slice` does NOT run a count query.

It only checks whether another page exists.

Benefits:

- Faster queries
    
- Less database load
    

Best suited for:

- Infinite scrolling
    
- Streaming data
    

Strong interview statement:

Prefer Slice when total count is unnecessary to improve performance.

---

# 9. Pagination with Custom Queries

Pagination works with JPQL as well.

Example:

```java
@Query("SELECT u FROM User u WHERE u.status='ACTIVE'")
Page<User> findActiveUsers(Pageable pageable);
```

Spring automatically applies LIMIT and OFFSET.

---

# 10. Best Practices (Production Level)

## Always Paginate Large Tables

Never expose unrestricted `findAll()` endpoints.

---

## Keep Page Size Reasonable

Typical values:

- 10
    
- 20
    
- 50
    
- 100
    

Very large page sizes defeat pagination benefits.

---

## Prefer Database Sorting

Avoid sorting in Java memory.

---

## Index Frequently Sorted Columns

For example:

- createdAt
    
- email
    
- status
    

Sorting without indexes can become very slow.

---

## Return DTOs Instead of Entities

Reduces payload size and improves response speed.

---

# 11. Common Developer Mistakes

Using `findAll()` in production APIs  
Leads to memory problems.

Ignoring indexes  
Pagination is fast only if the database can locate rows quickly.

Using extremely large page sizes  
This shifts the bottleneck instead of solving it.

Forgetting default pagination  
Always enforce limits on public APIs.

---

# 12. High-Probability Interview Questions

What is pagination?  
Fetching data in smaller chunks to improve performance.

Why is pagination important?  
Prevents loading massive datasets into memory.

What is the difference between Page and Slice?  
Page provides total count; Slice does not.

Is pagination zero-based?  
Yes.

Where should sorting happen?  
In the database.

---

# Quick Memory Summary

```
Pageable → defines page
Page → provides metadata
Slice → avoids count query
Sort → database ordering
```

Golden rule:

Design APIs assuming data will grow large. Pagination is not an optimization — it is a necessity.

---

# Final Takeaway

Pagination and sorting are fundamental to building scalable APIs.

Understanding them signals:

- Production readiness
    
- Performance awareness
    
- Database maturity
    

Professional guideline:

Never design an endpoint that assumes the dataset will remain small.