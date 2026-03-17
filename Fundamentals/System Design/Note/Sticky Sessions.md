## 1. Introduction

Sticky Sessions, also called **Session Affinity**, is a load balancing technique where a user's requests are always routed to the **same backend server** during a session.

In simple terms:

Once a user is connected to a server, all their future requests go to the **same server**.

---

## 2. Why Sticky Sessions Are Used

Some applications store **session data in server memory**.

Examples of session data:

- Login information
    
- Shopping cart
    
- User preferences
    

If requests go to different servers, the session data may not exist there.

Sticky sessions ensure the user continues interacting with the **same server**.

---

## 3. How Sticky Sessions Work

Step 1  
User sends request.

Step 2  
Load balancer selects a backend server.

Step 3  
Load balancer creates a mapping:

User → Specific Server

Step 4  
All future requests from that user are routed to the same server.

---

## 4. Techniques Used to Implement Sticky Sessions

### 4.1 Cookies

Load balancer adds a cookie in the user's browser.

Example:

Set-Cookie: SERVERID=server2

Next requests use this cookie to route traffic to the same server.

---

### 4.2 IP Hash

Server selection is based on the client's IP address.

Hash(Client IP) → Server

The same IP is always mapped to the same server.

---

### 4.3 Session ID

Load balancer routes traffic using the session identifier stored in requests.

---

## 5. Problems with Sticky Sessions

Although useful, sticky sessions have drawbacks.

### 5.1 Reduced Scalability

Traffic may become uneven.

Example:  
If many users are assigned to one server, it becomes overloaded.

---

### 5.2 Fault Tolerance Issues

If the server storing the session crashes:

- Session data is lost
    
- User may be logged out
    

---

### 5.3 Difficult Horizontal Scaling

Sticky sessions make load distribution less flexible.

---

## 6. Modern Solution: Stateless Architecture

Modern systems avoid sticky sessions by using **stateless servers**.

Session data is stored in shared storage such as:

- Redis
    
- Database
    
- Distributed cache
    

Example flow:

Client → Load Balancer → Any Server → Redis Session Store

Now any server can handle the request.

---

## 7. Example Architecture

With Sticky Sessions:

Client → Load Balancer → Server A (always)

Without Sticky Sessions:

Client → Load Balancer → Server A / Server B / Server C  
All servers share session data in Redis.

---

## 8. When Sticky Sessions Are Used

Sticky sessions may be used when:

- Legacy applications store sessions locally
    
- Migrating old monolithic systems
    
- Temporary solution before moving to stateless design
    

However, modern cloud architectures usually avoid them.

---

## 9. Interview Questions with Answers

### 1. What are sticky sessions?

Answer:  
Sticky sessions ensure that all requests from a user are routed to the same backend server during a session.

---

### 2. Why are sticky sessions used?

Answer:  
They are used when session data is stored in the server's memory and must remain available for subsequent requests.

---

### 3. What is the main problem with sticky sessions?

Answer:  
They reduce scalability and create uneven load distribution across servers.

---

### 4. What is the alternative to sticky sessions?

Answer:  
Stateless architecture with centralized session storage like Redis or databases.

---

### 5. How can sticky sessions be implemented?

Answer:  
Using cookies, IP hashing, or session identifiers.

---

## 10. Summary

Sticky sessions bind a user to the same server for the duration of a session.

Advantages:

- Maintains session state easily
    

Disadvantages:

- Poor scalability
    
- Uneven load distribution
    
- Reduced fault tolerance
    

Modern distributed systems prefer **stateless servers with centralized session storage**.

---

Next topic:  
[[Global Traffic Routing]] which explains how traffic is routed across **different geographic regions**.