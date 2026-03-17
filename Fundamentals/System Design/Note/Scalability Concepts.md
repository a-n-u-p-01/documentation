## 1. Definition

Scalability is the ability of a system to handle increasing load (users, traffic, or data) by adding resources without degrading performance.

In simple terms:

A scalable system continues to perform well even when the number of users grows significantly.

---

## 2. Why Scalability is Important

As applications grow:

- More users join
    
- More requests are generated
    
- More data is stored
    
- More background jobs are processed
    

If a system is not scalable:

- Response time increases
    
- Servers crash
    
- Database becomes slow
    
- Users experience downtime
    

Scalability ensures long-term system growth.

---

## 3. Types of Scalability

### 3.1 Vertical Scaling (Scaling Up)

Increasing the power of a single server.

Example:

- Adding more CPU
    
- Increasing RAM
    
- Using faster SSD
    

Advantages:

- Simple to implement
    
- No architecture change required
    

Disadvantages:

- Limited by hardware capacity
    
- Expensive
    
- Single point of failure
    

---

### 3.2 Horizontal Scaling (Scaling Out)

Adding more servers to distribute load.

Example:

- Adding more application instances
    
- Adding database replicas
    

Advantages:

- Highly scalable
    
- Better fault tolerance
    
- No hardware limit
    

Disadvantages:

- More complex
    
- Requires load balancing
    

In large-scale systems, horizontal scaling is preferred.

---

## 4. Read Scaling vs Write Scaling

### Read Scaling

Improving performance for read-heavy workloads.

Methods:

- Caching
    
- Read replicas
    
- CDN
    

Example:  
Social media feed systems.

---

### Write Scaling

Improving performance for write-heavy workloads.

Methods:

- Database sharding
    
- Partitioning
    
- Asynchronous processing
    
- Message queues
    

Example:  
Logging systems or payment systems.

---

## 5. Stateless vs Stateful Systems

Stateless System:

- No session stored in server memory
    
- Each request is independent
    
- Easy to scale horizontally
    

Stateful System:

- Stores session data in server memory
    
- Harder to scale
    

For better scalability:  
Application servers should be stateless.

---

## 6. Scaling the Database

Database often becomes bottleneck.

Common strategies:

- Indexing
    
- Read replicas
    
- Sharding
    
- Partitioning
    
- Caching layer
    
- Denormalization
    

---

## 7. Auto Scaling

Modern cloud systems support auto scaling:

- Automatically add servers during high traffic
    
- Remove servers during low traffic
    
- Cost efficient
    
- Maintains performance
    

---

## 8. Bottlenecks in Scaling

Common bottlenecks:

- Database
    
- Network bandwidth
    
- CPU
    
- Memory
    
- Disk I/O
    
- Synchronous blocking calls
    

Identifying bottlenecks is critical in system design.

---

## 9. Scalability vs Elasticity

Scalability:  
Ability to increase capacity.

Elasticity:  
Ability to automatically scale up and down based on load.

Elasticity is common in cloud systems.

---

## 10. Interview Questions with Answers

### 1. What is scalability?

Answer:  
Scalability is the ability of a system to handle increasing load by adding resources without significant performance degradation.

---

### 2. What is the difference between horizontal and vertical scaling?

Answer:  
Vertical scaling increases resources of a single machine.  
Horizontal scaling adds more machines to distribute load.

---

### 3. Why is horizontal scaling preferred in distributed systems?

Answer:  
It avoids single points of failure, supports large-scale growth, and is not limited by hardware constraints.

---

### 4. How do you scale a database?

Answer:  
Using read replicas, sharding, partitioning, caching, and indexing.

---

### 5. Why should application servers be stateless for better scalability?

Answer:  
Stateless servers allow easy horizontal scaling because any request can be handled by any server instance.

---

## 11. Summary

Scalability ensures that systems can grow with increasing users and traffic.

Key points:

- Vertical scaling has limits.
    
- Horizontal scaling is preferred for large systems.
    
- Databases are common bottlenecks.
    
- Stateless architecture improves scalability.
    
- Auto scaling helps manage dynamic traffic.
    

---

If you want, next topic logically connected is:  
[[Horizontal vs Vertical Scaling]] in deeper detail.