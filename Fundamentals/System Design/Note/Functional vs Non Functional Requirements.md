## 1. Definition

### Functional Requirements

Functional requirements describe **what the system should do**.  
They define the features and behavior of the system.

They answer the question:  
What services or functions must the system provide?

Examples:

- Users can register and log in.
    
- Users can upload images.
    
- Users can transfer money.
    
- Admin can delete posts.
    

Functional requirements are directly related to business logic.

---

### Non-Functional Requirements

Non-functional requirements describe **how the system should behave**.  
They define quality attributes and constraints of the system.

They answer the question:  
How well should the system perform its functions?

Examples:

- The system should handle 1 million users.
    
- API response time should be under 200 ms.
    
- System availability should be 99.99%.
    
- Data should be encrypted in transit.
    

Non-functional requirements focus on system quality.

---

## 2. Key Differences

Functional Requirements:

- Define system features
    
- Focus on business logic
    
- Usually validated using functional testing
    
- Describe inputs, outputs, and behavior
    

Non-Functional Requirements:

- Define system quality
    
- Focus on performance and constraints
    
- Usually validated using performance and stress testing
    
- Describe system characteristics
    

Simple Difference:  
Functional = What  
Non-Functional = How well

---

## 3. Common Types of Non-Functional Requirements

### Performance

- Latency
    
- Throughput
    
- Response time
    

### Scalability

- Ability to handle increased load
    

### Availability

- System uptime (e.g., 99.99%)
    

### Reliability

- Consistent correct operation
    

### Security

- Authentication
    
- Authorization
    
- Data encryption
    

### Maintainability

- Easy to modify and extend
    

### Usability

- Easy to use interface
    

---

## 4. Real Example

Consider a Payment System.

Functional Requirements:

- User can make a payment.
    
- User can view transaction history.
    
- System sends payment confirmation.
    

Non-Functional Requirements:

- Payment should be processed within 2 seconds.
    
- System should handle 10,000 transactions per second.
    
- System should be available 99.99% of the time.
    
- Data must be encrypted.
    

---

## 5. Importance in System Design

Before designing any system, you must clearly identify:

1. Functional requirements to understand features.
    
2. Non-functional requirements to design architecture.
    

Non-functional requirements often influence architecture more than functional requirements.

For example:

- High scalability may require microservices.
    
- High availability may require replication.
    
- Low latency may require caching.
    

---

## 6. Interview Questions with Answers

### 1. What is the difference between functional and non-functional requirements?

Answer:  
Functional requirements describe what the system does.  
Non-functional requirements describe how the system performs and behaves.

---

### 2. Which is more important in system design?

Answer:  
Both are important. However, non-functional requirements heavily influence architectural decisions such as scaling, database choice, caching, and replication.

---

### 3. Give examples of non-functional requirements in a social media application.

Answer:

- System should support 50 million users.
    
- Feed load time should be under 300 ms.
    
- System should have 99.9% uptime.
    
- Data should be encrypted.
    

---

## 7. Summary

Functional Requirements:  
Define system features and business logic.

Non-Functional Requirements:  
Define system quality attributes such as performance, scalability, availability, and security.

Understanding both clearly is the first step in any system design process.