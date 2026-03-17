## 1. Introduction

Scaling is the process of increasing system capacity to handle more traffic, users, or data.

There are two primary ways to scale a system:

- Vertical Scaling (Scale Up)
    
- Horizontal Scaling (Scale Out)
    

Understanding the difference is critical in system design interviews.

---

## 2. Vertical Scaling (Scaling Up)

Vertical scaling means increasing the resources of a single machine.

Example:

- Upgrade from 8 GB RAM to 32 GB RAM
    
- Increase CPU cores
    
- Use faster SSD
    
- Move to a more powerful server
    

### Advantages

- Simple to implement
    
- No need to modify architecture
    
- No distributed system complexity
    

### Disadvantages

- Limited by hardware capacity
    
- Expensive at higher levels
    
- Causes downtime during upgrade
    
- Creates single point of failure
    

### Real Example

If your Spring Boot application is slow, you move it from:  
2 CPU cores → 8 CPU cores  
8 GB RAM → 32 GB RAM

That is vertical scaling.

---

## 3. Horizontal Scaling (Scaling Out)

Horizontal scaling means adding more machines to distribute the load.

Example:

- Add more application servers
    
- Add more database replicas
    
- Add more worker nodes
    

Traffic is distributed using a load balancer.

### Advantages

- Virtually unlimited scaling
    
- Better fault tolerance
    
- No single point of failure
    
- Suitable for large-scale systems
    

### Disadvantages

- More complex architecture
    
- Requires load balancer
    
- Requires stateless services
    
- Data consistency becomes challenging
    

### Real Example

Instead of upgrading one server, you run:

- 10 application servers behind a load balancer
    

That is horizontal scaling.

---

## 4. Visual Comparison

Vertical Scaling:  
One strong server handling everything.

Horizontal Scaling:  
Many smaller servers sharing the load.

---

## 5. Database Scaling

Vertical Scaling:  
Upgrade database server hardware.

Horizontal Scaling:

- Read replicas
    
- Sharding
    
- Partitioning
    

Large systems prefer horizontal database scaling.

---

## 6. When to Use Vertical Scaling

- Small applications
    
- Early-stage startups
    
- Low traffic systems
    
- When simplicity is preferred
    

---

## 7. When to Use Horizontal Scaling

- High traffic systems
    
- Large-scale distributed systems
    
- Applications requiring high availability
    
- Microservices architecture
    

Most internet-scale systems use horizontal scaling.

---

## 8. Key Interview Points

Interviewers expect you to mention:

- Vertical scaling has limits
    
- Horizontal scaling improves availability
    
- Horizontal scaling requires stateless architecture
    
- Load balancer is mandatory in horizontal scaling
    
- Database scaling is more complex than application scaling
    

---

## 9. Interview Questions with Answers

### 1. What is the difference between vertical and horizontal scaling?

Answer:  
Vertical scaling increases the power of a single server.  
Horizontal scaling adds more servers to distribute the load.

---

### 2. Which scaling approach is better?

Answer:  
For large-scale distributed systems, horizontal scaling is better because it provides better availability and virtually unlimited growth.

---

### 3. Why is horizontal scaling more fault tolerant?

Answer:  
If one server fails, other servers continue serving requests. In vertical scaling, failure of the single server causes total downtime.

---

### 4. Why must services be stateless in horizontal scaling?

Answer:  
Because any request can go to any server. If state is stored in memory of one server, scaling becomes difficult.

---

## 10. Summary

Vertical Scaling:

- Increase machine power
    
- Simple
    
- Limited
    
- Single point of failure
    

Horizontal Scaling:

- Add more machines
    
- Complex
    
- Highly scalable
    
- Fault tolerant
    

For modern distributed systems, horizontal scaling is preferred.

---

Next logical topic:  
[[High Availability]] which directly connects to scaling.