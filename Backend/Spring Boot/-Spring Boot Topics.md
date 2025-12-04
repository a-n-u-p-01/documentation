## **Spring Boot Fundamentals**

- [[Note/Introduction to Spring Boot]] ✅ High
    
- Spring vs Spring Boot ✅ High
    
- Spring Boot Starter Dependencies ✅ High
    
- Spring Boot Auto Configuration ✅ High
    
- [[Note/Spring Boot Core Annotations]] ✅ High
    
- Application Properties and YAML Configuration ✅ High
    
- Spring Profiles and Environment Configuration ✅ High
    
- Configuration Properties (Type-Safe Config) ✅ High
    
- Externalized Configuration ⚙ Medium
    
- Spring Boot DevTools ⚙ Medium
    
- Spring Boot Actuator ⚙ Medium
    
- Spring Boot CLI 🔴 Low
    
- Spring Boot Application Lifecycle ⚙ Medium
    
- ApplicationRunner vs CommandLineRunner ⚙ Medium
    
- **Fat JAR vs WAR deployment basics** ⚙ Medium
    
- **Layered JAR for Docker optimization** ⚙ Medium
    

---

## **Dependency Injection & Core Concepts**

- Dependency Injection (Constructor/Setter) ✅ High
    
- Spring Stereotype Annotations (@Component, @Service, @Repository) ✅ High
    
- Autowired, Qualifier, Primary ✅ High
    
- Java-Based Bean Configuration (@Bean) ✅ High
    
- Spring Bean Scopes ⚙ Medium
    
- Spring Bean Lifecycle ⚙ Medium
    
- Lazy Initialization ⚙ Medium
    
- ApplicationContext vs BeanFactory ⚙ Medium
    

---

## **Aspect-Oriented Programming (AOP)**

- AOP Fundamentals ✅ High
    
- Pointcuts and JoinPoints ✅ High
    
- Spring AOP Advices (Before, After, Around) ✅ High
    
- Defining Aspects (@Aspect) ✅ High
    
- Common AOP Use Cases ⚙ Medium
    
- Spring Proxy Mechanism ⚙ Medium
    

---

## **Spring Boot Web (REST & MVC)**

- Spring Web Starter Overview ✅ High
    
- RESTful Web Services (CRUD, layered design) ✅ High
    
- Spring Controllers (@RestController) ✅ High
    
- Request Mappings (@GetMapping, @PostMapping...) ✅ High
    
- Request & Response Handling (ResponseEntity) ✅ High
    
- Path Variables and Request Parameters ✅ High
    
- Handling JSON Request Body (@RequestBody) ✅ High
    
- HTTP Message Converters (Jackson, XML) ⚙ Medium
    
- Exception Handling (@ControllerAdvice) ✅ High
    
- Form Validation (@Valid, constraints) ⚙ Medium
    
- CORS Configuration ⚙ Medium
    
- File Upload/Download ⚙ Medium
    
- Serving Static & Template Content ⚙ Medium
    
- Spring MVC Interceptors and Filters ⚙ Medium
    
- Content Negotiation (JSON/XML/HTML) ⚙ Medium
    
- **API Versioning (URI, headers)** ✅ High
    
- **Rate limiting for controllers (Bucket4j basics)** ⚙ Medium
    
- **Request size/time limit handling** ⚙ Medium
    
- Internationalization (i18n) 🔴 Low
    

---

## **Data Access & Persistence (Spring Data JPA)**

- Spring Data JPA Overview ✅ High
    
- Hibernate + JPA with Spring Boot ✅ High
    
- Entity Mapping (@Entity, @Table) ✅ High
    
- Entity Relationships (OneToMany, ManyToMany..) ✅ High
    
- Entity Lifecycle Callbacks (@PrePersist…) ⚙ Medium
    
- JPA Repositories (CrudRepository, JpaRepository) ✅ High
    
- Derived Query Methods (findBy...) ✅ High
    
- Custom Queries (JPQL, SQL) ✅ High
    
- Pagination & Sorting ⚙ Medium
    
- Transaction Management (@Transactional) ✅ High
    
- DTO Projection ⚙ Medium
    
- JPA Exception Handling ⚙ Medium
    
- Database Migration (Flyway/Liquibase) ✅ High
    
- Soft Delete (@SQLDelete) ⚙ Medium
    
- Enum Mapping (STRING vs ORDINAL) ⚙ Medium
    
- **Connection Pooling (HikariCP basics)** ✅ High
    
- **Indexing & query optimization basics** ⚙ Medium
    

---

## **Spring Security**

- Security Basics (Filters, Authentication, Authorization) ✅ High
    
- Authentication vs Authorization Concepts ✅ High
    
- Security Filter Chain ⚙ Medium
    
- In-Memory Authentication ⚙ Medium
    
- Database Authentication (JDBC/JPA) ✅ High
    
- UserDetailsService Implementation ✅ High
    
- Custom AuthenticationProvider ⚙ Medium
    
- Custom Security Filters ⚙ Medium
    
- JWT Authentication (Access/Refresh tokens) ✅ High
    
- OAuth2 Login & Social Login ⚙ Medium
    
- Password Encoding (BCryptPasswordEncoder) ✅ High
    
- CSRF and CORS Configuration ⚙ Medium
    
- Method Level Security (@PreAuthorize) ⚙ Medium
    
- Security Testing (@WithMockUser) ⚙ Medium
    
- **Session management (stateless vs stateful)** ⚙ Medium
    
- **Security hardening (HTTPS, HSTS, CSP, headers)** ✅ High
    
- **Audit logging & security events** ⚙ Medium
    
- **OAuth2 Resource Server (JWT validation, JWKS)** ⚙ Medium
    

---

## **Spring Boot Testing**

- JUnit 5 Testing Basics (assertions, lifecycle) ✅ High
    
- Mockito for Unit Testing (mocks, stubs) ✅ High
    
- Spring Boot Test Starter (@SpringBootTest) ✅ High
    
- MockMvc Testing (Controllers) ✅ High
    
- Testing JPA Repositories (@DataJpaTest) ✅ High
    
- JSON Assertions (jsonPath) ✅ High
    
- Integration Testing (RestTemplate/TestRestTemplate) ✅ High
    
- Slicing Tests (@WebMvcTest, @DataJpaTest) ⚙ Medium
    
- Testcontainers (DB test automation) ⚙ Medium
    
- WireMock for API Mocking ⚙ Medium
    
- **Spring Security testing** ⚙ Medium
    

---

## **Spring Boot with Cloud & DevOps**

- Dockerizing Spring Boot Applications (Dockerfile, layered JAR) ✅ High
    
- Jib Plugin (build images without Dockerfile) ⚙ Medium
    
- CI/CD Pipelines (GitHub Actions, Jenkins) ✅ High
    
- Actuator for Monitoring (Health, Metrics) ✅ High
    
- Spring Boot Admin ⚙ Medium
    
- Kubernetes Basics (Deployment, Service, ConfigMap, Secrets) ⚙ Medium
    
- **Readiness/Liveness Probes** ✅ High
    
- **Graceful Shutdown** ⚙ Medium
    
- Centralized Logging (ELK/EFK) ⚙ Medium
    
- Monitoring with Prometheus & Grafana ⚙ Medium
    

---

## **Spring Cloud (Microservices)**

- Spring Cloud Basics (Config, Discovery) ✅ High
    
- Spring Cloud Config Server (central config) ✅ High
    
- Service Discovery (Eureka/Consul) ⚙ Medium
    
- Spring Cloud Gateway (Routing/Filters) ✅ High
    
- Feign Client (declarative calls) ⚙ Medium
    
- Circuit Breaker (Resilience4j) ⚙ Medium
    
- Spring Cloud LoadBalancer ⚙ Medium
    
- API Rate Limiting (Gateway) ⚙ Medium
    
- Distributed Tracing (Micrometer Tracing) ⚙ Medium
    
- **Correlation IDs for distributed logs (MDC)** ✅ High
    

---

## **Reactive Stack (WebFlux)**

(_Only essential, no unnecessary deep-dive topics_)

- Reactive Programming Basics ⚙ Medium
    
- Mono & Flux Fundamentals ⚙ Medium
    
- Spring WebFlux (Reactive REST) ⚙ Medium
    
- WebClient (Reactive HTTP client) ⚙ Medium
    
- Backpressure concepts ⚙ Medium
    

---

## **Spring Boot Extras**

- Lombok Integration (DTOs, entities) ✅ High
    
- MapStruct for DTO Mapping ⚙ Medium
    
- Swagger/OpenAPI (Springdoc) documentation ✅ High
    
- Logging (SLF4J + Logback) ✅ High
    
- Caching in Spring Boot (@Cacheable) ⚙ Medium
    
- Task Scheduling (@Scheduled) ⚙ Medium
    
- Asynchronous Processing (@Async) ⚙ Medium
    
- Application Events (event-driven architecture basics) ⚙ Medium
    
- Retry Mechanism (@Retryable with Spring Retry) ⚙ Medium
    

---

## **Advanced Spring Boot**

- Performance Tuning ⚙ Medium
    
- ThreadPool Configuration (Async, Scheduling, Web) ⚙ Medium
    
- Graceful Shutdown ⚙ Medium
    
- Micrometer & Observability ⚙ Medium
    
- Metrics & Health Checks ⚙ Medium
    

---