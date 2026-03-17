## 1. Introduction

CDN stands for Content Delivery Network.

A CDN is a globally distributed network of servers that deliver content to users from the nearest geographic location.

In simple terms:

A CDN brings content closer to users to reduce latency and improve performance.

---

## 2. Why CDN is Needed

Without CDN:

User in India requests content from server in US.  
Data travels long distance → High latency → Slow loading.

With CDN:

Content is cached in a nearby edge server.  
User gets response from nearest location → Faster load time.

---

## 3. How CDN Works

Step 1: User requests a website.

Step 2: DNS directs the request to the nearest CDN edge server.

Step 3: If content is cached:  
CDN returns content immediately.

Step 4: If not cached:  
CDN fetches content from origin server, stores it, and serves the user.

---

## 4. Key Components

### 4.1 Origin Server

The main server where original content is stored.

### 4.2 Edge Servers

Servers distributed across different geographic regions.

They cache and deliver content.

---

## 5. What Type of Content CDN Caches

Static Content:

- Images
    
- CSS
    
- JavaScript
    
- Videos
    
- Fonts
    

Some CDNs also cache dynamic content using advanced rules.

---

## 6. Benefits of CDN

### 6.1 Reduced Latency

Content served from nearest location.

---

### 6.2 Improved Performance

Faster page load times.

---

### 6.3 Reduced Load on Origin Server

CDN handles majority of traffic.

---

### 6.4 High Availability

If one server fails, traffic is routed to another.

---

### 6.5 DDoS Protection

Many CDNs provide traffic filtering and attack protection.

---

## 7. CDN in System Architecture

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

CDN reduces traffic reaching backend.

---

## 8. CDN vs Reverse Proxy

Reverse Proxy:  
Usually inside your infrastructure.

CDN:  
Global distributed external network.

CDN sits closer to users geographically.

---

## 9. Popular CDN Providers

- Cloudflare
    
- Akamai
    
- Fastly
    
- Amazon CloudFront
    
- Google Cloud CDN
    

Used by large-scale applications worldwide.

---

## 10. CDN and Caching

CDN caching is controlled using:

- Cache-Control headers
    
- TTL (Time To Live)
    
- ETag
    
- Last-Modified
    

Proper caching strategy improves performance significantly.

---

## 11. CDN in System Design Interviews

When designing:

- Social media platform
    
- Video streaming service
    
- E-commerce website
    
- News website
    

Always mention CDN for static content delivery.

It shows scalability awareness.

---

## 12. Interview Questions with Answers

### 1. What is a CDN?

Answer:  
A CDN is a distributed network of servers that cache and deliver content from locations closer to users to reduce latency.

---

### 2. Why is CDN important in large-scale systems?

Answer:  
It reduces latency, improves performance, reduces load on origin servers, and improves availability.

---

### 3. What type of content is best suited for CDN?

Answer:  
Static content such as images, CSS, JavaScript, and videos.

---

### 4. How does CDN improve availability?

Answer:  
If one edge server fails, traffic is redirected to another nearby edge server.

---

### 5. Does CDN replace backend servers?

Answer:  
No. CDN caches and delivers content but origin servers still handle dynamic processing.

---

## 13. Summary

CDN is a distributed caching layer placed near users.

Key benefits:

- Reduced latency
    
- Better performance
    
- Lower server load
    
- Higher availability
    
- DDoS protection
    

CDN is essential in scalable internet-scale systems.

---

You have now completed Networking & Web Fundamentals.

Next major section:  
Load Balancing & Traffic Management

Starting with:  
[[Why Load Balancer is Needed]]