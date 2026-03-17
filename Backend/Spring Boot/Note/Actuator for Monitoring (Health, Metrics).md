Spring Boot Actuator provides production-ready monitoring and management features for Spring Boot applications.

Main purposes:

- Health monitoring
    
- Metrics collection
    
- Application diagnostics
    
- Observability integration (Prometheus, Grafana, etc.)
    

Dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

---

## 2. Base URL

Default base path:

```
http://localhost:8080/actuator
```

---

## 3. Important Endpoints

| Endpoint             | Description                      |
| -------------------- | -------------------------------- |
| /actuator/health     | Shows application health status  |
| /actuator/metrics    | Displays application metrics     |
| /actuator/info       | Displays custom application info |
| /actuator/env        | Shows environment properties     |
| /actuator/loggers    | View/change logging levels       |
| /actuator/threaddump | Thread dump information          |
|                      |                                  |

---

## 4. Exposing Endpoints

By default, only health and info are exposed.

Enable all endpoints:

```properties
management.endpoints.web.exposure.include=*
```

Or specific ones:

```properties
management.endpoints.web.exposure.include=health,metrics,info
```

---

## 5. Health Endpoint

URL:

```
/actuator/health
```

Default Response:

```json
{
  "status": "UP"
}
```

If database or other dependencies fail:

```json
{
  "status": "DOWN"
}
```

Enable detailed health info:

```properties
management.endpoint.health.show-details=always
```

Health indicators include:

- Database
    
- Disk space
    
- MongoDB
    
- Redis
    
- Custom indicators
    

Custom Health Indicator:

```java
@Component
public class CustomHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        if (serviceIsHealthy()) {
            return Health.up().build();
        }
        return Health.down().withDetail("error", "Service not available").build();
    }
}
```

---

## 6. Metrics Endpoint

URL:

```
/actuator/metrics
```

Shows available metrics like:

- jvm.memory.used
    
- jvm.gc.pause
    
- system.cpu.usage
    
- http.server.requests
    
- process.uptime
    

Get specific metric:

```
/actuator/metrics/jvm.memory.used
```

Metrics are powered by Micrometer.

---

## 7. Prometheus Integration

Add dependency:

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

Expose:

```properties
management.endpoints.web.exposure.include=prometheus
```

Endpoint:

```
/actuator/prometheus
```

Used by Prometheus to scrape metrics.

---

## 8. Security Best Practice

Never expose all endpoints publicly in production.

Secure with Spring Security:

```properties
management.endpoints.web.exposure.include=health,metrics
management.endpoint.health.show-details=when_authorized
```

---

## 9. Production Usage

Used for:

- Kubernetes liveness and readiness probes
    
- Monitoring with Prometheus and Grafana
    
- Alerting systems
    
- Debugging production issues
    
- Observability in microservices architecture
    

---

## 10. Interview Points

- Actuator provides monitoring endpoints.
    
- Health endpoint checks application dependencies.
    
- Metrics are powered by Micrometer.
    
- Prometheus integration uses /actuator/prometheus.
    
- Used heavily in cloud-native and microservices environments.