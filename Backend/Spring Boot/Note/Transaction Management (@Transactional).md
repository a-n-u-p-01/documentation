# 1. What is a Transaction?

A transaction is a group of database operations executed as a single unit.

The rule is simple:

Either all operations succeed, or none are applied.

There is no partial completion.

## Example

Consider transferring money between two accounts:

1. Deduct money from Account A
    
2. Add money to Account B
    

If the deduction succeeds but the addition fails, the system becomes inconsistent.

Transactions prevent this by rolling back all changes when an error occurs.

---

# 2. ACID Properties (Must Know for Interviews)

Transactions guarantee the ACID properties.

## Atomicity

All operations execute completely or not at all.

## Consistency

The database always moves from one valid state to another. Constraints and rules are preserved.

## Isolation

Transactions do not interfere with each other during execution.

## Durability

Once committed, the data is permanently stored even if the system crashes.

Strong interview statement:

Transactions ensure atomic and reliable database behavior through ACID guarantees.

---

# 3. What Does @Transactional Do?

`@Transactional` tells Spring to wrap a method inside a database transaction.

Example:

```java
@Transactional
public void placeOrder() {
    saveOrder();
    updateInventory();
    processPayment();
}
```

If any step throws an exception, Spring rolls back the entire transaction automatically.

If everything succeeds, the transaction is committed.

---

# 4. Where Should @Transactional Be Used?

Correct placement is very important for clean architecture.

## Recommended Location: Service Layer

Typical flow:

```
Controller → Service (@Transactional) → Repository → Database
```

Why service layer?

Because a single business operation often involves multiple repository calls that must succeed together.

## Avoid Using It On:

### Controllers

Controllers should remain lightweight and focused on request handling.

### Repository Interfaces

Transactions belong to business operations, not individual database calls.

Strong guideline:

Define transaction boundaries around business logic, not persistence methods.

---

# 5. How Spring Manages Transactions Internally

Spring uses proxy-based AOP (Aspect-Oriented Programming).

Execution flow:

```
Method call
   ↓
Proxy opens transaction
   ↓
Method executes
   ↓
Commit if successful
Rollback if exception occurs
```

This behavior is automatic. You do not manually open or close transactions.

---

# 6. Default Rollback Behavior (Very Important)

Spring rolls back transactions only for:

- RuntimeException
    
- Error
    

It does NOT roll back for checked exceptions by default.

## Example

This triggers rollback:

```java
throw new RuntimeException();
```

This does NOT:

```java
throw new IOException();
```

## How to Force Rollback for Checked Exceptions

```java
@Transactional(rollbackFor = Exception.class)
```

This is a commonly asked interview question.

---

# 7. Transaction Propagation

Propagation defines how transactions behave when one transactional method calls another.

## REQUIRED (Default)

- Joins an existing transaction if present
    
- Creates a new one if none exists
    

Used in most applications.

---

## REQUIRES_NEW

Always starts a completely new transaction and suspends the current one.

Useful when you must commit something regardless of the main transaction outcome.

Example use cases:

- Audit logging
    
- Payment history
    
- Event tracking
    

Even if the main transaction fails, this one commits.

---

## SUPPORTS

- Uses a transaction if available
    
- Otherwise runs without one
    

Less commonly used.

Strong interview point:

REQUIRED is the default and most widely used propagation level.

---

# 8. Isolation Levels (Concurrency Control)

Isolation determines how transactions interact when running simultaneously.

## READ_COMMITTED (Most Common)

Prevents reading uncommitted data from other transactions.

Provides a good balance between consistency and performance.

---

## REPEATABLE_READ

Ensures that repeated reads within a transaction return the same data.

Stronger consistency but slightly more locking.

---

## SERIALIZABLE

Highest isolation level.

Transactions execute as if they were sequential.

Very safe but can severely impact performance due to heavy locking.

Strong interview statement:

READ_COMMITTED is the most commonly used isolation level in production systems.

---

# 9. Read-Only Transactions

If a method only fetches data:

```java
@Transactional(readOnly = true)
```

Benefits:

- Reduces locking
    
- Allows database optimizations
    
- Improves performance
    

Common in high-traffic read APIs.

---

# 10. Transaction Boundary (Critical Design Concept)

A transaction should represent one complete business operation.

Good transaction:

- Short-lived
    
- Focused
    
- Efficient
    

Bad transaction:

- Calls external APIs
    
- Waits on network responses
    
- Processes large files
    

Long transactions hold database locks and hurt scalability.

Professional rule:

Keep transactions short to minimize contention.

---

# 11. Self Invocation Problem (Advanced Interview Trap)

If a transactional method calls another method within the same class, the transaction may not activate.

Example:

```java
public void outer() {
    inner(); // Transaction might be ignored
}
```

Why?

Because the call bypasses the Spring proxy.

## Solution

Call the method through another Spring-managed bean.

Understanding this shows deeper framework knowledge.

---

# 12. Common Developer Mistakes

Missing @Transactional  
Leads to partial updates and inconsistent data.

Placing transactions on controllers  
Breaks architectural layering.

Ignoring rollback rules  
Checked exceptions can silently commit data.

Creating long transactions  
Causes lock contention and slows the system.

Mixing database operations with remote service calls  
If the remote call hangs, the database remains locked.

---

# 13. High-Probability Interview Questions

What is a transaction?  
A unit of work that ensures all database operations succeed or fail together.

What are ACID properties?  
Atomicity, Consistency, Isolation, Durability.

Where should @Transactional be placed?  
Service layer.

Does Spring roll back for checked exceptions?  
No, unless explicitly configured.

What is the default propagation?  
REQUIRED.

Why keep transactions short?  
To reduce locking and improve performance.

---

# Quick Memory Summary

```
@Transactional → Opens transaction
Success → Commit
Exception → Rollback
```

Golden rule:

Design transactions around business operations, not individual queries.

---

# Final Takeaway

Transaction management is fundamental to building reliable backend systems.

Mastering it signals:

- Strong database understanding
    
- Production awareness
    
- Concurrency knowledge
    

Professional guideline:

Always define clear transaction boundaries, understand rollback behavior, and avoid long-running transactions.