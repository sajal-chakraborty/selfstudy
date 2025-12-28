# Spring Boot Interview Questions

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




3. A REST API is slow only in production, not locally. How do you debug it?
4. You changed `application.properties` but changes are not reflecting. Why?
5. Your Spring Boot service crashes under high traffic. What could be the reasons?
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
