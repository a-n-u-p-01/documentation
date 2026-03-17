## 1. Introduction

A Load Balancer is a system component that distributes incoming network traffic across multiple servers.

In simple terms:

A load balancer ensures that no single server gets overloaded.

It acts as a traffic manager between clients and servers.

---

## 2. Why a Load Balancer is Needed

When an application grows:

- More users send requests
    
- A single server cannot handle all traffic
    
- Server may crash due to overload
    

A load balancer solves these problems.

---

## 3. Problems Without a Load Balancer

### 3.1 Single Point of Failure

If only one server exists and it crashes:

- Entire system goes down
    

---

### 3.2 Limited Scalability

One server has hardware limits:

- CPU
    
- RAM
    
- Disk
    
- Network
    

System cannot scale beyond machine capacity.

---

### 3.3 Uneven Traffic Distribution

If traffic is not distributed properly:

- Some servers become overloaded
    
- Others remain idle
    

---

## 4. How a Load Balancer Works

Step 1:  
Client sends request.

Step 2:  
Request reaches load balancer.

Step 3:  
Load balancer selects a backend server.

Step 4:  
Server processes request and returns response.

Step 5:  
Load balancer sends response back to client.

Client does not know which server handled the request.

---

## 5. Benefits of Using a Load Balancer

### 5.1 High Availability

If one server fails:

- Load balancer redirects traffic to healthy servers.
    

---

### 5.2 Scalability

You can add more servers:

- Load balancer distributes traffic automatically.
    

---

### 5.3 Fault Tolerance

Unhealthy servers are removed from traffic pool.

---

### 5.4 Better Performance

Distributes traffic evenly, reducing response time.

---

### 5.5 SSL Termination

Load balancer can handle HTTPS encryption.

Backend servers receive decrypted traffic.

Improves performance.

---

## 6. Types of Load Balancing

### 6.1 Hardware Load Balancer

Physical device used in data centers.

Expensive but powerful.

---

### 6.2 Software Load Balancer

Software-based solution.

Examples:

- NGINX
    
- HAProxy
    
- Cloud load balancers
    

Most modern systems use software load balancers.

---

## 7. Load Balancing Algorithms

Common strategies:

Round Robin  
Requests distributed sequentially.

Least Connections  
Server with fewest active connections selected.

IP Hash  
Same client IP always goes to same server.

Weighted Round Robin  
Servers assigned weights based on capacity.

---

## 8. Layer 4 vs Layer 7 Load Balancing

Layer 4 (Transport Layer):  
Routes based on IP and port.

Layer 7 (Application Layer):  
Routes based on HTTP headers, URL path, cookies.

Layer 7 is more intelligent and flexible.

---

## 9. Load Balancer in System Design

In a scalable system:

Client → Load Balancer → Multiple Application Servers → Database

Load balancer is essential for horizontal scaling.

---

## 10. Interview Questions with Answers

### 1. Why do we need a load balancer?

Answer:  
To distribute traffic across multiple servers, prevent overload, improve availability, and enable horizontal scaling.

---

### 2. What happens if one server behind load balancer fails?

Answer:  
The load balancer detects failure using health checks and stops routing traffic to that server.

---

### 3. What is a Single Point of Failure?

Answer:  
A component whose failure causes the entire system to go down. Load balancers help eliminate this at the application server level.

---

### 4. What is the difference between Layer 4 and Layer 7 load balancing?

Answer:  
Layer 4 routes traffic based on IP and port.  
Layer 7 routes traffic based on application-level data like URL or headers.

---

### 5. Can a load balancer itself become a single point of failure?

Answer:  
Yes. To prevent this, systems use multiple load balancers with redundancy.

---

## 11. Summary

A Load Balancer:

- Distributes traffic
    
- Prevents server overload
    
- Enables horizontal scaling
    
- Improves high availability
    
- Removes unhealthy servers
    

It is a critical component in scalable distributed systems.

---

