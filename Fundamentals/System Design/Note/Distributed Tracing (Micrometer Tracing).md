# 1. Introduction

Distributed Tracing is used to track a single request as it travels across multiple microservices.

In microservices:

User request → API Gateway → user-service → order-service → payment-service → database

Without tracing:

- Hard to debug slow requests
    
- Difficult to identify which service failed
    
- No visibility into call chain
    

Distributed tracing solves this by assigning tracking identifiers.

---

# 2. Core Concepts

Trace ID  
A unique ID assigned to a complete request flow.

Span  
A single unit of work within a trace (e.g., service call).

Trace = Collection of spans.

Example:

Trace ID: abc123

Spans:

- Gateway span
    
- user-service span
    
- order-service span
    
- payment-service span
    

---

# 3. Micrometer Tracing

Micrometer Tracing replaces Spring Cloud Sleuth in modern Spring Boot.

It integrates with:

- Zipkin
    
- Jaeger
    
- OpenTelemetry
    
- Prometheus (metrics)
    

---

# 4. Add Dependencies

For Spring Boot 3+:

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing-bridge-brave</artifactId>
</dependency>

<dependency>
    <groupId>io.zipkin.reporter2</groupId>
    <artifactId>zipkin-reporter-brave</artifactId>
</dependency>
```

---

# 5. How It Works

When a request enters system:

1. Gateway generates Trace ID.
    
2. Trace ID is added to HTTP headers.
    
3. Each service receives and propagates header.
    
4. Each service creates its own span.
    
5. Data sent to tracing backend (Zipkin/Jaeger).
    

Common headers:

```
traceparent
X-B3-TraceId
X-B3-SpanId
```

---

# 6. Automatic Propagation

Spring Boot automatically:

- Injects trace ID into logs
    
- Propagates headers in Feign/WebClient
    
- Creates spans for HTTP calls
    

You do not manually pass trace ID.

---

# 7. Example Flow

User request:

Trace ID: 987xyz

Gateway logs:  
[traceId=987xyz]

user-service logs:  
[traceId=987xyz]

order-service logs:  
[traceId=987xyz]

Now you can search logs using same trace ID.

---

# 8. Viewing Traces (Zipkin Example)

Run Zipkin.

Configure:

```properties
management.tracing.sampling.probability=1.0
management.zipkin.tracing.endpoint=http://localhost:9411/api/v2/spans
```

Open:

```
http://localhost:9411
```

You see full request timeline:

- Duration
    
- Service call order
    
- Slow service detection
    

---

# 9. Why Distributed Tracing Is Important

- Debug latency issues
    
- Detect bottlenecks
    
- Monitor request path
    
- Improve observability
    
- Root cause analysis
    

Especially important in:

- Cloud-native systems
    
- Microservices
    
- High-traffic applications
    

---

# 10. Tracing in Kubernetes

In Kubernetes:

- Each pod generates spans.
    
- Traces collected centrally.
    
- Use OpenTelemetry + Jaeger/Zipkin.
    
- Works across multiple pods automatically.
    

No special Kubernetes config required.

---

# 11. Difference Between Logging and Tracing

Logging:  
Records events in each service separately.

Tracing:  
Connects logs across services using Trace ID.

Tracing answers:  
"Where did request slow down?"

Logging answers:  
"What happened inside this service?"

---

# 12. Best Practices

- Enable sampling carefully (not 100% in high-traffic production).
    
- Always propagate trace headers.
    
- Combine with centralized logging.
    
- Use with metrics for full observability.
    

---

# Interview Questions and Answers

1. What is distributed tracing?  
    It tracks a request across multiple microservices using a unique Trace ID.
    
2. What is the difference between trace and span?  
    Trace represents the entire request flow. Span represents a single service operation within that flow.
    
3. What replaced Spring Cloud Sleuth?  
    Micrometer Tracing.
    
4. How is trace ID propagated?  
    Through HTTP headers automatically by Spring Boot.
    
5. Why is distributed tracing important in microservices?  
    Because debugging cross-service issues is otherwise extremely difficult.
    
6. Does tracing work automatically with Feign?  
    Yes, Spring automatically propagates trace headers in Feign and WebClient.
    
