## 1. Introduction

Load balancing algorithms determine how incoming requests are distributed among multiple servers.

When a request reaches the load balancer, the algorithm decides which backend server should handle that request.

The goal is to:

- Distribute traffic evenly
    
- Prevent server overload
    
- Improve system performance
    
- Maintain high availability
    

---

## 2. Why Load Balancing Algorithms Are Important

Without proper algorithms:

- Some servers may become overloaded
    
- Other servers may remain idle
    
- Response time increases
    
- System performance decreases
     

Load balancing algorithms ensure efficient resource utilization.

---

## 3. Types of Load Balancing Algorithms

Load balancing algorithms are generally divided into two categories:

- Static algorithms
    
- Dynamic algorithms
    

---

## 4. Static Load Balancing Algorithms

Static algorithms distribute requests without considering server load.

They assume all servers have equal capacity.

---

### 4.1 Round Robin

Requests are distributed sequentially among servers.

Example:

Server list:  
Server1, Server2, Server3

Requests distribution:

Request1 → Server1  
Request2 → Server2  
Request3 → Server3  
Request4 → Server1

Advantages:

- Simple
    
- Easy to implement
    

Disadvantages:

- Does not consider server load.
    

---

### 4.2 Weighted Round Robin

Each server is assigned a weight based on its capacity.

Servers with higher capacity receive more requests.

Example:

Server1 weight = 3  
Server2 weight = 1

Distribution:

Server1, Server1, Server1, Server2

Advantages:

- Better distribution when servers have different capacities.
    

---

## 5. Dynamic Load Balancing Algorithms

Dynamic algorithms consider server load before routing requests.

They monitor real-time server performance.

---

### 5.1 Least Connections

The request is sent to the server with the fewest active connections.

Example:

Server1 → 20 connections  
Server2 → 5 connections

New request → Server2

Advantages:

- Good for long-lived connections.
    

---

### 5.2 Least Response Time

Load balancer selects the server with:

- Lowest response time
    
- Fewest active connections
    

This improves overall system performance.

---

### 5.3 IP Hash

Server is selected based on the client's IP address.

Hash(Client IP) → Server

Advantages:

- Same user always connects to same server.
    

Used for:  
Session persistence.

---

## 6. Sticky Sessions (Session Affinity)

Sticky sessions ensure that a user's requests always go to the same server.

This is useful when session data is stored in server memory.

However, it reduces load balancing efficiency.

Modern systems prefer stateless architecture instead.

---

## 7. Choosing the Right Algorithm

Choice depends on system requirements.

Round Robin:  
Good for equal servers.

Weighted Round Robin:  
Useful when servers have different capacities.

Least Connections:  
Good for applications with long sessions.

IP Hash:  
Useful when session persistence is needed.

---

## 8. Real-World Examples

Web Servers:  
Round Robin or Least Connections.

Streaming Services:  
Least Response Time.

Session-based Applications:  
IP Hash.

Microservices:  
Often use Round Robin or Least Connections.

---

## 9. Interview Questions with Answers

### 1. What is a load balancing algorithm?

Answer:  
It is a strategy used by a load balancer to distribute incoming requests across multiple servers.

---

### 2. What is the difference between static and dynamic algorithms?

Answer:  
Static algorithms distribute traffic without considering server load.  
Dynamic algorithms consider real-time server load before routing requests.

---

### 3. What is Round Robin load balancing?

Answer:  
It distributes requests sequentially among servers.

---

### 4. What is Least Connections algorithm?

Answer:  
It routes requests to the server with the fewest active connections.

---

### 5. Why is Weighted Round Robin used?

Answer:  
To distribute requests according to server capacity.

---

## 10. Summary

Load balancing algorithms determine how traffic is distributed across servers.

Common algorithms:

- Round Robin
    
- Weighted Round Robin
    
- Least Connections
    
- Least Response Time
    
- IP Hash
    

Choosing the correct algorithm improves performance, scalability, and reliability of distributed systems.

---
