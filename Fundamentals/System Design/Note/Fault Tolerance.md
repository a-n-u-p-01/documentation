## 1. Definition

Fault Tolerance is the ability of a system to continue operating correctly even when one or more components fail.

In simple terms:

A fault-tolerant system does not stop working when something breaks.

---

## 2. Why Fault Tolerance is Important

In distributed systems, failures are normal:

- Servers crash
    
- Networks fail
    
- Databases become unreachable
    
- Services timeout
    

A well-designed system assumes failures will happen and handles them gracefully.

---

## 3. Fault Tolerance vs High Availability

High Availability:  
System minimizes downtime.

Fault Tolerance:  
System continues operating without interruption even when components fail.

Example:

High Availability:  
System may briefly restart but recovers quickly.

Fault Tolerance:  
System continues running with no noticeable interruption.

Fault tolerance is stronger than high availability.

---

## 4. Types of Failures in Distributed Systems

### Hardware Failure

Server crashes, disk failure.

### Network Failure

Packet loss, timeouts, partitioning.

### Software Failure

Bugs, memory leaks, crashes.

### Dependency Failure

External service is down.

A fault-tolerant system must handle all these.

---

## 5. Techniques to Achieve Fault Tolerance

### 5.1 Redundancy

Duplicate critical components.

Example:

- Multiple application servers
    
- Multiple database replicas
    

If one fails, others continue.

---

### 5.2 Replication

Data is stored in multiple locations.

Example:  
Primary-replica database.

If primary fails, replica takes over.

---

### 5.3 Failover Mechanism

Automatic switching to backup component when failure occurs.

Manual failover increases downtime.  
Automatic failover improves fault tolerance.

---

### 5.4 Circuit Breaker Pattern

Prevents repeated calls to a failing service.

If service fails repeatedly:

- Circuit opens
    
- Requests are stopped temporarily
    
- System uses fallback response
    

Prevents cascading failures.

---

### 5.5 Timeouts and Retries

If a request takes too long:

- Timeout is triggered
    
- Retry with backoff
    

Prevents resource exhaustion.

---

### 5.6 Graceful Degradation

System provides limited functionality instead of failing completely.

Example:  
If recommendation service fails,  
System still shows homepage without recommendations.

---

## 6. Distributed System Challenges

In distributed systems:

- Nodes communicate over network
    
- Network is unreliable
    
- Partial failures occur
    

Fault tolerance requires:

- Monitoring
    
- Automatic recovery
    
- Data consistency management
    

---

## 7. Active-Active vs Active-Passive

Active-Active:  
Multiple systems actively handle traffic.  
If one fails, others continue.

Active-Passive:  
One active system, one standby.  
Standby takes over when active fails.

Active-Active offers better fault tolerance.

---

## 8. Trade-Offs

Fault tolerance increases:

- System complexity
    
- Infrastructure cost
    
- Data synchronization challenges
    

There is always a balance between cost and reliability.

---

## 9. Interview Questions with Answers

### 1. What is fault tolerance?

Answer:  
Fault tolerance is the ability of a system to continue functioning correctly even when some components fail.

---

### 2. How is fault tolerance achieved in distributed systems?

Answer:  
Through redundancy, replication, failover mechanisms, circuit breakers, retries, and monitoring.

---

### 3. What is the difference between fault tolerance and high availability?

Answer:  
High availability minimizes downtime.  
Fault tolerance ensures continuous operation even during failures.

---

### 4. What is cascading failure?

Answer:  
When failure in one component causes failures in other components, leading to system-wide outage.

---

### 5. Why are timeouts important in fault-tolerant systems?

Answer:  
Timeouts prevent threads and resources from being blocked indefinitely when a dependency fails.

---

## 10. Summary

Fault tolerance means designing systems that expect failure and continue operating despite it.

Key principles:

- Remove single points of failure
    
- Use redundancy and replication
    
- Implement failover
    
- Use circuit breakers
    
- Handle partial failures
    

Fault tolerance is essential in large-scale distributed systems.

---
