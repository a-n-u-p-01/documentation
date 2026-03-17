## 1. Overview

Kubernetes is a container orchestration platform used to deploy, manage, and scale containerized applications (like Spring Boot apps running in Docker).

Core responsibilities:

- Container deployment
    
- Scaling
    
- Load balancing
    
- Self-healing
    
- Configuration management
    

---

## 2. Deployment

A Deployment manages Pods and ensures the desired number of replicas are running.

Used for:

- Rolling updates
    
- Auto-healing
    
- Scaling
    

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: springboot-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: springboot
  template:
    metadata:
      labels:
        app: springboot
    spec:
      containers:
        - name: app
          image: anupam/springboot-app:1.0
          ports:
            - containerPort: 8080
```

Key Points:

- replicas: number of running instances
    
- selector: connects Deployment to Pods
    
- image: Docker image
    
- containerPort: exposed port inside container
    

Scaling:

```
kubectl scale deployment springboot-app --replicas=5
```

---

## 3. Service

Pods are temporary and their IP changes. A Service provides a stable endpoint.

Types of Services:

1. ClusterIP (default)  
    Internal communication inside cluster.
    
2. NodePort  
    Exposes service on a port of each node.
    
3. LoadBalancer  
    Creates cloud load balancer (AWS, GCP, etc.).
    

Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: springboot-service
spec:
  type: ClusterIP
  selector:
    app: springboot
  ports:
    - port: 80
      targetPort: 8080
```

Flow:  
Client → Service → Pod

---

## 4. ConfigMap

Used to store non-sensitive configuration data.

Example use cases:

- application.properties
    
- environment variables
    
- external configuration
    

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: mysql-service
  DB_NAME: prod_db
```

Use in Pod:

```yaml
env:
  - name: DB_HOST
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: DB_HOST
```

Purpose:

- Decouple config from image
    
- Environment-specific configuration
    

---

## 5. Secrets

Used to store sensitive data:

- Passwords
    
- API keys
    
- Database credentials
    

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=
```

Note:  
Values must be Base64 encoded.

Use in Pod:

```yaml
env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: DB_PASSWORD
```

Important:  
Secrets should never be stored in Git in plain form.

---

## 6. Real-World Flow (Spring Boot)

1. Dockerize Spring Boot app.
    
2. Push image to Docker registry.
    
3. Create Deployment with image.
    
4. Create Service for access.
    
5. Store DB config in ConfigMap.
    
6. Store DB password in Secret.
    
7. Deploy using kubectl apply -f.
    

---

## 7. Production Best Practices

- Use replicas > 1
    
- Use readiness and liveness probes
    
- Use Secrets for credentials
    
- Use resource limits (CPU, memory)
    
- Use rolling updates
    

---

# Interview Questions and Answers

1. What is a Deployment in Kubernetes?  
    A Deployment manages Pods and ensures the desired number of replicas are running with support for rolling updates.
    
2. What problem does a Service solve?  
    Pods have dynamic IP addresses. A Service provides a stable endpoint for communication.
    
3. Difference between ConfigMap and Secret?  
    ConfigMap stores non-sensitive data. Secret stores sensitive data encoded in Base64.
    
4. What are the types of Kubernetes Services?  
    ClusterIP, NodePort, and LoadBalancer.
    
5. How does Kubernetes achieve high availability?  
    By running multiple replicas of Pods and automatically restarting failed containers.
    
6. Why is ConfigMap important in microservices?  
    It allows environment-specific configuration without rebuilding Docker images.