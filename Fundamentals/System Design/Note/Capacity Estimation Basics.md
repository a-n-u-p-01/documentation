## 1. Definition

Capacity Estimation is the process of estimating the system resources required to handle expected traffic and data growth.

It helps answer questions like:

- How many users will use the system?
    
- How many requests per second will come?
    
- How much storage is needed?
    
- How many servers are required?
    

Capacity estimation is usually done at the beginning of system design.

---

## 2. Why Capacity Estimation is Important

Without estimation:

- Servers may crash under load.
    
- Databases may become slow.
    
- Infrastructure cost may become too high.
    
- Scaling decisions may be incorrect.
    

Proper estimation helps in:

- Designing scalable systems
    
- Reducing cost
    
- Avoiding bottlenecks
    
- Planning future growth
    

---

## 3. Key Metrics in Capacity Estimation

### 1. Number of Users

Total registered users  
Daily active users (DAU)  
Monthly active users (MAU)

### 2. Requests Per Second (RPS or QPS)

How many requests the system receives per second.

Formula:  
RPS = Total Requests per Day / (24 × 60 × 60)

### 3. Read vs Write Ratio

Example:

- 80% reads
    
- 20% writes
    

This helps choose database and caching strategy.

### 4. Storage Estimation

Storage per user × Total users

Example:  
If each user generates 1 MB per day  
And 1 million users use the system

Daily storage = 1 MB × 1,000,000  
= 1,000,000 MB  
= 1,000 GB  
= 1 TB per day

---

## 4. Basic Estimation Example

Example: Design a URL Shortener

Assumptions:

- 10 million daily active users
    
- Each user creates 2 short URLs per day
    
- Each user clicks 10 times per day
    

Step 1: Write Requests  
10M × 2 = 20M writes per day

Write RPS:  
20,000,000 / 86,400 ≈ 231 writes/sec

Step 2: Read Requests  
10M × 10 = 100M reads per day

Read RPS:  
100,000,000 / 86,400 ≈ 1157 reads/sec

Step 3: Storage Estimation  
Assume each record needs 500 bytes

Daily storage:  
20M × 500 bytes = 10,000,000,000 bytes  
≈ 10 GB per day

---

## 5. Bandwidth Estimation

Bandwidth = Response Size × Requests Per Second

If:  
Response size = 1 KB  
RPS = 1000

Bandwidth = 1000 KB/sec  
≈ 1 MB/sec

This helps estimate network requirements.

---

## 6. Peak Traffic Estimation

Traffic is not uniform.

Peak traffic may be:  
2x or 3x average traffic.

Always design for peak load, not average load.

Example:  
If average RPS = 1000  
Peak RPS may be = 3000

System should handle 3000 RPS safely.

---

## 7. Important Concepts

### Horizontal Scaling

Adding more servers to handle increased load.

### Vertical Scaling

Increasing CPU/RAM of existing server.

### Load Factor

System should not run at 100% capacity.  
Keep 60–70% utilization for safety.

---

## 8. Common Interview Questions with Answers

### 1. Why is capacity estimation important in system design?

Answer:  
It helps determine infrastructure requirements, prevents system overload, and ensures scalability and cost efficiency.

---

### 2. How do you calculate requests per second?

Answer:  
Divide total daily requests by 86,400 seconds.

---

### 3. Why do we design for peak load instead of average load?

Answer:  
Traffic is not constant. Designing for average load may cause system failure during traffic spikes.

---

### 4. What factors influence storage estimation?

Answer:

- Data size per record
    
- Number of users
    
- Growth rate
    
- Retention policy
    

---

## 9. Summary

Capacity Estimation Basics involve:

- Estimating users
    
- Calculating requests per second
    
- Estimating read/write ratio
    
- Estimating storage
    
- Estimating bandwidth
    
- Designing for peak traffic
    

It is a critical step before designing scalable architecture.