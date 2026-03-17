## 1. Introduction

Health checks are mechanisms used by load balancers to determine whether backend servers are functioning properly.

In simple terms:

A health check ensures that the load balancer sends traffic only to healthy servers.

If a server becomes unhealthy, the load balancer stops sending requests to it.

---

## 2. Why Health Checks Are Needed

In distributed systems:

- Servers may crash
    
- Services may stop responding
    
- Network issues may occur
    

Without health checks:

Requests may be sent to failed servers, causing errors and poor user experience.

Health checks help maintain system reliability and availability.

---

## 3. How Health Checks Work

Step 1  
Load balancer periodically sends a request to backend servers.

Step 2  
Server responds with a success status.

Step 3  
If response is correct → server is marked healthy.

Step 4  
If response fails repeatedly → server is marked unhealthy.

Step 5  
Load balancer stops sending traffic to that server.

---

## 4. Types of Health Checks

### 4.1 Active Health Checks

The load balancer actively sends requests to servers at regular intervals.

Example:

HTTP GET /health

If response is 200 OK → server is healthy.

This is the most common type.

---

### 4.2 Passive Health Checks

Load balancer monitors real client requests.

If a server returns too many errors or timeouts, it is marked unhealthy.

Passive checks rely on real traffic behavior.

---

## 5. Health Check Parameters

Health checks usually have configurable parameters.

Interval  
How often health checks are performed.

Timeout  
Maximum time allowed for response.

Failure Threshold  
Number of failed checks before marking server unhealthy.

Success Threshold  
Number of successful checks required to mark server healthy again.

Example:

Check every 10 seconds  
Timeout 3 seconds  
3 failures → server removed

---

## 6. Health Check Endpoint

Applications often provide a dedicated endpoint for health checks.

Example:

GET /health  
GET /status  
GET /ping

Example response:

{  
"status": "UP"  
}

This endpoint confirms that the application is running properly.

---

## 7. Importance in Microservices

In microservices architecture:

- Many services run independently
    
- Services may fail or restart
    

Health checks help:

- Detect failures quickly
    
- Prevent routing traffic to unhealthy services
    
- Improve system resilience
    

Kubernetes also uses health checks for containers.

---

## 8. Health Checks in Kubernetes

Kubernetes uses two important probes:

Liveness Probe  
Checks if container is alive.

Readiness Probe  
Checks if container is ready to receive traffic.

If a container fails liveness probe, it is restarted.

If it fails readiness probe, it is removed from load balancing.

---

## 9. Example Architecture

Client  
↓  
Load Balancer  
↓  
Application Servers

Load balancer continuously monitors servers.

If Server 2 fails health check:

Client → Load Balancer → Server 1 / Server 3

Server 2 receives no traffic until it becomes healthy again.

---

## 10. Interview Questions with Answers

### 1. What is a health check in load balancing?

Answer:  
A health check is a mechanism used to determine whether backend servers are functioning properly so that traffic is routed only to healthy servers.

---

### 2. What happens when a server fails a health check?

Answer:  
The load balancer marks the server as unhealthy and stops routing traffic to it.

---

### 3. What is the difference between active and passive health checks?

Answer:  
Active checks send periodic test requests.  
Passive checks rely on real traffic failures to detect unhealthy servers.

---

### 4. Why are health check endpoints used?

Answer:  
They allow load balancers to verify that the application is running and capable of handling requests.

---

### 5. What is the difference between liveness and readiness probes in Kubernetes?

Answer:  
Liveness checks if a container is alive.  
Readiness checks if it is ready to receive traffic.

---

## 11. Summary

Health checks help ensure that traffic is sent only to healthy servers.

Key points:

- Detect failed servers
    
- Prevent routing traffic to unhealthy nodes
    
- Improve reliability and availability
    
- Common in load balancers and container orchestration systems
    

Health checks are essential for maintaining stable distributed systems.

---

Next topic:  
[[Sticky Sessions]] which explains session persistence in load balancing.