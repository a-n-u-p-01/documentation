## 1. Introduction

Cache Invalidation is the process of removing or updating data in the cache when the underlying data in the database changes.

In simple terms:

Cache invalidation ensures that cached data does not become outdated.

It is often called one of the hardest problems in system design because keeping cache and database consistent is challenging.

---

## 2. Why Cache Invalidation is Needed

When data changes in the database:

- The cache may still contain the old value.
    
- Users may receive stale or incorrect data.
    

Example:

User updates profile name.

Database → updated  
Cache → still stores old name

Without invalidation, users will see outdated information.

---

## 3. Types of Cache Invalidation Strategies

### 3.1 Time-Based Expiration (TTL)

Cached data is automatically removed after a certain time.

TTL stands for Time To Live.

Example:

Cache entry expires after 10 minutes.

Advantages:

- Simple implementation
    
- No manual invalidation required
    

Disadvantages:

- Data may remain stale until expiration time.
    

---

### 3.2 Write Invalidation

When data is updated in the database, the corresponding cache entry is deleted.

Flow:

1. Update database
    
2. Delete cache entry
    
3. Next request fetches fresh data from database
    

Advantages:

- Ensures fresh data on next request.
    

Disadvantages:

- Cache miss occurs after invalidation.
    

---

### 3.3 Write Update

Instead of deleting the cache, the updated value is written to the cache.

Flow:

1. Update database
    
2. Update cache
    

Advantages:

- Cache always contains latest data.
    

Disadvantages:

- Slightly more complex.
    

---

### 3.4 Event-Based Invalidation

Cache is updated based on events.

Example:

When a product price changes, an event triggers cache invalidation.

Often used with message queues such as Kafka or RabbitMQ.

---

## 4. Example Scenario

Product price update.

Step 1  
Application updates database.

Step 2  
Application deletes cache entry for product.

Step 3  
Next user request causes cache miss.

Step 4  
Fresh data is loaded from database and stored in cache.

---

## 5. Challenges with Cache Invalidation

### Stale Data

Users may see outdated information.

---

### Race Conditions

Two requests may update cache simultaneously.

---

### Distributed Systems

Multiple application servers may have separate caches.

Cache consistency becomes difficult.

---

## 6. Techniques to Handle Cache Invalidation

Use TTL to automatically expire cache.

Use versioning for cache keys.

Use centralized cache systems like Redis.

Use event-driven invalidation.

---

## 7. Cache Key Design

Proper cache key design helps invalidation.

Example:

user:123  
product:456

If product changes:

Invalidate key → product:456

Clear key naming improves maintainability.

---

## 8. Real-World Example

E-commerce product catalog.

Product information stored in cache.

When product price changes:

Database updated  
Cache entry removed  
Next request loads updated price.

---

## 9. Interview Questions with Answers

### 1. What is cache invalidation?

Answer:  
Cache invalidation is the process of removing or updating cached data when the underlying data changes.

---

### 2. Why is cache invalidation difficult?

Answer:  
Because cache and database must stay synchronized across distributed systems while avoiding stale data.

---

### 3. What is TTL in caching?

Answer:  
TTL (Time To Live) defines how long a cache entry remains valid before it expires automatically.

---

### 4. What is write invalidation?

Answer:  
When data is updated in the database, the corresponding cache entry is deleted.

---

### 5. What is event-based cache invalidation?

Answer:  
Cache is updated or invalidated when a specific event occurs, such as a data update.

---

## 10. Summary

Cache invalidation ensures cached data stays consistent with the database.

Common strategies:

- Time-based expiration (TTL)
    
- Write invalidation
    
- Write update
    
- Event-based invalidation
    

Proper cache invalidation is essential for maintaining data correctness in scalable systems.

---

Next topic:  
[[Cache Eviction Policies]] which explains how caches decide **which data to remove when memory becomes full**.