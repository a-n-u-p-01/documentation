Entity lifecycle callbacks allow you to execute logic **automatically when an entity changes state.**

Think of them as:

> Hooks that run at specific moments in an entity’s life.

Very useful in production systems for:

- Audit fields
    
- Data normalization
    
- Security checks
    
- Logging
    

Interviewers often ask this to test whether you understand **how Hibernate manages entities internally.**

---

# 1. What is Entity Lifecycle?

When Hibernate manages an entity, it moves through stages:

```
Create → Persist → Update → Delete → Load
```

Lifecycle callbacks let you attach logic to these stages.

Example:

> Automatically set `createdAt` before saving.

No need to remember it in service code.

---

# 2. Why Lifecycle Callbacks Matter

Without callbacks:

Every save requires manual setup:

```java
user.setCreatedAt(now);
```

Easy to forget.

Callbacks guarantee consistency.

### Professional Insight:

> Push repetitive persistence logic closer to the entity.

Cleaner architecture.

---

# 3. Available Lifecycle Annotations (Must Know)

## @PrePersist

Runs **before the entity is inserted** into the database.

### Most commonly used callback.

### Example:

```java
@PrePersist
public void beforeSave() {
    this.createdAt = LocalDateTime.now();
}
```

Perfect for:

- createdAt
    
- default values
    
- UUID generation
    

---

## @PostPersist

Runs AFTER the insert.

Useful when you need the generated ID.

Example use case:

- Send audit event
    
- Log creation
    

(Not extremely common.)

---

## @PreUpdate

Runs BEFORE an update occurs.

### Common Use:

```java
@PreUpdate
public void beforeUpdate() {
    this.updatedAt = LocalDateTime.now();
}
```

Ensures timestamps stay accurate.

---

## @PostUpdate

Runs AFTER update.

Mostly used for logging or notifications.

Less common.

---

## @PreRemove

Runs BEFORE deletion.

Useful for:

- Validation
    
- Preventing dangerous deletes
    
- Archiving data
    

Example:

```java
@PreRemove
public void validateDelete() {
    if(activeOrders > 0){
        throw new RuntimeException("Cannot delete");
    }
}
```

Powerful safeguard.

---

## @PostRemove

Runs AFTER deletion.

Typical use:

- Audit logs
    
- Cleanup
    

---

## @PostLoad

Runs AFTER entity is fetched from DB.

Useful for computed fields.

Example:

```java
@PostLoad
public void calculateAge() {
    this.age = Period.between(dob, LocalDate.now()).getYears();
}
```

Avoid storing derived values in DB.

Calculate instead.

Strong design habit.

---

# 4. Where to Place These Methods

Inside the entity:

```java
@Entity
public class User {

    @PrePersist
    void onCreate(){}
}
```

OR

Use an external listener (advanced pattern).

---

# 5. Entity Listeners (Senior-Level Insight)

Instead of cluttering entities:

Create a listener class.

```java
@EntityListeners(AuditListener.class)
```

Listener:

```java
public class AuditListener {

    @PrePersist
    public void setCreated(Object entity){}
}
```

Better separation of concerns.

Seen in mature systems.

---

# 6. Execution Order (Good Interview Point)

For save:

```
@PrePersist
INSERT
@PostPersist
```

For update:

```
@PreUpdate
UPDATE
@PostUpdate
```

For delete:

```
@PreRemove
DELETE
@PostRemove
```

---

# 7. VERY Important Limitations

## Do NOT Put Business Logic Here

Bad idea:

- Payment processing
    
- External API calls
    
- Complex validation
    

Why?

Lifecycle methods should be:

✅ Fast  
✅ Lightweight  
✅ Reliable

Blocking operations can slow persistence.

---

## Avoid Database Calls

Can create recursive persistence problems.

Keep them simple.

---

# 8. Lifecycle vs Service Logic

### Use Lifecycle For:

✔ timestamps  
✔ normalization  
✔ auto-values  
✔ audit metadata

---

### Use Services For:

✔ business rules  
✔ workflows  
✔ transactions  
✔ external integrations

Strong architectural boundary.

---

# 9. Performance Awareness

Callbacks run automatically.

Too many heavy callbacks can:

- Slow inserts
    
- Increase transaction time
    
- Hurt throughput
    

Design carefully.

---

# 10. Common Developer Mistakes

### Forgetting @PreUpdate

Leads to stale `updatedAt`.

---

### Writing Heavy Logic

Kills performance.

---

### Throwing Random Exceptions

Can break transactions.

Be intentional.

---

### Overusing Callbacks

Not every entity needs them.

---

# 11. Most Common Production Pattern

Nearly every serious system has:

```
createdAt
updatedAt
```

Auto-managed via:

```
@PrePersist
@PreUpdate
```

This is practically industry standard.

---

# 12. High-Probability Interview Questions

### What is @PrePersist?

Runs before inserting an entity.

---

### Difference between @PrePersist and @PreUpdate?

Insert vs update trigger.

---

### When does @PostLoad run?

After entity retrieval.

---

### Should business logic go into lifecycle callbacks?

No.

---

### Why use entity listeners?

To separate auditing logic from entity code.

---

# Quick Memory Summary

```
PrePersist → Before insert
PreUpdate → Before update
PreRemove → Before delete
PostLoad → After fetch
```

### Golden Rule:

> Use lifecycle callbacks for persistence concerns — not business workflows.

---

# Final Takeaway

Lifecycle callbacks give you **automatic persistence behavior** without polluting service code.

Knowing this shows:

- ORM depth
    
- Clean architecture thinking
    
- Production awareness
    

### Professional Guideline:

> Automate repetitive persistence tasks, keep callbacks lightweight, and never mix them with business logic.

---
