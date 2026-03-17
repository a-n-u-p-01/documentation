## 1. Definition

### Latency

Latency is the time taken to process a single request from start to finish.

In simple terms:  
It is how fast the system responds.

Example:  
If an API takes 200 milliseconds to respond, the latency is 200 ms.

---

### Throughput

Throughput is the number of requests a system can handle per unit time.

In simple terms:  
It is how many requests the system can process per second.

Measured as:

- Requests per second (RPS)
    
- Queries per second (QPS)
    
- Transactions per second (TPS)
    

---

## 2. Key Difference

Latency measures speed of one request.  
Throughput measures capacity of many requests.

Latency = Time per request  
Throughput = Requests per second

---

## 3. Real-Life Example

Consider a highway.

Latency:  
How long it takes one car to travel from start to end.

Throughput:  
How many cars pass per minute.

A road can have:

- Low latency but low throughput (very fast but small road)
    
- High throughput but high latency (traffic jam but many cars)
    

---

## 4. Mathematical Understanding

Latency is measured in:

- Milliseconds (ms)
    
- Seconds
    

Throughput is measured in:

- Requests/second
    
- Transactions/second
    

Example:

If a server processes:  
1 request in 100 ms

Then theoretical maximum throughput:

1 second = 1000 ms  
1000 / 100 = 10 requests per second

Throughput ≈ 10 RPS

---

## 5. Relationship Between Latency and Throughput

They are related but not the same.

Reducing latency often increases throughput.  
But sometimes increasing throughput may increase latency.

Example:  
If too many requests come at once:

- Throughput may increase
    
- Latency may also increase due to queuing
    

---

## 6. Types of Latency

Network Latency  
Time taken for data to travel across the network.

Application Latency  
Time taken by server logic.

Database Latency  
Time taken to query the database.

Disk Latency  
Time taken to read/write to disk.

Total latency is the sum of all these.

---

## 7. Why This Matters in System Design

Some systems prioritize low latency:

- Stock trading platforms
    
- Real-time gaming
    
- Payment processing
    

Some systems prioritize high throughput:

- Batch processing
    
- Data analytics
    
- Logging systems
    

In system design, you must decide which is more important.

---

## 8. How to Reduce Latency

- Use caching
    
- Use CDN
    
- Optimize database queries
    
- Use indexing
    
- Reduce network calls
    
- Use faster hardware
    

---

## 9. How to Increase Throughput

- Horizontal scaling
    
- Load balancing
    
- Asynchronous processing
    
- Queue-based architecture
    
- Connection pooling
    
- Parallel processing
    

---

## 10. Interview Questions with Answers

### 1. What is the difference between latency and throughput?

Answer:  
Latency is the time taken to process one request.  
Throughput is the number of requests processed per second.

---

### 2. Can a system have low latency but low throughput?

Answer:  
Yes. A system may respond quickly to a small number of requests but fail under heavy load.

---

### 3. Why might throughput increase but latency also increase?

Answer:  
When too many requests arrive, they get queued. While more total requests are processed, individual request time increases.

---

### 4. Which is more important: latency or throughput?

Answer:  
It depends on the system requirements. Real-time systems prioritize latency. High-volume processing systems prioritize throughput.

---

## 11. Summary

Latency measures how fast a system responds.  
Throughput measures how much work a system can handle.

Both are critical metrics in designing scalable and high-performance systems.

---
