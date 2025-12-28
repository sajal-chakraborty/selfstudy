# Spring Boot Interview Questions (Scenario + Internals)

A curated collection of **real-world Spring Boot interview questions**, focusing on  
👉 production issues  
👉 debugging mindset  
👉 performance & scalability  
👉 Spring Boot internals  

> Interviewers don’t test annotations.  
> They test **how you think when things break in production**.

---

## 🔥 Scenario-Based Questions (Real Interview Style)

1. Your Spring Boot app works locally but fails after deployment. What are the first 3 things you check?
2. A REST API is slow only in production, not locally. How do you debug it?
3. You changed `application.properties` but changes are not reflecting. Why?
4. Your Spring Boot service crashes under high traffic. What could be the reasons?
5. Multiple beans of the same type exist and Spring throws an error. How do you fix it?
6. You need different configs for dev, test, and prod. How will you design this?
7. Your API works fine but returns 401 randomly. What could cause this?
8. A database connection pool is getting exhausted. How do you identify and solve it?
9. How do you handle API failures when calling another microservice?
10. Your application memory usage keeps increasing over time. What do you suspect?
11. How do you implement retry logic for failed REST calls?
12. You deployed a new version but users still hit the old behavior. Why?
13. How do you secure sensitive values like DB passwords in Spring Boot?
14. What happens if `@Transactional` fails midway? How do you verify rollback?
15. Your logs are missing in production. Where do you look?
16. How do you handle versioning of REST APIs without breaking clients?
17. One microservice is down and others are failing. How do you prevent this?
18. How do you reduce startup time of a Spring Boot application?
19. You need to execute code only once after application startup. How?
20. How do you handle long-running background jobs in Spring Boot?
21. Your API returns correct data but response time is inconsistent. Why?
22. How do you trace a request across multiple microservices?
23. What strategy do you use for graceful shutdown of Spring Boot apps?
24. How do you manage schema changes without breaking production?
25. Your Spring Boot app fails only when deployed in Docker. Why?
26. How do you implement rate limiting for APIs?
27. What happens if an exception occurs inside a filter or interceptor?
28. How do you test production-like behavior locally?
29. Your service needs zero-downtime deployment. How do you achieve it?
30. What are common mistakes freshers make in Spring Boot projects?

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
