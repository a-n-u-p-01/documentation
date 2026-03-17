## 1. Introduction

WebSockets provide a full-duplex, persistent communication channel between client and server over a single TCP connection.

In simple terms:

WebSockets allow real-time, two-way communication between client and server.

Unlike HTTP, the connection remains open.

---

## 2. Why WebSockets Are Needed

HTTP is:

- Request-response based
    
- Stateless
    
- Client must initiate every request
    

Problem:

If server wants to push updates (e.g., chat message), client must continuously poll the server.

Polling causes:

- High latency
    
- Increased server load
    
- Inefficient network usage
    

WebSockets solve this problem.

---

## 3. How WebSockets Work

Step 1: Client sends HTTP request with "Upgrade" header.

Step 2: Server agrees to upgrade connection.

Step 3: TCP connection becomes persistent WebSocket connection.

After upgrade:

- Both client and server can send messages anytime.
    
- No need to reopen connections.
    

---

## 4. WebSocket vs HTTP

HTTP:

- Request-response model
    
- Connection closes after response
    
- Client always initiates communication
    

WebSocket:

- Persistent connection
    
- Full-duplex communication
    
- Server can push data anytime
    

---

## 5. Real-World Use Cases

Chat applications  
Live notifications  
Online gaming  
Stock price updates  
Real-time dashboards  
Collaborative editing tools

These systems require instant updates.

---

## 6. WebSocket Protocol

WebSocket uses:

- ws:// (non-secure)
    
- wss:// (secure, over TLS)
    

After handshake, data is exchanged in frames.

WebSockets operate over TCP.

---

## 7. Advantages

- Low latency
    
- Real-time communication
    
- Reduced overhead
    
- Efficient for frequent updates
    

---

## 8. Disadvantages

- Harder to scale
    
- Connection management complexity
    
- Stateful connection
    
- Load balancing requires special handling
    

Unlike REST APIs, WebSockets are stateful.

---

## 9. WebSockets in System Design

Key considerations:

### 1. Connection Management

Thousands or millions of open connections require memory.

### 2. Horizontal Scaling

Requires sticky sessions or shared message broker.

### 3. Message Broker

Often integrated with:

- Kafka
    
- Redis Pub/Sub
    
- RabbitMQ
    

For distributing messages across servers.

---

## 10. WebSockets vs Polling vs Long Polling

Polling:  
Client repeatedly sends requests at fixed intervals.

Long Polling:  
Client waits until server responds, then reconnects.

WebSocket:  
Persistent two-way connection.

WebSockets are most efficient for real-time systems.

---

## 11. Scaling WebSocket Systems

To scale:

- Use load balancer with sticky sessions
    
- Use distributed message brokers
    
- Store connection metadata in shared cache
    
- Use horizontal scaling with coordination
    

Chat systems and trading platforms rely heavily on this.

---

## 12. Interview Questions with Answers

### 1. What is WebSocket?

Answer:  
WebSocket is a protocol that enables full-duplex, real-time communication over a persistent TCP connection.

---

### 2. How is WebSocket different from HTTP?

Answer:  
HTTP is request-response based and stateless.  
WebSocket maintains a persistent two-way connection.

---

### 3. Why are WebSockets used in chat applications?

Answer:  
Because they allow instant message delivery without repeated polling.

---

### 4. What challenges arise when scaling WebSockets?

Answer:  
Managing persistent connections, load balancing, and distributing messages across servers.

---

### 5. What is the difference between ws and wss?

Answer:  
ws is non-secure WebSocket.  
wss is secure WebSocket over TLS.

---

## 13. Summary

WebSockets enable real-time, bidirectional communication between client and server.

They are ideal for:

- Chat systems
    
- Live notifications
    
- Online gaming
    
- Real-time updates
    

However, they introduce scaling and state management challenges.

---
