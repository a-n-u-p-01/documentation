## 1. Introduction

Load balancers operate at different layers of the OSI model.

The two most common types are:

- Layer 4 (Transport Layer) Load Balancing
    
- Layer 7 (Application Layer) Load Balancing
    

Understanding the difference is important in system design interviews.

---

## 2. Layer 4 (L4) Load Balancing

Layer 4 operates at the Transport Layer.

It makes routing decisions based on:

- IP address
    
- Port number
    
- TCP/UDP protocol
    

It does not inspect the actual application data.

---

### How L4 Load Balancer Works

Client sends request →  
Load balancer checks IP and port →  
Forwards request to one backend server.

It does not look at:

- URL
    
- HTTP headers
    
- Cookies
    

---

### Advantages of L4

- Faster (less processing)
    
- Lower latency
    
- Simple routing
    
- Good for TCP/UDP traffic
    

---

### Disadvantages of L4

- Cannot route based on URL
    
- Cannot inspect HTTP headers
    
- No content-based routing
    

---

### Example Use Case 

- Simple web applications
    
- TCP-based services
    
- Database load balancing
    

---

## 3. Layer 7 (L7) Load Balancing

Layer 7 operates at the Application Layer.

It can inspect:

- HTTP headers
    
- URL path
    
- Cookies
    
- Request body
    

It makes intelligent routing decisions.

---

### How L7 Load Balancer Works

Client sends HTTP request →  
Load balancer inspects request content →  
Routes request based on rules.

Example:

/api/users → Server A  
/api/orders → Server B

---

### Advantages of L7

- Content-based routing
    
- URL-based routing
    
- Cookie-based routing
    
- SSL termination
    
- API gateway capabilities
    

---

### Disadvantages of L7

- Slightly higher latency
    
- More CPU usage
    
- More complex
    

---

### Example Use Case

Microservices architecture:

- User service
    
- Order service
    
- Payment service
    

L7 load balancer routes requests to correct service.

---

## 4. Comparison Table

Layer 4:

- Works at transport layer
    
- Routes using IP and port
    
- Faster
    
- Less intelligent
    

Layer 7:

- Works at application layer
    
- Routes using HTTP data
    
- More flexible
    
- Slightly slower
    

---

## 5. Real-World Example

Suppose:

example.com/users  
example.com/orders

L4 cannot differentiate between these.

L7 can route:

/users → User service  
/orders → Order service

---

## 6. L4 vs L7 in System Design

Use L4 when:

- You need high performance
    
- Simple routing is enough
    

Use L7 when:

- You have microservices
    
- You need path-based routing
    
- You need SSL termination
    
- You need API gateway features
    

Modern cloud load balancers often support both.

---

## 7. Interview Questions with Answers

### 1. What is the difference between L4 and L7 load balancing?

Answer:  
L4 routes traffic based on IP and port.  
L7 routes traffic based on application-level data like URL and headers.

---

### 2. Which is faster, L4 or L7?

Answer:  
L4 is faster because it does not inspect application data.

---

### 3. Why is L7 used in microservices architecture?

Answer:  
Because it can route requests based on URL paths to different services.

---

### 4. Can L7 handle SSL termination?

Answer:  
Yes. L7 load balancers can decrypt HTTPS traffic and forward plain HTTP to backend servers.

---

### 5. Does L4 understand HTTP?

Answer:  
No. L4 operates at transport layer and does not inspect HTTP content.

---

## 8. Summary

Layer 4:

- Fast
    
- IP-based routing
    
- Limited intelligence
    

Layer 7:

- Smart routing
    
- URL/header-based decisions
    
- Suitable for microservices
    

In modern distributed systems, L7 load balancing is commonly used.

---
