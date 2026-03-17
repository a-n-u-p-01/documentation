## 1. Introduction

Cache Eviction Policies determine **which data should be removed from the cache when the cache becomes full**.

Since cache memory is limited, older or less useful data must be removed to make space for new data.

In simple terms:

Cache eviction decides **which item to remove when the cache runs out of memory**.

---

## 2. Why Cache Eviction is Needed

Caches like Redis or Memcached store data in **RAM**, which is limited and expensive.

When the cache reaches its memory limit:

- New data cannot be stored
    
- Old data must be removed
    

Eviction policies decide **which cached items should be deleted**.

---

## 3. Common Cache Eviction Policies

### 3.1 Least Recently Used (LRU)

Removes the item that has **not been used for the longest time**.

Example:

Cache contains:

A → B → C → D

If A has not been accessed for the longest time, A is removed.

Advantages:

- Works well for most real-world systems
    
- Keeps frequently used data
    

Disadvantages:

- Requires tracking usage history.
    

Used in:  
Redis, operating systems, databases.

---

### 3.2 Least Frequently Used (LFU)

Removes the item that is accessed **least often**.

Example:

Cache access counts:

A → 20 times  
B → 10 times  
C → 2 times

C will be removed.

Advantages:

- Keeps popular items longer
    

Disadvantages:

- More complex tracking.
    

Used when **popular data should remain cached**.

---

### 3.3 First In First Out (FIFO)

Removes the **oldest inserted item**.

Example:

Cache insertion order:

A → B → C → D

A will be removed first.

Advantages:

- Simple implementation
    

Disadvantages:

- May remove frequently used items.
    

---

### 3.4 Random Replacement

Randomly selects a cache item to remove.

Advantages:

- Very simple
    
- Low overhead
    

Disadvantages:

- Not optimized for performance.
    

Rarely used in production systems.

---

## 4. Comparison of Policies

LRU:  
Removes least recently accessed item.

LFU:  
Removes least frequently accessed item.

FIFO:  
Removes oldest item.

Random:  
Removes random item.

LRU is the **most widely used eviction policy**.

---

## 5. Example Scenario

Suppose cache capacity = 3.

Requests:

A → B → C

Cache becomes full.

Next request:

D

Eviction policy decides which of A, B, or C should be removed.

If using LRU:

The least recently used item is removed.

---

## 6. Cache Eviction in Redis

Redis supports multiple eviction policies:

- allkeys-lru
    
- volatile-lru
    
- allkeys-lfu
    
- volatile-lfu
    
- volatile-ttl
    
- noeviction
    

Example:

allkeys-lru → removes least recently used key from entire cache.

---

## 7. When to Use Each Policy

LRU:  
Best for general web applications.

LFU:  
Best for systems with **highly popular data**.

FIFO:  
Simple caching systems.

Random:  
Used rarely.

---

## 8. Interview Questions with Answers

### 1. What is a cache eviction policy?

Answer:  
A cache eviction policy determines which cached data should be removed when the cache memory becomes full.

---

### 2. What is LRU caching?

Answer:  
LRU removes the item that has not been accessed for the longest time.

---

### 3. What is the difference between LRU and LFU?

Answer:  
LRU removes least recently used items, while LFU removes least frequently used items.

---

### 4. Which eviction policy is most commonly used?

Answer:  
Least Recently Used (LRU).

---

### 5. Why is cache memory limited?

Answer:  
Because cache usually uses RAM, which is expensive and limited.

---

## 9. Summary

Cache eviction policies help manage limited cache memory.

Common policies:

- LRU (most common)
    
- LFU
    
- FIFO
    
- Random
    

Choosing the right eviction policy improves cache efficiency and system performance.

---