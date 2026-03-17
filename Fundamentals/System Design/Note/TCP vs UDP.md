## 1. Introduction

TCP and UDP are transport layer protocols in the TCP/IP model.

They are responsible for delivering data between applications over the network.

After DNS resolution and IP routing, data is transmitted using either TCP or UDP.

---

## 2. What is TCP?

TCP stands for Transmission Control Protocol.

It is a connection-oriented and reliable protocol.

Before sending data, TCP establishes a connection between client and server.

---

### Key Features of TCP

- Connection-oriented (3-way handshake)
    
- Reliable data delivery
    
- Ordered delivery
    
- Error detection and retransmission
    
- Flow control
    
- Congestion control
    

---

### How TCP Works (Simplified)

Step 1: Connection setup (3-way handshake)  
Step 2: Data transfer  
Step 3: Connection termination

If a packet is lost:

- TCP detects it
    
- Retransmits it
    

---

### Where TCP is Used

- HTTP / HTTPS
    
- FTP
    
- SMTP (Email)
    
- Database connections
    

Reliable delivery is required in these cases.

---

## 3. What is UDP?

UDP stands for User Datagram Protocol.

It is connectionless and unreliable.

UDP sends data without establishing a connection.

---

### Key Features of UDP

- No connection setup
    
- No guarantee of delivery
    
- No ordering guarantee
    
- No retransmission
    
- Faster than TCP
    
- Lower overhead
    

UDP prioritizes speed over reliability.

---

### Where UDP is Used

- Video streaming
    
- Online gaming
    
- Voice calls (VoIP)
    
- DNS queries
    

In these cases, speed is more important than perfect reliability.

---

## 4. TCP vs UDP Comparison

TCP:

- Reliable
    
- Ordered
    
- Slower
    
- Higher overhead
    
- Used for critical data
    

UDP:

- Unreliable
    
- Unordered
    
- Faster
    
- Lower overhead
    
- Used for real-time data
    

---

## 5. 3-Way Handshake (TCP)

TCP connection setup:

1. SYN → Client sends request
    
2. SYN-ACK → Server acknowledges
    
3. ACK → Client confirms
    

After this, connection is established.

UDP does not have this process.

---

## 6. Reliability in TCP

TCP ensures:

- Data arrives correctly
    
- Data arrives in order
    
- Lost packets are retransmitted
    

UDP does not provide these guarantees.

Applications using UDP must handle reliability themselves if needed.

---

## 7. Performance Considerations

TCP:

- Higher latency due to handshake and acknowledgments
    
- Good for file transfer and transactions
    

UDP:

- Lower latency
    
- Suitable for real-time applications
    

Example:  
In video streaming, losing a few frames is acceptable.  
In banking transactions, losing data is unacceptable.

---

## 8. TCP and System Design

In system design interviews:

Mention:

- HTTP runs on TCP
    
- HTTPS runs on TCP
    
- DNS usually uses UDP
    
- Real-time systems often use UDP
    
- TCP ensures reliability
    

Understanding where each protocol fits is important.

---

## 9. Interview Questions with Answers

### 1. What is the main difference between TCP and UDP?

Answer:  
TCP is connection-oriented and reliable.  
UDP is connectionless and faster but unreliable.

---

### 2. Why is TCP reliable?

Answer:  
Because it uses acknowledgments, retransmissions, sequencing, flow control, and congestion control.

---

### 3. Why is UDP faster than TCP?

Answer:  
Because it does not establish a connection and does not guarantee delivery.

---

### 4. Why does DNS use UDP?

Answer:  
Because DNS queries are small and require low latency. Speed is more important than reliability.

---

### 5. Which protocol would you choose for a payment system?

Answer:  
TCP, because reliability and ordered delivery are critical.

---

## 10. Summary

TCP:  
Reliable, ordered, connection-oriented.

UDP:  
Fast, connectionless, no delivery guarantee.

Choice depends on application requirements.

---

Next topic:  
[[WebSockets]] which builds on TCP for real-time communication.