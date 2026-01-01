# Spring Boot Interview Questions

# Spring Boot – Core Interview Questions & Answers (Deep Dive)

This document covers **commonly asked Spring Boot interview questions** with **clear explanations and real-world reasoning**.  
Use this as a **quick revision + conceptual clarity guide**.

---

## 1. Can `@Transactional` work on private methods? Why / why not?

**No, `@Transactional` does not work on private methods.**

### Why?
Spring applies `@Transactional` using **AOP proxies**. Proxies can intercept only:
- public (and sometimes protected/package-private) methods
- methods invoked through the Spring-managed proxy

Private methods:
- cannot be overridden
- are invoked internally within the same class
- bypass the proxy mechanism

### Key takeaway
> `@Transactional` works only when the method call goes **through the Spring proxy**, which private methods do not.

---

## 2. What happens if an exception is thrown inside a `@Transactional` method but caught?

**Rollback will NOT happen if the exception is caught and not rethrown.**

### Why?
Spring marks a transaction for rollback only when:
- an exception escapes the transactional method
- and it matches rollback rules (`RuntimeException` by default)

If you catch the exception:
- Spring assumes successful execution
- the transaction commits

### How to force rollback
- Rethrow the exception
- Use `rollbackFor = Exception.class`
- Call `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly()`

---

## 3. Difference between `@ComponentScan` and `spring.factories` auto-configuration

### `@ComponentScan`
- Scans packages for annotated components (`@Component`, `@Service`, etc.)
- Explicit and package-based
- Controlled by application developers

### `spring.factories` Auto-configuration
- Used internally by Spring Boot
- Loads configuration classes conditionally
- Based on classpath, environment, and missing beans
- Uses annotations like `@ConditionalOnClass`, `@ConditionalOnMissingBean`

### Core difference
> `@ComponentScan` is **developer-driven discovery**,  
> auto-configuration is **framework-driven conditional setup**.

---

## 4. Why does `@Autowired` sometimes fail even when a bean exists?

Common reasons:
- Multiple beans of the same type (ambiguity)
- Bean exists in a different ApplicationContext
- Conditional bean not created (condition not met)
- Profile mismatch
- Incorrect or missing `@Qualifier`
- Circular dependency (especially with constructor injection)
- Bean initialization order issues

### Key idea
> Bean existence alone is not enough — **uniqueness, visibility, and lifecycle** matter.

---

## 5. How does Spring Boot decide which `DataSource` to auto-configure?

Spring Boot follows a **priority-based decision model**:

1. User-defined `DataSource` bean (highest priority)
2. Explicit configuration in `application.yml/properties`
3. Embedded databases (H2, HSQL, Derby) if present
4. External DB drivers (MySQL, PostgreSQL, etc.)

If multiple drivers exist without clear configuration:
- Spring Boot may fail due to ambiguity
- Or choose an embedded DB if available

### Rule
> Auto-configuration happens only when Spring Boot can make a **safe, unambiguous choice**.

---

## 6. Can we change the default embedded server (Tomcat) at runtime? Why not?

**No, the embedded server cannot be changed at runtime.**

### Why?
- The server is initialized during application startup
- It manages:
  - ports
  - thread pools
  - servlet lifecycle
- Changing it would require restarting the ApplicationContext

You can:
- switch servers at build or startup time
- not dynamically at runtime

---

## 7. What is the real difference between `@Configuration` and `@Component`?

Both register beans, but their behavior differs.

### `@Configuration`
- Uses CGLIB proxying
- Ensures `@Bean` methods return singleton instances
- Enforces full configuration semantics

### `@Component`
- Registers a simple bean
- `@Bean` methods behave like normal methods
- No proxy enforcement

### Key difference
> `@Configuration` guarantees **consistent singleton behavior**,  
> `@Component` does not.

---

## 8. Why is constructor injection preferred over field injection?

Constructor injection is preferred because it:
- Ensures mandatory dependencies are provided
- Makes objects immutable
- Improves testability
- Avoids partially initialized beans
- Detects circular dependencies early

Field injection:
- Uses reflection
- Hides dependencies
- Makes testing harder
- Can cause runtime surprises

### Spring’s internal preference
> Constructor injection is safer, clearer, and more maintainable.

---

## Final Interview Summary

These questions test:
- Spring internals
- Proxy behavior
- Lifecycle management
- Design principles

Understanding the **why** behind each answer is what separates  
**framework users** from **framework engineers**.

---


## (1) Your Spring Boot app works locally but fails after deployment. What are the first 3 things you check?

---

## ✅ High-Level Answer

1. **Application startup logs** to identify configuration, bean, or dependency failures.
2. **Active Spring profile and environment-specific configuration** to ensure required properties are correctly set.
3. **Infrastructure and external dependencies** such as load balancer health checks, networking rules, database connectivity, and access to config or secret management services.

---

## 🔍 Detailed Breakdown

### 1️⃣ Application Startup Logs (First Priority)

- Check logs immediately after application startup to identify:
  - Bean creation or dependency injection failures
  - Missing or invalid configuration values
  - Port binding or server startup issues
  - Database or external service connection errors
- In cloud environments, verify:
  - Container logs (Docker / Kubernetes pods)
  - VM logs (EC2, ECS, EKS)
- Logs usually reveal **what failed** and **at which phase**:
  - Configuration loading
  - Application context initialization
  - Embedded server startup

---

### 2️⃣ Environment-Specific Configuration & Profiles

- Verify the active Spring profile using:
  - `spring.profiles.active`
- Ensure the correct configuration files exist and are loaded:
  - `application-{profile}.yml`
  - `application-{profile}.properties`
- Validate that all required properties are present:
  - Database URLs and credentials
  - External service endpoints
  - Feature flags or environment variables

**Common failure pattern:**
> Application works locally with the `default` profile but fails in `prod` due to missing or incorrect configuration.

---

### 3️⃣ Infrastructure & External Dependencies

Depending on the deployment environment (AWS / Kubernetes / VM):

#### Load Balancer & Networking
- Health check endpoint is correct (e.g. `/actuator/health`)
- Security groups / firewall rules allow inbound traffic on the application port

#### Database
- Database endpoint is reachable from the deployed environment
- Credentials, SSL settings, and network access are correctly configured

#### External Config & Secrets
- Config Server or Secrets Manager is reachable during startup
- IAM roles / permissions are properly attached
- No missing secrets causing application startup failure

---





## A REST API is slow only in production, not locally. How do you debug it?

---

## 🧠 One-Liner Answer
When an API is slow only in production, I compare data volume and dependencies first, then analyze connection pools, network latency, and use observability tools to identify bottlenecks under real load.

---

## ✅ High-Level Approach
1. Compare **database behavior and data volume**
2. Analyze **downstream service latency**
3. Check **connection pooling and thread pools**
4. Evaluate **network latency and deployment topology**
5. Confirm findings using **observability and load testing**

---

## 🔍 Detailed Explanation

### 1️⃣ Database & Data Volume Differences
- Local environments usually run with small test datasets
- Production has:
  - Large tables
  - Real indexes
  - Lock contention
- Check:
  - Slow queries
  - Missing indexes
  - Execution plans
- Many APIs are fast locally but slow in production due to **data scale**.

---

### 2️⃣ Downstream Service Dependencies
- Identify calls to:
  - Other microservices
  - External third-party APIs
- Measure:
  - Latency contribution per dependency
  - Retry and timeout behavior
- Downstream slowness often cascades and impacts upstream APIs.

---

### 3️⃣ Connection Pooling & Thread Pool Saturation
- Validate:
  - Database connection pool size and wait time
  - HTTP client connection pools (RestTemplate / WebClient)
  - Server thread pools (Tomcat / Netty)
- Pool exhaustion causes:
  - Increased response time
  - Thread blocking
- This issue rarely appears locally but is common in production.

---

### 4️⃣ Network Latency & Deployment Topology
- Check for:
  - Cross-AZ or cross-region calls
  - DNS resolution delays
  - TLS handshake overhead
- Network hops are minimal locally but significant in production.

---

### 5️⃣ Observability, Profiling & Load Testing
- Use APM tools (Dynatrace / New Relic / Datadog) to:
  - Break down latency by layer
  - Identify hotspots
- Capture thread dumps and analyze for:
  - Blocked threads
  - Deadlocks
- Run load tests with production-like TPS to confirm behavior.

---

## You changed `application.properties` but changes are not reflecting. Why?

## One-Line Explanation

Spring Boot configuration changes may not reflect due to restart or refresh issues, active profile mismatch, external configuration overrides, caching, or incorrect property resolution.

---

## Common Root Causes & Debugging Checklist

### 1. Application Restart or Refresh Not Triggered
- `application.properties` is loaded only at application startup by default.
- A restart is required for changes to take effect.
- If Spring Boot Actuator is enabled, configuration can be refreshed using `/actuator/refresh`.
- If Spring Boot Admin is configured, configuration refresh can be triggered centrally.

---

### 2. Active Profile Mismatch
- If a profile such as `dev`, `qa`, or `prod` is active, Spring loads `application-{profile}.properties`.
- Changes in `application.properties` will be ignored if overridden by a profile-specific file.

---

### 3. Spring Cloud Config Server Overrides Local Configuration
- When Spring Cloud Config Server is enabled, it becomes the primary source of configuration.
- Local configuration files are ignored.
- Changes must be updated in the config repository and refreshed using Actuator.

---

### 4. Typo or Incorrect Property Key
- Spring silently ignores unknown or misspelled property keys.
- Common causes include wrong property prefixes and incorrect YAML indentation.

---

### 5. Environment Variables or JVM Arguments Override File Configuration
- Spring Boot follows a strict property precedence order.
- JVM arguments and environment variables override values in `application.properties`.
- If a property is passed using `-Dproperty=value` or as an environment variable, file changes will not apply.

---

### 6. Configuration Cached at Startup (Non-Refreshable Beans)
- `@Value` and `@ConfigurationProperties` are bound during application startup.
- Without `@RefreshScope`, refreshed configuration values will not update beans at runtime.

---

### 7. Multiple Configuration Locations
- Spring may load configuration from multiple locations such as `classpath:/config/` or external paths.
- The application might be reading a different configuration file than the one being edited.

---

## Recommended Production Debugging Order

1. Confirm application restart or refresh was performed  
2. Verify active Spring profile  
3. Check Spring Cloud Config or other external configuration sources  
4. Validate environment variable and JVM overrides  
5. Verify property key correctness  
6. Check refresh scope usage  
7. Confirm correct configuration file location  

---

## Interview-Ready Summary

When configuration changes do not reflect, first verify restart or refresh, then check active profiles and external configuration sources like Spring Cloud Config. Also validate environment variable overrides, refresh scope usage, property keys, and configuration file locations.

---

## License

MIT




## Your Spring Boot service crashes under high traffic. What could be the reasons?

## One-Line Explanation

A Spring Boot service can crash under high traffic due to insufficient resources, memory leaks, thread or connection pool exhaustion, unbounded caching, GC pressure, lack of auto-scaling, or failures in downstream dependencies.

---

## Root Causes & Debugging Checklist

### 1. Insufficient CPU and Memory Resources
- The JVM may run out of heap memory or face CPU starvation under load.
- Incorrect container or VM resource limits can cause frequent crashes or restarts.

---

### 2. Memory Leaks in the Application
- Objects retained unintentionally due to static references, listeners, or unbounded collections.
- Leads to gradual heap exhaustion and `OutOfMemoryError`.

---

### 3. Improper Caching Strategy
- Unbounded caches or missing eviction policies increase memory usage.
- Cache entries must have size limits and TTLs.

---

### 4. Inadequate Load Testing and Capacity Planning
- Absence of production-like load testing hides scalability issues.
- Incorrect capacity planning results in undersized infrastructure.

---

### 5. Auto-Scaling Not Enabled or Misconfigured
- Without proper horizontal or vertical scaling, traffic spikes overwhelm the service.
- Delayed or incorrect scaling thresholds worsen outages.

---

### 6. Downstream Dependency Failures
- Slow or failing downstream APIs block request threads.
- Missing timeouts, circuit breakers, or bulkheads cause cascading failures.

---

### 7. Thread Pool Exhaustion
- Limited servlet, executor, or async thread pools get exhausted under load.
- Blocked threads lead to request pile-up and service crash.

---

### 8. Database or Connection Pool Exhaustion
- Insufficient database or HTTP connection pool size causes threads to wait indefinitely.
- Results in request timeouts and service instability.

---

### 9. Excessive Garbage Collection Pressure
- High object creation rate leads to frequent Full GCs.
- Causes long pause times and eventual JVM or container termination.

---

### 10. Blocking I/O in Request Threads
- Blocking REST, database, or file I/O reduces throughput.
- Under heavy load, this leads to thread starvation.

---

## Recommended Production Debugging Order

1. Verify CPU and memory limits  
2. Check for memory leaks and GC behavior  
3. Inspect thread pools and connection pools  
4. Validate caching configuration and eviction policies  
5. Review downstream dependencies and resilience patterns  
6. Confirm load testing and capacity planning  
7. Validate auto-scaling configuration  

---

## Interview-Ready Summary

When a Spring Boot service crashes under high traffic, the root causes typically include resource exhaustion, memory leaks, unbounded caches, thread or connection pool exhaustion, GC pressure, missing auto-scaling, and unprotected downstream dependencies.

---


6. Multiple beans of the same type exist and Spring throws an error. How do you fix it?
7. You need different configs for dev, test, and prod. How will you design this?
8. Your API works fine but returns 401 randomly. What could cause this?
9. A database connection pool is getting exhausted. How do you identify and solve it?
10. How do you handle API failures when calling another microservice?
11. Your application memory usage keeps increasing over time. What do you suspect?
12. How do you implement retry logic for failed REST calls?
13. You deployed a new version but users still hit the old behavior. Why?
14. How do you secure sensitive values like DB passwords in Spring Boot?
15. What happens if `@Transactional` fails midway? How do you verify rollback?
16. Your logs are missing in production. Where do you look?
17. How do you handle versioning of REST APIs without breaking clients?
18. One microservice is down and others are failing. How do you prevent this?
19. How do you reduce startup time of a Spring Boot application?
20. You need to execute code only once after application startup. How?
21. How do you handle long-running background jobs in Spring Boot?
22. Your API returns correct data but response time is inconsistent. Why?
23. How do you trace a request across multiple microservices?
24. What strategy do you use for graceful shutdown of Spring Boot apps?
25. How do you manage schema changes without breaking production?
26. Your Spring Boot app fails only when deployed in Docker. Why?
27. How do you implement rate limiting for APIs?
28. What happens if an exception occurs inside a filter or interceptor?
29. How do you test production-like behavior locally?
30. Your service needs zero-downtime deployment. How do you achieve it?
31. What are common mistakes freshers make in Spring Boot projects?

---

## 🧠 Spring Boot Internals Questions (Asked in Interviews)

1. How does Spring Boot decide which auto-configuration to apply?
2. What happens internally when you add `spring-boot-starter-web`?
3. Why does Spring Boot prefer convention over configuration?
4. How does Spring Boot load `application.properties` internally?
5. Exact startup flow of a Spring Boot application.
6. Difference between `@ComponentScan` and `@SpringBootApplication`.
7. How does Spring Boot detect embedded Tomcat and configure it?
8. What happens if two beans of the same type exist without `@Qualifier`?
9. How does Spring Boot handle profile-specific configuration?
10. What is the role of `SpringFactoriesLoader` under the hood?
11. Difference between `@RestController` and `@Controller` internally.
12. How does Spring Boot manage dependency versions automatically?
13. Lifecycle of a Spring Bean in Spring Boot.
14. How does Spring Boot handle externalized configuration?
15. Fat JAR vs normal JAR — internal difference.
16. How does Spring Boot decide server port priority?
17. What happens internally when you hit a REST endpoint?
18. How Spring Boot integrates with Actuator internally.
19. How exception translation works in Spring Boot.
20. Common performance mistakes in Spring Boot applications.

---

## ⭐ Contribute

PRs are welcome for:
- Better answers
- Real production incidents
- Performance tuning tips
