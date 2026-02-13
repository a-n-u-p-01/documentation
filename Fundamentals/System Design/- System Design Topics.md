## **System Design Fundamentals**

- [[Note/What is System Design]]
    
- [[Note/Functional vs Non-Functional Requirements]]
    
- [[Note/Capacity Estimation]]
    
- [[Note/Traffic Estimation]]
    
- [[Note/Latency vs Throughput]]
    
- [[Note/Scalability Basics]]
    
- [[Note/CAP Theorem]]
    
- [[Note/PACELC Theorem]]
    
- [[Note/Single Point of Failure]]
    
- [[Note/Redundancy and Replication]]
    
- [[Note/Fault Tolerance]]
    
- [[Note/High Availability Architecture]]
    

---

## **Networking & Communication**

- [[Note/HTTP HTTPS]]
    
- [[Note/HTTP Methods]]
    
- [[Note/REST vs gRPC vs GraphQL]]
    
- [[Note/WebSockets]]
    
- [[Note/API Gateway]]
    
- [[Note/Service to Service]]
    

(Removed OSI, TCP/UDP, Polling → rarely asked for backend interviews unless networking role.)

---

## **Load Balancing**

- [[Note/Load Balancing]]
    
- [[Note/L4 vs L7]]
    
- [[Note/Load Balancing Algorithms]]
    
- [[Note/Reverse Proxy]]
    

(Removed deep NGINX — concept matters more than tool internals.)

---

## **Caching (🔥 Critical Section)**

- [[Note/Caching Fundamentals]]
    
- [[Note/Cache Aside]]
    
- [[Note/Write Through vs Write Back]]
    
- [[Note/TTL and Eviction]]
    
- [[Note/Redis]]
    
- [[Note/Distributed Caching]]
    
- [[Note/Cache Invalidation]]
    

👉 Do NOT skip this section.

Many system design interviews secretly test caching knowledge.

---

## **Databases**

### Fundamentals

- [[Note/SQL vs NoSQL]]
    
- [[Note/ACID]]
    
- [[Note/Database Indexing]]
    
- [[Note/Query Optimization]]
    

### Scaling

- [[Note/Database Replication]]
    
- [[Note/Master-Slave]]
    
- [[Note/Sharding]]
    
- [[Note/Consistent Hashing]]
    

### Distributed Data

- [[Note/Distributed Transactions]]
    
- [[Note/Saga Pattern]]
    
- [[Note/CQRS]]
    

(Removed BASE, Federation, Multi-leader — lower interview frequency.)

---

## **Message Queues & Event Systems**

🔥 Senior-signal topics.

- [[Note/Why Message Queues]]
    
- [[Note/Pub-Sub]]
    
- [[Note/Queue vs Stream]]
    
- [[Note/Kafka]]
    
- [[Note/Exactly Once vs At Least Once]]
    
- [[Note/Idempotency]]
    
- [[Note/Dead Letter Queue]]
    
- [[Note/Event-Driven Architecture]]
    

---

## **Microservices**

- [[Note/Monolith vs Microservices]]
    
- [[Note/When Not Microservices]]
    
- [[Note/Database per Service]]
    
- [[Note/Service Discovery]]
    
- [[Note/API Composition]]
    
- [[Note/Strangler Fig]]
    

(Removed BFF — situational pattern.)

---

## **Reliability & Resilience (🔥 Interview Gold)**

- [[Note/Retry Strategies]]
    
- [[Note/Circuit Breaker]]
    
- [[Note/Bulkhead]]
    
- [[Note/Timeouts]]
    
- [[Note/Rate Limiting]]
    
- [[Note/Graceful Degradation]]
    

(Removed throttling — overlaps with rate limiting.)

---

## **Security**

- [[Note/Auth vs AuthZ]]
    
- [[Note/JWT]]
    
- [[Note/OAuth2]]
    
- [[Note/API Security]]
    
- [[Note/Secrets Management]]
    

(Removed Zero Trust — advanced architecture topic.)

---

## **Observability (Most Engineers Skip — Big Advantage)**

- [[Note/Logging Strategy]]
    
- [[Note/Metrics]]
    
- [[Note/Distributed Tracing]]
    
- [[Note/SLI SLO SLA]]
    

(Alerting is operational — not core for interviews.)

---

## **Storage Systems**

- [[Note/Object Storage]]
    
- [[Note/CDN]]
    
- [[Note/Hot vs Cold]]
    

(Removed Blob — overlaps with object storage concepts.)

---

## **Search**

- [[Note/Search Engines]]
    
- [[Note/Inverted Index]]
    
- [[Note/Elasticsearch]]
    

---

## 🔥 **MOST IMPORTANT — Real System Designs**

👉 Interviewers care MORE about these than theory.

- [[Note/URL Shortener]]
    
- [[Note/Design WhatsApp]]
    
- [[Note/Design Twitter]]
    
- [[Note/Design Instagram]]
    
- [[Note/Design Notification System]]
    
- [[Note/Design Rate Limiter]]
    
- [[Note/Design Distributed Cache]]
    

(Removed Uber & YouTube — lower probability for early-career interviews.)

---

## ❌ Intentionally Removed (For Now)

Learn later — NOT needed to become top 10% quickly:

- Vector Clocks
    
- CRDTs
    
- Gossip Protocol
    
- Leader Election
    
- Multi-Leader
    
- Federation
    

👉 These are **staff-level topics**, not 1–2 year engineer topics.

---