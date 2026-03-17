## 1. Introduction

Cache Aside Pattern (also called **Lazy Loading**) is the most commonly used caching strategy in backend systems.

In this pattern, the **application is responsible for managing the cache**.

In simple terms:

The application first checks the cache.  
If data is not found, it retrieves data from the database and stores it in the cache.

---

## 2. Why Cache Aside is Used

Databases are slower than in-memory storage.

Cache Aside helps:

- Reduce database load
    
- Improve response time
    
- Handle high traffic
    

It is widely used with caching systems like **Redis** or **Memcached**.

---

## 3. How Cache Aside Works

### Step-by-Step Flow

Step 1  
Application receives request for data.

Step 2  
Application checks cache.

Step 3  
If data exists → return cached data.  
This is called a **Cache Hit**.

Step 4  
If data does not exist → query database.  
This is called a **Cache Miss**.

Step 5  
Store the retrieved data in cache.

Step 6  
Return the data to the user.

---

## 4. Example

User requests product details.

Step 1  
Application checks Redis cache.

Step 2  
If product exists in cache → return result.

Step 3  
If not → fetch product from database.

Step 4  
Store product in cache.

Step 5  
Return response.

Future requests will use the cached data.

---

## 5. Architecture Example

User Request  
↓  
Application Server  
↓  
Check Cache (Redis)  
↓  
If miss → Database  
↓  
Store result in cache  
↓  
Return response

---

## 6. Updating Data in Cache Aside

When data changes:

Step 1  
Update database.

Step 2  
Invalidate or remove cache entry.

Next request will fetch fresh data from database and update cache again.

This ensures cache does not serve stale data.

---

## 7. Advantages of Cache Aside

Simple to implement.

Cache is used only when needed.

Reduces database load.

Efficient for **read-heavy systems**.

Works well with distributed caches like Redis.

---

## 8. Disadvantages of Cache Aside

Cache misses cause additional latency.

Cache may serve stale data if invalidation is not handled correctly.

Extra logic required in application.

---

## 9. Real-World Use Cases

Cache Aside is used in:

- Product catalogs in e-commerce
    
- User profiles
    
- Social media feeds
    
- Configuration data
    

Most large-scale systems use this pattern.

---

## 10. Cache Aside vs Other Patterns

Cache Aside  
Application manages cache.

Write Through  
Cache is updated at the same time as database.

Write Back  
Cache writes to database asynchronously.

Cache Aside is the most common pattern.

---

## 11. Interview Questions with Answers

### 1. What is Cache Aside pattern?

Answer:  
Cache Aside is a caching strategy where the application first checks the cache and loads data from the database into the cache when it is missing.

---

### 2. What is a cache hit?

Answer:  
When requested data is found in the cache.

---

### 3. What is a cache miss?

Answer:  
When requested data is not found in cache and must be retrieved from the database.

---

### 4. How do you update cache in Cache Aside pattern?

Answer:  
Update the database first and then invalidate or delete the cache entry.

---

### 5. Why is Cache Aside widely used?

Answer:  
Because it is simple, efficient for read-heavy workloads, and reduces database load.

---

## 12. Summary

Cache Aside is the most commonly used caching strategy.

Flow:

Application → Cache → Database → Cache Update

Key points:

- Cache checked first
    
- Database used only on cache miss
    
- Cache updated after database fetch
    
- Cache invalidated when data changes
    

It is widely used in scalable backend architectures.

---
