Centralized logging collects logs from multiple services into a single system for monitoring, debugging, and analysis.

In microservices:

- Logs are distributed across many pods
    
- Pods are ephemeral
    
- Logs are lost when pods restart
    

Solution: Use centralized logging.

Common stacks:

- ELK (Elasticsearch, Logstash, Kibana)
    
- EFK (Elasticsearch, Fluentd, Kibana)
    

---

## 2. ELK Stack Components

### 1. Elasticsearch

- Stores and indexes logs
    
- Provides fast search capability
    

### 2. Logstash

- Collects, parses, transforms logs
    
- Sends logs to Elasticsearch
    

### 3. Kibana

- Web UI to visualize and search logs
    
- Create dashboards
    

---

## 3. EFK Stack Components (Kubernetes Preferred)

Instead of Logstash, Kubernetes often uses Fluentd.

### Fluentd

- Lightweight log collector
    
- Runs as DaemonSet
    
- Collects logs from all nodes
    

Flow:

Application → Stdout → Fluentd → Elasticsearch → Kibana

---

## 4. Spring Boot Logging Best Practice

Use structured logging (JSON format).

Add dependency:

```xml
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
</dependency>
```

Logback configuration example:

```xml
<appender name="jsonAppender" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
</appender>

<root level="INFO">
    <appender-ref ref="jsonAppender"/>
</root>
```

Benefits:

- Machine-readable logs
    
- Easy filtering
    
- Better observability
    

---

## 5. Why Centralized Logging is Important

- Debug production issues quickly
    
- Search logs across all microservices
    
- Detect errors in real time
    
- Monitor security events
    
- Perform root cause analysis
    

---

## 6. Kubernetes Logging Architecture

1. Application logs to stdout
    
2. Container runtime writes logs
    
3. Fluentd/Fluent Bit collects logs
    
4. Sends to Elasticsearch
    
5. Kibana visualizes logs
    

No need to write log files inside container.

---

## 7. Production Best Practices

- Use JSON structured logging
    
- Do not log sensitive data
    
- Use correlation IDs (traceId)
    
- Integrate with distributed tracing
    
- Set proper log levels (INFO in prod)
    

---

# Interview Questions and Answers

1. What is centralized logging?  
    It is a system that collects logs from multiple services into one centralized storage for analysis.
    
2. What is the difference between ELK and EFK?  
    ELK uses Logstash for log processing, while EFK uses Fluentd, which is lighter and commonly used in Kubernetes.
    
3. Why should we log to stdout in containers?  
    Because Kubernetes collects container stdout logs automatically.
    
4. Why use structured logging?  
    Structured logging (JSON) makes logs searchable and easier to analyze in systems like Elasticsearch.
    
5. What is the role of Elasticsearch in logging?  
    It stores and indexes logs for fast search and querying.
    
6. What is a DaemonSet in logging architecture?  
    A DaemonSet ensures one logging agent (like Fluentd) runs on every node in the cluster.