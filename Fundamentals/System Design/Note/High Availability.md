## 1. Definition

High Availability (HA) refers to a system’s ability to remain operational and accessible for a high percentage of time, even when some components fail.

In simple terms:

High Availability means the system stays up and running with minimal downtime.

---

## 2. Why High Availability is Important

In real-world systems:

- Users expect services 24/7
    
- Downtime causes revenue loss
    
- Downtime damages reputation
    
- Critical systems (banking, healthcare) cannot afford failure
    

Therefore, systems must be designed to minimize downtime.

---

## 3. Measuring Availability

Availability is usually measured as a percentage.

Formula:

Availability = Uptime / (Uptime + Downtime)

Examples:

99% availability → ~3.65 days downtime per year  
99.9% availability → ~8.7 hours downtime per year  
99.99% availability → ~52 minutes downtime per year  
99.999% availability → ~5 minutes downtime per year

This is often referred to as "number of nines".

---

## 4. Single Point of Failure (SPOF)

A Single Point of Failure is a component whose failure brings down the entire system.

Examples:

- Only one application server
    
- Only one database server
    
- Only one load balancer
    

To achieve high availability, eliminate single points of failure.

---

## 5. Techniques to Achieve High Availability

### 5.1 Redundancy

Run multiple instances of components.

Example:

- Multiple application servers
    
- Multiple database replicas
    

If one fails, others continue working.

---

### 5.2 Load Balancing

A load balancer distributes traffic across multiple servers.

If one server fails, traffic is redirected to healthy servers.

---

### 5.3 Database Replication

Primary-Replica setup:

- Primary handles writes
    
- Replicas handle reads
    
- If primary fails, failover occurs
    

---

### 5.4 Failover Mechanism

Automatic switching to a backup system when the primary fails.

Example:  
If primary database crashes → replica becomes new primary.

---

### 5.5 Health Checks

Load balancers periodically check server health.

If a server is unhealthy, traffic is stopped.

---

### 5.6 Multi-Region Deployment

Deploy system in multiple geographic regions.

If one region goes down:

- Traffic is routed to another region.
    

This improves availability and disaster recovery.

---

## 6. High Availability vs Fault Tolerance

High Availability:  
System minimizes downtime.

Fault Tolerance:  
System continues functioning even during failure.

Fault tolerance is stronger than high availability.

Example:

- HA: brief interruption but system recovers quickly.
    
- Fault Tolerance: no visible interruption.
    

---

## 7. High Availability in Microservices

In microservices architecture:

- Each service should have multiple instances
    
- Services should be stateless
    
- Circuit breakers prevent cascading failures
    
- Auto-scaling handles traffic spikes
    

---

## 8. Common Causes of Downtime

- Hardware failure
    
- Network failure
    
- Software bugs
    
- Database crash
    
- Overload
    
- Human error
    

Good system design anticipates these.

---

## 9. Interview Questions with Answers

### 1. What is High Availability?

Answer:  
High Availability is the ability of a system to remain operational for a high percentage of time by eliminating single points of failure and using redundancy.

---

### 2. How do you achieve high availability in a web application?

Answer:  
By using multiple application servers, load balancers, database replication, failover mechanisms, and health checks.

---

### 3. What is a Single Point of Failure?

Answer:  
A component whose failure causes the entire system to go down.

---

### 4. What is failover?

Answer:  
Failover is the automatic switching to a backup component when the primary component fails.

---

### 5. What does 99.99% availability mean?

Answer:  
It means the system can have approximately 52 minutes of downtime per year.

---

## 10. Summary

High Availability ensures that systems remain accessible even during failures.

Key concepts:

- Remove single points of failure
    
- Use redundancy
    
- Use load balancing
    
- Use replication
    
- Implement failover
    
- Monitor health
    

High availability is a core requirement for production-grade distributed systems.

---
