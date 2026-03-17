## 1. Introduction

A Reverse Proxy is a server that sits between clients and backend servers, forwarding client requests to the appropriate backend server.

In simple terms:

Client → Reverse Proxy → Application Server

The client does not communicate directly with backend servers.

---

## 2. Why Reverse Proxy is Needed

Without reverse proxy:

- Clients directly connect to application servers
    
- Harder to scale
    
- Harder to secure
    
- No centralized traffic control
    

With reverse proxy:

- Central entry point
    
- Better security
    
- Load balancing
    
- SSL termination
    
- Caching
    

---

## 3. Forward Proxy vs Reverse Proxy

Forward Proxy:

- Sits between client and internet
    
- Hides client identity
    
- Used for filtering or anonymity
    

Reverse Proxy:

- Sits between internet and servers
    
- Hides server identity
    
- Protects backend systems
    

---

## 4. How Reverse Proxy Works

Step 1: Client sends request to server domain.

Step 2: Reverse proxy receives request.

Step 3: Proxy forwards request to appropriate backend server.

Step 4: Backend processes request and sends response to proxy.

Step 5: Proxy sends response back to client.

Client never directly interacts with backend.

---

## 5. Common Reverse Proxy Servers

- Nginx
    
- Apache HTTP Server
    
- HAProxy
    
- Cloudflare
    
- AWS Application Load Balancer
    

These are widely used in production systems.

---

## 6. Key Functions of a Reverse Proxy

### 6.1 Load Balancing

Distributes traffic across multiple servers.

Improves scalability and availability.

---

### 6.2 SSL Termination

Handles HTTPS encryption and decryption.

Backend servers can communicate internally using HTTP.

Reduces load on application servers.

---

### 6.3 Security

- Hides internal IP addresses
    
- Blocks malicious traffic
    
- Implements rate limiting
    
- Protects against DDoS
    

---

### 6.4 Caching

Stores frequently requested responses.

Reduces load on backend servers.

---

### 6.5 Compression

Compresses responses before sending to client.

Reduces bandwidth usage.

---

## 7. Reverse Proxy in Microservices

In microservices architecture:

Reverse proxy can act as:

- API Gateway
    
- Traffic router
    
- Central authentication point
    

Example flow:

Client → Reverse Proxy → Service A  
Client → Reverse Proxy → Service B

---

## 8. Reverse Proxy vs Load Balancer

Load Balancer:  
Primarily distributes traffic.

Reverse Proxy:  
Can perform load balancing plus caching, SSL termination, and security filtering.

Many load balancers are implemented as reverse proxies.

---

## 9. Real-World Architecture Example

Modern architecture:

Client  
↓  
CDN  
↓  
Reverse Proxy / Load Balancer  
↓  
Application Servers  
↓  
Database

Reverse proxy is a key layer in production systems.

---

## 10. Advantages

- Improves security
    
- Enhances scalability
    
- Centralizes traffic management
    
- Simplifies certificate management
    
- Enables caching
    

---

## 11. Interview Questions with Answers

### 1. What is a reverse proxy?

Answer:  
A reverse proxy is a server that forwards client requests to backend servers and returns responses to clients, acting as an intermediary.

---

### 2. Why use a reverse proxy?

Answer:  
To improve security, enable load balancing, handle SSL termination, and centralize traffic control.

---

### 3. How does a reverse proxy improve security?

Answer:  
It hides internal server IP addresses and filters incoming traffic before reaching backend systems.

---

### 4. What is SSL termination?

Answer:  
SSL termination is when the reverse proxy handles HTTPS encryption and decrypts traffic before forwarding it to backend servers.

---

### 5. Is a load balancer the same as a reverse proxy?

Answer:  
A load balancer is often implemented as a reverse proxy, but reverse proxies offer additional features like caching and security filtering.

---

## 12. Summary

A reverse proxy is an intermediary server between clients and backend servers.

Key benefits:

- Load balancing
    
- Security
    
- SSL termination
    
- Caching
    
- Traffic management
    

It is a critical component in scalable, production-ready systems.

---

Next topic:  
[[Documentation/Fundamentals/System Design/Note/CDN Basics]] which sits even before reverse proxy in large-scale systems.