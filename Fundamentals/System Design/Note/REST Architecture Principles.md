## 1. Introduction

REST stands for Representational State Transfer.

It is an architectural style for designing networked applications, especially web APIs.

REST is built on top of HTTP and uses standard HTTP methods.

In simple terms:

REST defines rules for designing scalable and maintainable web services.

---

## 2. Key Idea Behind REST

In REST:

- Everything is treated as a resource.
    
- Each resource is identified by a URL.
    
- Clients interact with resources using HTTP methods.
    

Example:

Resource: User  
URL: /users/101

---

## 3. Core REST Principles

### 3.1 Client-Server Architecture

Client and server are separated.

Client:

- Handles UI
    
- Sends requests
    

Server:

- Handles business logic
    
- Manages database
    

This separation improves scalability and maintainability.

---

### 3.2 Statelessness

Each request from client must contain all necessary information.

Server does not store client session state.

Example:  
Each API call includes authentication token.

Advantages:

- Easier horizontal scaling
    
- No server-side session storage
    
- Better reliability
    

---

### 3.3 Uniform Interface

REST uses standard HTTP methods consistently.

Common HTTP methods:

GET → Retrieve resource  
POST → Create resource  
PUT → Update resource  
DELETE → Remove resource  
PATCH → Partial update

Example:

GET /users → Get all users  
GET /users/101 → Get specific user  
POST /users → Create new user  
PUT /users/101 → Update user  
DELETE /users/101 → Delete user

---

### 3.4 Resource-Based URLs

URLs represent nouns (resources), not actions.

Correct:

- /orders
    
- /products
    
- /users/101
    

Incorrect:

- /getUsers
    
- /createUser
    

REST focuses on resources, not verbs.

---

### 3.5 Representation

A resource can have multiple representations:

- JSON
    
- XML
    
- HTML
    

Most modern APIs use JSON.

---

### 3.6 Layered System

Client does not know if it is connected directly to server or through intermediaries.

Possible layers:

- Load balancer
    
- Reverse proxy
    
- API gateway
    
- Cache
    

This improves scalability and security.

---

### 3.7 Cacheability

Responses should define whether they are cacheable.

Example:  
GET requests can be cached.

Caching improves performance and reduces server load.

---

## 4. REST vs SOAP

REST:

- Lightweight
    
- Uses HTTP
    
- Usually JSON
    
- Stateless
    
- Simple
    

SOAP:

- Protocol-based
    
- XML heavy
    
- More complex
    

Modern systems prefer REST.

---

## 5. REST in System Design

REST supports:

- Stateless architecture
    
- Horizontal scaling
    
- Microservices communication
    
- Clear API contracts
    

Most backend systems expose REST APIs.

---

## 6. REST Best Practices

- Use proper HTTP methods
    
- Use correct HTTP status codes
    
- Use meaningful resource names
    
- Keep APIs stateless
    
- Version APIs (/v1/users)
    
- Use pagination for large data
    

---

## 7. Example REST API Design

For a product system:

GET /products  
GET /products/10  
POST /products  
PUT /products/10  
DELETE /products/10

Status Codes:

200 → Success  
201 → Created  
400 → Bad Request  
404 → Not Found  
500 → Internal Error

---

## 8. Interview Questions with Answers

### 1. What is REST?

Answer:  
REST is an architectural style for designing scalable and stateless web services using HTTP protocols.

---

### 2. What are the key principles of REST?

Answer:  
Client-server separation, statelessness, uniform interface, layered system, cacheability, and resource-based URLs.

---

### 3. Why is statelessness important in REST?

Answer:  
It allows easy horizontal scaling because any request can be handled by any server instance.

---

### 4. What is the difference between PUT and POST?

Answer:  
POST creates a new resource.  
PUT updates an existing resource or creates it if it does not exist.

---

### 5. Why should URLs use nouns instead of verbs?

Answer:  
Because REST represents resources, and actions are defined by HTTP methods.

---

## 9. Summary

REST is an architectural style for building scalable web APIs.

Key concepts:

- Resources identified by URLs
    
- Use of HTTP methods
    
- Stateless communication
    
- Client-server separation
    
- Cacheable responses
    

REST is the foundation of most modern backend APIs.

---

Next topic:  
[[TCP vs UDP]] which explains transport-level communication below HTTP.