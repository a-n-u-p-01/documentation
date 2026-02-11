Indexing and query optimization are fundamental database skills. Even perfectly written application code cannot compensate for slow database queries.

Many backend performance problems are actually database problems.

Interviewers often ask this topic to evaluate whether you understand real-world scalability.

---

# 1. What is an Index?

An index is a special data structure that allows the database to locate rows quickly without scanning the entire table.

Think of it like a book index.

Instead of reading every page to find a topic, you jump directly to the correct page.

Core purpose:

Reduce data lookup time.

---

# 2. What Happens Without an Index?

If a table has 5 million rows and you search for one user by email:

Without an index, the database performs a **full table scan**.

```
Row 1 → check  
Row 2 → check  
Row 3 → check  
...
```

This is slow and consumes significant resources.

---

# 3. What Happens With an Index?

The database uses the index to jump directly to the correct row.

Search time drops dramatically.

This is why indexing is one of the biggest performance improvements you can make.

Strong interview statement:

Indexes convert expensive table scans into fast lookups.

---

# 4. How Databases Store Indexes

Most databases use a structure similar to a **B-Tree**.

It keeps values sorted, enabling fast:

- Searches
    
- Range queries
    
- Sorting
    

You do not need deep internal knowledge for interviews, but remember:

Indexes are optimized for fast retrieval.

---

# 5. When Should You Create an Index?

Index columns that are frequently used in:

## WHERE clauses

Example:

```sql
SELECT * FROM users WHERE email = ?
```

Email should be indexed.

---

## JOIN conditions

Example:

```
orders.customer_id
```

Foreign keys are excellent index candidates.

---

## Sorting (ORDER BY)

Example:

```sql
ORDER BY created_at DESC
```

Index improves sort performance.

---

## Filtering Large Tables

If a column is used often to narrow results, index it.

---

# 6. When NOT to Use Indexes

Indexes are powerful but not free.

Avoid indexing:

## Small Tables

Full scan is already fast.

## Columns That Change Frequently

Indexes must be updated on every write.

This slows inserts and updates.

## Low-Cardinality Columns

Example:

```
is_active = true/false
```

If most rows share the same value, the index provides little benefit.

---

# 7. Cardinality (Important Concept)

Cardinality refers to the uniqueness of column values.

## High Cardinality (Good for Indexing)

- email
    
- phone number
    
- user_id
    

## Low Cardinality (Bad for Indexing)

- gender
    
- boolean flags
    
- status with few values
    

Strong interview line:

Indexes work best on highly selective columns.

---

# 8. Types of Indexes (Basic Awareness)

You do not need deep knowledge, but understand the common ones.

## Primary Index

Automatically created for primary keys.

Always indexed.

---

## Unique Index

Ensures no duplicate values.

Example:

```
email UNIQUE
```

Improves both integrity and lookup speed.

---

## Composite Index (Very Important)

Index on multiple columns.

Example:

```
(first_name, last_name)
```

Best when queries filter using both fields.

Important rule:

Column order matters.

An index on:

```
(A, B)
```

Works for:

```
WHERE A = ?
WHERE A = ? AND B = ?
```

But NOT for:

```
WHERE B = ?
```

This is a favorite interview question.

---

# 9. Index Tradeoffs

Indexes speed up reads but slow down writes.

Why?

Because every insert or update must also update the index.

Balance is key.

Professional guideline:

Optimize for the dominant workload (read-heavy vs write-heavy).

---

# 10. Query Optimization Basics

Indexing alone is not enough. Poor queries can still perform badly.

---

## Select Only Needed Columns

Bad:

```sql
SELECT * FROM users;
```

Good:

```sql
SELECT id, name FROM users;
```

Reduces:

- Disk I/O
    
- Memory usage
    
- Network load
    

---

## Avoid Unnecessary Data Fetching

Fetching large entity graphs in JPA can generate multiple queries.

Always be aware of what your ORM is doing.

---

## Use Pagination

Never load massive datasets into memory.

Limit results.

---

## Avoid Leading Wildcards

Bad:

```sql
WHERE name LIKE '%john'
```

Index cannot be used efficiently.

Better:

```sql
WHERE name LIKE 'john%'
```

---

## Optimize Joins

Join only required tables.

Excess joins increase query complexity and execution time.

---

# 11. Detecting Slow Queries

Databases provide tools such as:

- EXPLAIN plans
    
- Query analyzers
    
- Performance dashboards
    

These show whether indexes are being used or if a table scan is happening.

Learning to read execution plans is a valuable advanced skill.

---

# 12. Indexing in JPA

You can define indexes directly in the entity.

Example:

```java
@Table(
    indexes = {
        @Index(name="idx_email", columnList="email")
    }
)
```

This ensures the schema includes the index.

Important for production readiness.

---

# 13. Common Developer Mistakes

Indexing every column  
Creates overhead and slows writes.

Ignoring indexes completely  
Leads to slow queries at scale.

Using SELECT * everywhere  
Transfers unnecessary data.

Not indexing foreign keys  
Hurts join performance.

Assuming ORM automatically optimizes queries  
It does not.

Skipping query monitoring  
Performance issues go unnoticed.

---

# 14. High-Probability Interview Questions

What is an index?  
A structure that speeds up data retrieval.

Do indexes improve writes?  
No, they slow writes slightly.

Which columns should be indexed?  
Frequently filtered, joined, or sorted columns.

What is cardinality?  
The uniqueness of column values.

Why is SELECT * discouraged?  
It fetches unnecessary data.

What is a composite index?  
An index on multiple columns where order matters.

---

# Quick Memory Summary

```
Index → Faster reads
Too many indexes → Slower writes
High cardinality → Best candidate
Composite index → Order matters
```

Golden rule:

Index what you search — not everything.

---

# Final Takeaway

Indexing and query optimization directly determine database performance. Mastering these concepts shows that you think beyond code and understand system behavior at scale.

This signals:

- Performance awareness
    
- Database maturity
    
- Production readiness
    

Professional guideline:

Design queries carefully, index selectively, monitor performance, and treat the database as a critical part of system architecture.