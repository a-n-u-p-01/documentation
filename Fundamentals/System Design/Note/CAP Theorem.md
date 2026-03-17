## 1. Introduction

CAP Theorem is one of the most important concepts in distributed systems.

It states:

In a distributed system, during a network partition, you can guarantee only two out of the following three properties:

- Consistency (C)
    
- Availability (A)
    
- Partition Tolerance (P)
    

You cannot guarantee all three at the same time.

---

## 2. Understanding the Three Properties

### 2.1 Consistency (C)

Every read receives the most recent write.

This means:  
All nodes see the same data at the same time.

If a user updates data, all future reads must return the updated value.

Example:  
Bank account balance must always be correct.

---

### 2.2 Availability (A)

Every request receives a response, even if it may not contain the latest data.

System continues to operate and respond.

Example:  
Social media feed still loads even if some data is slightly outdated.

---

### 2.3 Partition Tolerance (P)

The system continues to operate even if there is a network partition between nodes.

A network partition occurs when communication between nodes is lost.

In distributed systems, partition tolerance is mandatory because network failures are unavoidable.

---

## 3. What CAP Theorem Actually Says

When a network partition happens:

You must choose between:

- Consistency  
    OR
    
- Availability
    

You cannot have both.

Since network partitions are unavoidable, modern distributed systems must choose between C and A.

---

## 4. CP vs AP Systems

### CP (Consistency + Partition Tolerance)

System guarantees consistency.  
During partition, some requests may be rejected to maintain correctness.

Example:  
Banking systems.

Behavior:  
Better data correctness, lower availability during partition.

---

### AP (Availability + Partition Tolerance)

System guarantees availability.  
During partition, it may return stale data.

Example:  
Social media feeds, DNS systems.

Behavior:  
Always responds, but data may be temporarily inconsistent.

---

## 5. Why CA is Not Practical

CA (Consistency + Availability without Partition Tolerance) assumes no network failures.

In distributed systems, network failures are unavoidable.

Therefore:  
Partition tolerance is not optional.

---

## 6. Real Example

Imagine two database nodes:

Node A and Node B.

If the network link between them breaks:

If you choose Consistency:  
One node must stop serving requests.

If you choose Availability:  
Both nodes continue serving requests, but data may diverge.

This is the CAP trade-off.

---

## 7. CAP Theorem in Real Databases

CP Systems:

- Traditional relational databases (in strict mode)
    
- Systems using strong consistency protocols
    

AP Systems:

- Cassandra
    
- DynamoDB
    
- CouchDB
    

Different databases make different trade-offs.

---

## 8. Misconceptions

1. CAP applies only during network partition.  
    Not during normal operation.
    
2. You do not permanently choose two.  
    Trade-offs apply when partition occurs.
    
3. CAP does not mean systems are either fully CP or fully AP.  
    Modern systems allow configurable consistency levels.
    

---

## 9. Interview Questions with Answers

### 1. What does CAP Theorem state?

Answer:  
In a distributed system, during a network partition, you can guarantee only two out of Consistency, Availability, and Partition Tolerance.

---

### 2. Why is Partition Tolerance mandatory?

Answer:  
Because network failures are unavoidable in distributed systems.

---

### 3. What happens during a network partition in a CP system?

Answer:  
The system may reject some requests to maintain data consistency.

---

### 4. What happens during a network partition in an AP system?

Answer:  
The system continues serving requests but may return stale or inconsistent data.

---

### 5. Which type of system would you use for a banking application?

Answer:  
CP system because data consistency is critical.

---

## 10. Summary

CAP Theorem explains a fundamental trade-off in distributed systems.

During network partition:

- You can choose Consistency or Availability.
    
- Partition tolerance is mandatory.
    

Understanding CAP is essential for making architectural decisions in distributed systems.

---
