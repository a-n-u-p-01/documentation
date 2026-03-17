## 1. Introduction

Write Through and Write Back are caching strategies used to manage how **write operations** are handled between the cache and the database.

While **Cache Aside** focuses mostly on reads, these patterns focus on **how updates are written**.

They help maintain consistency between cache and database.

---

## 2. Write Through Cache

Write Through means data is written **to both the cache and the database at the same time**.

The cache sits between the application and the database.

---

### How Write Through Works

Step 1  
Application sends write request.

Step 2  
Cache updates the data.

Step 3  
Cache writes the same data to the database.

Step 4  
Both cache and database remain synchronized.

---

### Example Flow

User updates profile.

Application → Cache  
Cache → Database

Now both cache and database contain the updated data.

---

### Advantages of Write Through

Data in cache is always consistent with database.

Reads become faster because updated data is already in cache.

Simple consistency model.

---

### Disadvantages of Write Through

Write operations become slower because data must be written to two places.

Cache may store unnecessary data that may never be read.

---

## 3. Write Back Cache (Write Behind)

Write Back means data is first written **only to the cache**, and later **asynchronously written to the database**.

The database update happens in the background.

---

### How Write Back Works

Step 1  
Application writes data to cache.

Step 2  
Cache immediately returns success.

Step 3  
Cache updates database later in background.

---

### Example Flow

Application → Cache (fast)  
Cache → Database (later)

This reduces write latency.

---

### Advantages of Write Back

Very fast write operations.

Reduced database load.

Efficient for write-heavy systems.

---

### Disadvantages of Write Back

Risk of data loss if cache crashes before writing to database.

More complex implementation.

Requires background processing.

---

## 4. Comparison

Write Through:

- Write to cache and database simultaneously
    
- Strong consistency
    
- Slower writes
    

Write Back:

- Write to cache first
    
- Database updated later
    
- Faster writes but risk of data loss
    

---

## 5. Real-World Use Cases

Write Through:

- Systems where data consistency is critical
    
- User profile updates
    
- Configuration data
    

Write Back:

- Logging systems
    
- Analytics pipelines
    
- High write throughput systems
    

---

## 6. Architecture Example

Write Through:

Application → Cache → Database

Write Back:

Application → Cache → (Async) Database

---

## 7. Write Through vs Cache Aside

Cache Aside:

Application manages cache.

Write Through:

Cache manages database writes automatically.

---

## 8. Interview Questions with Answers

### 1. What is Write Through caching?

Answer:  
Write Through caching writes data to both cache and database simultaneously to keep them consistent.

---

### 2. What is Write Back caching?

Answer:  
Write Back caching writes data to the cache first and updates the database asynchronously later.

---

### 3. Which caching strategy has faster writes?

Answer:  
Write Back caching because database writes happen asynchronously.

---

### 4. What is the risk of Write Back caching?

Answer:  
Data loss if the cache crashes before writing to the database.

---

### 5. Which is safer: Write Through or Write Back?

Answer:  
Write Through is safer because data is written to the database immediately.

---

## 9. Summary

Write Through:

- Cache and database updated together
    
- Strong consistency
    
- Slower writes
    

Write Back:

- Cache updated first
    
- Database updated later
    
- Faster writes but riskier
    

Both strategies are important in designing scalable caching systems.

---

Next topic:  
[[Cache Invalidation Strategies]] which is considered **one of the hardest problems in system design**.