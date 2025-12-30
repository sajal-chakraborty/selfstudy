# Two-Phase Commit (2PC) – Complete Notes (Interview + Practical)

---

## What is Two-Phase Commit (2PC)?

Two-Phase Commit (2PC) is a **distributed transaction protocol** that guarantees **atomicity** across multiple databases or services.

**Atomicity means**:  
Either **all participants commit** the transaction or **all roll back**.

A central **Coordinator** manages the transaction, and all databases/services act as **Participants**.

---

## Phases of Two-Phase Commit

### Phase 1: Prepare (Voting Phase)
- Coordinator asks all participants: *“Can you commit?”*
- Each participant:
  - Writes data to a transaction log
  - Locks required resources
  - Responds with **YES** or **NO**

### Phase 2: Commit / Rollback Phase
- If **all participants vote YES** → Coordinator sends **COMMIT**
- If **any participant votes NO** → Coordinator sends **ROLLBACK**
- Participants then release locks

---

## Why Two-Phase Commit Is Not Used in Distributed Applications

### 1. Blocking Nature
- If the coordinator crashes after the prepare phase:
  - Participants remain locked
  - They cannot commit or rollback
  - System can block indefinitely

### 2. Poor Scalability
- Requires synchronous communication
- Central coordinator becomes a bottleneck
- Latency increases as participants increase

### 3. Tight Coupling
- All services must be XA-compliant
- All participants must be available simultaneously
- Breaks microservices independence

### 4. Performance Issues
- Database locks are held for long durations
- Thread pools and connection pools get exhausted
- Throughput drops under load

### 5. Not Cloud-Native Friendly
- Cloud systems expect failures and restarts
- 2PC assumes stable network and long-lived processes
- Unsuitable for Kubernetes and microservices

---

## One-Line Interview Answer (Why 2PC Is Avoided)

> Two-Phase Commit ensures atomicity but is avoided in distributed systems because it is blocking, tightly coupled, hard to scale, and unsuitable for failure-prone cloud environments.

---

## How Two-Phase Commit Is Achieved in Spring Boot (Code Level)

Spring Boot does **not implement 2PC directly**.  
It relies on **JTA (Java Transaction API)** and **XA-capable resources**.

---

## Core Components Required

1. **XA-capable DataSources**
   - MySQL XA
   - Oracle XA
   - PostgreSQL XA

2. **JTA Transaction Manager**
   - Atomikos (most commonly used)
   - Narayana
   - Bitronix

3. **Spring `@Transactional` annotation**

---

## Maven Dependency (Atomikos Example)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jta-atomikos</artifactId>
</dependency>
```

## XA DataSource Configuration Example

```java
@Bean
public DataSource db1DataSource() {
    MysqlXADataSource xaDs = new MysqlXADataSource();
    xaDs.setUrl("jdbc:mysql://localhost:3306/db1");
    xaDs.setUser("user");
    xaDs.setPassword("pass");

    AtomikosDataSourceBean ds = new AtomikosDataSourceBean();
    ds.setXaDataSource(xaDs);
    ds.setUniqueResourceName("db1");
    return ds;
}
```

Same for db2DataSource with a different uniqueResourceName.

## Step 3: Use @Transactional (THIS triggers 2PC)
```java
@Service
public class OrderService {

    @Autowired
    private JdbcTemplate db1JdbcTemplate;

    @Autowired
    private JdbcTemplate db2JdbcTemplate;

    @Transactional
    public void placeOrder() {

        db1JdbcTemplate.update(
            "INSERT INTO orders VALUES (?, ?)", 1, "CREATED"
        );

        db2JdbcTemplate.update(
            "INSERT INTO payments VALUES (?, ?)", 1, "INITIATED"
        );

        // If any exception occurs → both DBs rollback
    }
}

```
## What Happens Internally

- Spring starts a **JTA transaction**
- **Atomikos** becomes the **Coordinator**
- Both databases act as **Participants**
- Atomikos executes:
  - **Phase 1:** Prepare
  - **Phase 2:** Commit / Rollback

👉 You do **not** see Two-Phase Commit in application code — it happens **implicitly**.

---

## How Failure Looks (Important)

If **DB2 fails after DB1 insert**:

- DB1 is **rolled back**
- DB2 is **rolled back**
- **Atomicity is preserved**

✔ This is a **true distributed transaction**.

---

## Why This Is Dangerous in Real Systems (Code-Level Reality)

Even though the code looks simple:

```java
@Transactional
public void doSomething() {
    ...
}

```
## Under the Hood (Why 2PC Is Risky)

- Database locks are held for the **entire transaction duration**
- Application threads remain **blocked**
- Network latency increases overall transaction time
- Coordinator crash ⇒ **participants remain blocked indefinitely**

That’s why **modern microservices avoid Two-Phase Commit**.

---

## Where Two-Phase Commit Is Still Acceptable

- ✅ Same organization
- ✅ Low traffic
- ✅ Few participants
- ✅ Strong consistency requirement

**Typical use cases:**
- Legacy monolith with multiple databases
- Banking core systems
- Internal batch processing

---

## What Spring Teams Prefer Instead (Production Reality)

| Problem | Preferred Pattern |
|------|------------------|
| DB + Message consistency | Outbox Pattern |
| Multi-service consistency | Saga Pattern |
| Reliability | Idempotency + retries |
| Scalability | Eventual consistency |

---

## One-Line Interview Answer

> In Spring Boot, Two-Phase Commit is achieved using JTA with XA-capable resources and a transaction manager like Atomikos, where `@Transactional` implicitly triggers prepare and commit phases across participants.




