## 1. Introduction

DNS (Domain Name System) is responsible for converting human-readable domain names into IP addresses.

In simple terms:

DNS is the phonebook of the Internet.

Computers communicate using IP addresses, not domain names.  
DNS makes it possible for users to type example.com instead of a numeric IP address.

---

## 2. Why DNS is Needed

Without DNS:

You would need to remember IP addresses like:

142.250.183.14

Instead of:

google.com

DNS maps domain names to IP addresses so browsers can locate the correct server.

---

## 3. Step-by-Step DNS Resolution Process

When you type:

[www.example.com](http://www.example.com/)

The following steps occur:

### Step 1: Browser Cache Check

The browser checks its local cache.  
If the IP is already stored and not expired, it uses it.

If not found, continue.

---

### Step 2: OS Cache Check

The operating system checks its DNS cache.

If found → return IP.  
If not → continue.

---

### Step 3: Resolver (ISP DNS Server)

The request goes to a DNS resolver, usually provided by your ISP.

If resolver has cached result → returns IP.  
If not → it performs recursive lookup.

---

### Step 4: Root DNS Server

Resolver asks the Root DNS server.

Root server does not know the IP but responds with the address of the TLD server.

---

### Step 5: TLD (Top-Level Domain) Server

Example:  
For [www.example.com](http://www.example.com/)

TLD is .com

TLD server responds with the authoritative name server for example.com.

---

### Step 6: Authoritative Name Server

The authoritative DNS server holds the actual domain record.

It responds with the IP address of [www.example.com](http://www.example.com/).

---

### Step 7: IP Returned to Browser

The resolver sends the IP address back to the browser.

The browser can now connect to the server using that IP.

---

## 4. Types of DNS Servers

### 1. Recursive Resolver

Performs full lookup on behalf of client.

### 2. Root Server

Top-level server in DNS hierarchy.

### 3. TLD Server

Handles domain extensions like .com, .org.

### 4. Authoritative Server

Stores actual domain-to-IP mapping.

---

## 5. DNS Caching

DNS responses are cached at multiple levels:

- Browser cache
    
- OS cache
    
- ISP resolver cache
    

Each DNS record has a TTL (Time To Live).

TTL defines how long the record remains cached.

Caching reduces latency and DNS load.

---

## 6. DNS Record Types

Common DNS record types:

A Record  
Maps domain to IPv4 address.

AAAA Record  
Maps domain to IPv6 address.

CNAME  
Alias for another domain.

MX Record  
Mail server record.

TXT Record  
Used for verification and security.

---

## 7. What Happens After DNS?

After getting the IP:

- Browser establishes TCP connection
    
- Sends HTTP request
    
- Receives response
    

DNS is only the first step in accessing a website.

---

## 8. DNS and System Design

DNS improves:

- Scalability (through caching)
    
- Performance (reduced lookup time)
    
- Reliability (multiple DNS servers worldwide)
    

Large systems often use:

- Geo-based DNS routing
    
- Failover DNS
    
- Load balancing through DNS
    

---

## 9. Interview Questions with Answers

### 1. What happens during DNS resolution?

Answer:  
The browser checks cache, then resolver queries root server, TLD server, and authoritative server to obtain the IP address.

---

### 2. What is TTL in DNS?

Answer:  
TTL (Time To Live) defines how long a DNS record is cached before it must be refreshed.

---

### 3. What is an Authoritative DNS server?

Answer:  
It is the server that stores the official domain-to-IP mapping.

---

### 4. What is the difference between recursive and iterative query?

Answer:  
Recursive query means the resolver performs the entire lookup process.  
Iterative query means each server responds with the next server to query.

---

### 5. Why is DNS caching important?

Answer:  
It reduces latency and lowers load on DNS infrastructure.

---

## 10. Summary

DNS converts domain names into IP addresses.

Resolution process:  
Browser cache → OS cache → Resolver → Root → TLD → Authoritative server → IP returned.

DNS is essential for web communication and system scalability.

---
