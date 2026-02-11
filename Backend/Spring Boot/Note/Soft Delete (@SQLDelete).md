Soft delete is a widely used production strategy where data is **not physically removed** from the database. Instead, it is marked as deleted and hidden from normal queries.

Many enterprise systems use soft deletes to preserve history, enable recovery, and meet audit requirements.

Interviewers often ask this to evaluate your database design maturity.

---

# 1. What is Soft Delete?

Instead of executing:

```sql
DELETE FROM users WHERE id = 1;
```

You perform an update:

```sql
UPDATE users SET deleted = true WHERE id = 1;
```

The row still exists — but your application treats it as deleted.

Core idea:

Delete logically, not physically.

---

# 2. Why Soft Delete is Important

## Data Recovery

Accidental deletes can be reversed.

## Audit Trail

Historical data remains available.

## Regulatory Compliance

Some industries require data retention.

## Relationship Safety

Prevents breaking foreign key references.

## Debugging

Old records help investigate past issues.

Strong design insight:

Hard deletes are irreversible. Soft deletes provide safety.

---

# 3. Basic Implementation Strategy

Soft delete typically requires a flag column.

## Example Entity Field

```java
@Column(nullable = false)
private boolean deleted = false;
```

Common alternatives:

- `is_deleted`
    
- `active`
    
- `status`
    

Boolean is simplest, but timestamps are often better (explained later).

---

# 4. Using @SQLDelete

Hibernate provides `@SQLDelete` to override the default DELETE statement.

## Example

```java
@Entity
@SQLDelete(sql =
    "UPDATE users SET deleted = true WHERE id=?")
public class User {
}
```

Now when you call:

```java
repository.delete(user);
```

Hibernate runs an UPDATE instead of DELETE.

No application code changes required.

Very clean solution.

---

# 5. Hiding Deleted Rows with @Where

Marking rows as deleted is not enough.  
You must also prevent them from appearing in queries.

Use:

```
@Where
```

## Example

```java
@Entity
@SQLDelete(sql =
    "UPDATE users SET deleted = true WHERE id=?")
@Where(clause = "deleted = false")
public class User {
}
```

Effect:

Every query automatically adds:

```sql
WHERE deleted = false
```

Developers do not need to remember filtering.

Strong production pattern.

---

# 6. What Happens Internally

When delete is called:

```
repository.delete()
     ↓
Hibernate intercepts
     ↓
Executes UPDATE
     ↓
Row marked deleted
```

Future queries ignore it due to `@Where`.

---

# 7. Boolean vs Timestamp Soft Delete

## Boolean Flag

```
deleted = true/false
```

Simple and common.

But lacks historical context.

---

## Timestamp (Recommended for Mature Systems)

```
deleted_at = 2025-01-10 12:30
```

Benefits:

- Shows when deletion happened
    
- Enables retention policies
    
- Useful for analytics
    

Many senior teams prefer timestamps.

Strong interview statement:

Timestamp-based soft deletes provide better auditability than boolean flags.

---

# 8. Querying Deleted Data

Sometimes admins need access to deleted records.

Problem:

`@Where` filters them automatically.

Solutions include:

- Writing native queries
    
- Using repository methods without filters
    
- Maintaining separate admin views
    

Design your system with this requirement in mind.

---

# 9. Soft Delete and Relationships (Important)

Be careful when soft deleting parent entities.

Example:

```
User → Orders
```

If the user is soft deleted:

Orders still reference the user.

Decide whether to:

- Allow access for reporting
    
- Hide related data
    
- Cascade soft delete
    

There is no universal rule — it is a business decision.

But ignoring this leads to inconsistent APIs.

---

# 10. Unique Constraint Problem (Advanced Insight)

Imagine a unique email column.

User is soft deleted.

Now a new user registers with the same email.

The database blocks it because the old row still exists.

Solutions:

- Include deleted flag in unique index
    
- Archive old records
    
- Restore instead of re-create
    

This is a real production challenge.

Mentioning it shows senior-level awareness.

---

# 11. Performance Considerations

Soft deletes increase table size over time.

Large tables can slow queries.

Mitigation strategies:

- Index the deleted column
    
- Archive old data
    
- Partition large tables
    

Always plan long-term storage behavior.

---

# 12. When NOT to Use Soft Delete

Soft delete is not always the right choice.

Avoid it when:

## Data Has No Long-Term Value

Example: temporary logs.

## Tables Grow Extremely Fast

Storage becomes expensive.

## Strict Privacy Laws Apply

Some regulations require permanent deletion.

Engineering is about choosing wisely.

---

# 13. Common Developer Mistakes

Forgetting to filter deleted rows  
Leads to confusing API responses.

Using soft delete everywhere  
Not all tables need it.

Ignoring indexing  
Queries slow down significantly.

Assuming soft delete is free  
It adds storage and complexity.

Mixing hard and soft deletes randomly  
Creates unpredictable data states.

---

# 14. High-Probability Interview Questions

What is soft delete?  
Marking a record as deleted instead of physically removing it.

Why use @SQLDelete?  
To override the default DELETE with an UPDATE.

What does @Where do?  
Automatically filters out deleted rows.

Boolean vs timestamp — which is better?  
Timestamp is better for auditing.

What is a common issue with soft deletes?  
Unique constraints can block new inserts.

---

# Quick Memory Summary

```
Hard delete → Row removed
Soft delete → Row flagged
@SQLDelete → Converts delete to update
@Where → Hides deleted rows
```

Golden rule:

Prefer soft delete when data history matters.

---

# Final Takeaway

Soft delete is a hallmark of mature database design because it prioritizes safety, traceability, and auditability.

Understanding it signals:

- Production experience
    
- Data lifecycle awareness
    
- Strong schema design
    

Professional guideline:

Use soft deletes intentionally, index the delete flag, and plan how historical data will be managed as your system grows.