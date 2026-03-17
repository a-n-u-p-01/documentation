## 1. Introduction

A Distributed Cache is a caching system where cached data is stored across **multiple machines instead of a single server**.

In simple terms:

Instead of storing cache in one server, the cache is spread across several servers to handle large-scale traffic.

Distributed caching is commonly used in large systems to improve **scalability and performance**.

---

## 2. Why Distributed Cache is Needed

In small applications, cache may exist inside the application server memory.

Example:

Application Server → Local Cache

But in large systems:

- Many application servers exist
    
- Traffic is very high
    
- Local cache becomes inconsistent
    

Distributed cache solves these problems.

---

## 3. Problem with Local Cache

Suppose we have multiple servers:

Server A  
Server B  
Server C

If each server has its own cache:

User request → Server A → Cache updated

But Server B and Server C still have old data.

This creates **cache inconsistency**.

---

## 4. Solution: Distributed Cache

Distributed cache stores data in a **shared caching system**.

Example architecture:

Client  
↓  
Load Balancer  
↓  
Application Servers  
↓  
Distributed Cache (Redis Cluster)  
↓  
Database

Now all servers read and write to the **same cache system**.

---

## 5. How Distributed Cache Works

Step 1  
Application receives request.

Step 2  
Application checks distributed cache.

Step 3  
If data exists → return cached data.

Step 4  
If not → fetch data from database.

Step 5  
Store result in distributed cache.

---

## 6. Popular Distributed Cache Systems

Common technologies include:

Redis  
Most widely used distributed cache.

Memcached  
Simple, high-performance caching system.

Hazelcast  
Distributed in-memory data grid.

Redis is the most common in modern backend systems.

---

## 7. Advantages of Distributed Cache

### Scalability

Cache capacity increases by adding more nodes.

---

### Consistency

All application servers use the same cache.

---

### High Availability

If one cache node fails, others continue operating.

---

### Performance

Memory-based storage provides extremely fast access.

---

## 8. Data Partitioning (Sharding)

Distributed cache often divides data across nodes.

Example:

Cache Node 1 → Keys 1–1000  
Cache Node 2 → Keys 1001–2000  
Cache Node 3 → Keys 2001–3000

This is called **sharding**.

It improves scalability.

---

## 9. Consistent Hashing

In distributed caches, consistent hashing is used to determine which node stores a specific key.

Benefits:

- Even data distribution
    
- Minimal data movement when nodes are added or removed
    

Consistent hashing is commonly used in Redis clusters.

---

## 10. Cache Replication

For fault tolerance, cache nodes may replicate data.

Example:

Primary Cache Node  
Replica Cache Node

If the primary fails, replica continues serving requests.

---

## 11. Challenges with Distributed Cache

### Cache Invalidation

Maintaining consistency with database.

---

### Network Latency

Cache is no longer local memory.

---

### Cache Synchronization

Multiple nodes must stay consistent.

---

## 12. Real-World Use Cases

Distributed caches are used in:

- Social media platforms
    
- E-commerce websites
    
- Content delivery systems
    
- Microservices architectures
    

Most high-scale systems rely heavily on distributed caching.

---

## 13. Interview Questions with Answers

### 1. What is a distributed cache?

Answer:  
A distributed cache stores cached data across multiple servers to improve scalability and performance.

---

### 2. Why is distributed cache used instead of local cache?

Answer:  
Because local caches become inconsistent when multiple application servers exist.

---

### 3. What technologies are used for distributed caching?

Answer:  
Redis, Memcached, and Hazelcast.

---

### 4. What is cache sharding?

Answer:  
Dividing cached data across multiple nodes to distribute load and increase capacity.

---

### 5. What is consistent hashing?

Answer:  
A technique used to distribute data evenly across cache nodes with minimal data movement when nodes change.

---

## 14. Summary

Distributed caching allows multiple servers to share a centralized cache system.

Key benefits:

- Improved scalability
    
- Faster response time
    
- Reduced database load
    
- High availability
    

It is a core component of large-scale distributed systems.

---

Next topic:  
[[Redis Basics]] which explains **how Redis works internally and why it is the most popular caching system**.