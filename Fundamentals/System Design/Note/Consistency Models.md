## 1. Introduction

Consistency models define the rules that determine when changes made to data become visible to users in a distributed system.

In simple terms:

A consistency model tells us how "fresh" or "up-to-date" the data is when we read it.

Consistency is a spectrum. It is not just strong or weak.

---

## 2. Why Consistency Models Matter

In distributed systems:

- Data is replicated across multiple nodes
    
- Network delays exist
    
- Nodes may temporarily disagree
    

Consistency models define what guarantees the system provides when reading data.

Different applications require different levels of consistency.

---

## 3. Types of Consistency Models

### 3.1 Strong Consistency

After a write completes, all future reads return the latest value.

Guarantee:  
No stale reads.

Example:  
Bank balance system.

Advantage:  
Data is always correct.

Disadvantage:  
Higher latency due to synchronization between nodes.

---

### 3.2 Eventual Consistency

After a write, the system will eventually become consistent, but reads may temporarily return stale data.

Guarantee:  
If no new writes occur, all nodes will eventually have the same value.

Example:  
	Social media likes count.

Advantage:  
High availability and low latency.

Disadvantage:  
Temporary inconsistency possible.

---

### 3.3 Read-Your-Writes Consistency

A user always sees their own updates.

Example:  
You update your profile name and immediately refresh.  
You must see the updated name.

Used in:  
User-centric applications.

---

### 3.4 Monotonic Reads

If a user reads a value, future reads will not return older values.

Example:  
If you see version 5 of data, you will not later see version 3.

Prevents time-travel inconsistencies.

---

### 3.5 Causal Consistency

Operations that are causally related are seen in the correct order.

Example:  
If A posts a message and B replies,  
Others should not see the reply before the original message.

---

### 3.6 Linearizability

Strongest form of consistency.

Operations appear as if they occurred instantly in a single global order.

Often considered equivalent to strong consistency in interviews.

---

## 4. Strong vs Eventual Consistency Comparison

Strong Consistency:

- No stale reads
    
- Higher latency
    
- More coordination
    
- Lower availability during partition
    

Eventual Consistency:

- Possible stale reads
    
- Lower latency
    
- High availability
    
- Better scalability
    

---

## 5. Real-World Examples

Banking System:  
Requires strong consistency.

Messaging System:  
May use read-your-writes consistency.

Social Media Feed:  
Often uses eventual consistency.

Distributed Cache:  
Usually eventual consistency.

---

## 6. Trade-Offs

Stronger consistency:

- More network communication
    
- More coordination
    
- Higher latency
    
- Lower availability
    

Weaker consistency:

- Better performance
    
- Higher availability
    
- Possible temporary inconsistency
    

Choosing the right model depends on business requirements.

---

## 7. Consistency in Databases

Relational databases:  
Often provide strong consistency.

NoSQL databases:  
Often provide eventual consistency but allow configurable consistency levels.

Some systems allow tuning:

- Strong consistency for critical operations
    
- Eventual consistency for non-critical operations
    

---

## 8. Interview Questions with Answers

### 1. What is a consistency model?

Answer:  
A consistency model defines the guarantees about visibility and ordering of updates in a distributed system.

---

### 2. What is the difference between strong consistency and eventual consistency?

Answer:  
Strong consistency guarantees that every read returns the latest write.  
Eventual consistency allows temporary stale reads but ensures data eventually becomes consistent.

---

### 3. When would you choose eventual consistency?

Answer:  
In systems where high availability and low latency are more important than immediate consistency, such as social media feeds.

---

### 4. What is read-your-writes consistency?

Answer:  
It guarantees that a user always sees their own recent updates.

---

### 5. Why does strong consistency increase latency?

Answer:  
Because nodes must coordinate and synchronize before confirming writes.

---

## 9. Summary

Consistency models define how up-to-date data is in a distributed system.

Main types:

- Strong consistency
    
- Eventual consistency
    
- Read-your-writes
    
- Monotonic reads
    
- Causal consistency
    

Choosing the right consistency model depends on application requirements.

---
