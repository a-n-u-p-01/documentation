# 1. What is an Entity?

An **Entity** is a Java object that represents a **database table**.

Mapping looks like:

```
Class  → Table
Fields → Columns
Row    → Object
```

Example:

```
User class ↔ users table
```

This is the core of **Object Relational Mapping (ORM).**

---

# 2. @Entity — Making a Class Persistent

Mark a class with `@Entity` so Hibernate knows it should be stored in the database.

## Example

```java
@Entity
public class User {

    @Id
    private Long id;

    private String name;
}
```

Now Hibernate treats this class as a table.

Without `@Entity` → the class is ignored.

---

## Important Rules (Interview Favorite)

### Must Have a Primary Key

Every entity requires an identifier.

```
@Id
```

No ID → runtime failure.

---

### Must Be Non-Final

Hibernate creates proxies.

Final classes break proxying.

---

### Needs a No-Arg Constructor

Required for object creation via reflection.

Can be `protected`.

---

### Should Not Be Immutable

Hibernate updates fields dynamically.

---

# 3. @Table — Controlling Table Mapping

By default:

```
Class name → Table name
```

But you should rarely rely on defaults in production.

Use `@Table` for clarity.

---

## Example

```java
@Entity
@Table(name = "users")
public class User {
}
```

Now explicitly mapped.

Improves readability and avoids naming surprises.

---

## Useful @Table Attributes

### name

Specify table name.

```java
@Table(name="app_users")
```

---

### uniqueConstraints

```java
@Table(
    uniqueConstraints = {
        @UniqueConstraint(columnNames = "email")
    }
)
```

Prevents duplicate data.

Great for:

- email
    
- username
    

---

### indexes (Performance Boost)

```java
@Table(
    indexes = {
        @Index(name="idx_email", columnList="email")
    }
)
```

Improves query speed dramatically.

Very strong production signal.

---

# 4. Mapping Fields to Columns

Default behavior:

```
fieldName → column_name
```

But explicit mapping is safer.

## @Column Example

```java
@Column(name="email", nullable=false, unique=true)
private String email;
```

---

## Important Attributes

### nullable

```
nullable=false
```

Enforces NOT NULL.

---

### unique

Adds a DB constraint.

---

### length

```
@Column(length = 100)
```

Useful for strings.

---

### updatable / insertable

Control write behavior.

Rare but powerful.

---

# 5. Primary Key Strategies (@Id + @GeneratedValue)

Every entity needs a primary key.

## Auto Generation Example

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

---

## Common Strategies

### IDENTITY (Most Common)

Database auto-increments.

Best for MySQL/Postgres.

---

### SEQUENCE (Preferred for High Scale)

Uses DB sequence.

Better performance in some systems.

Common in PostgreSQL/Oracle.

---

### AUTO

Hibernate chooses.

Less predictable — many teams avoid it.

---

### UUID (Increasingly Popular)

Avoids sequential IDs.

Great for distributed systems.

---

# 6. Best Practices for ID Fields

### Prefer Numeric or UUID

Avoid business data as primary keys.

Bad:

```
email as PK
```

Emails change.

IDs should not.

---

### Never Expose Sequential IDs Publicly (Advanced Insight)

Sequential IDs reveal:

- User count
    
- Business growth
    
- Internal structure
    

Public APIs often use UUIDs.

Strong architecture signal.

---

# 7. Avoid Heavy Entities (Beginner Mistake)

Entities should represent persistence — NOT business logic containers.

Avoid putting:

- Complex calculations
    
- External API calls
    
- Heavy transformations
    

Keep them lightweight.

---

# 8. Entity Naming Best Practice

Use singular names.

```
User
Order
Product
```

NOT:

```
Users
Orders
```

Because each object represents ONE row.

---

# 9. Auditing Fields (Very Common)

Most production entities include:

```
createdAt
updatedAt
```

Example:

```java
@Column(nullable = false)
private LocalDateTime createdAt;
```

Often auto-managed later with auditing features.

Good design habit.

---

# 10. Common Developer Mistakes

### Skipping @Table

Leads to unpredictable naming.

---

### Using Reserved SQL Keywords

Example:

```
@Table(name="order")
```

"ORDER" is reserved.

Causes failures.

Use:

```
orders
```

---

### Forgetting Indexes

Queries become slow at scale.

---

### Large Entities

Fetching them becomes expensive.

---

### Bidirectional Relationships Too Early

Creates complexity.

Start simple.

---

# 11. Performance Insight (High Value)

Mapping decisions directly affect:

- Query speed
    
- Join cost
    
- Memory usage
    

ORM is not just convenience — it shapes database behavior.

Think carefully.

---

# 12. High-Probability Interview Questions

### What is an entity?

A Java object mapped to a database table.

---

### Is @Table mandatory?

No — but recommended.

---

### Why must an entity have a primary key?

Hibernate needs a unique identifier to track objects.

---

### Which ID strategy is most common?

IDENTITY.

---

### Why add indexes?

To speed up database queries.

---

# Quick Memory Summary

```
@Entity → Marks persistent class
@Table → Controls table mapping
@Id → Primary key
@Column → Controls column behavior
```

### Golden Rule:

> Treat entities as database models — design them carefully.

---

# Final Takeaway

Entity mapping is where **object design meets database design.**

Strong mapping leads to:

- Faster queries
    
- Cleaner schema
    
- Scalable systems
    

### Professional Guideline:

> Be explicit in mappings, choose IDs wisely, and design entities with long-term database health in mind.

---
