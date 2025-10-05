# 🧠 System Design Concepts

A comprehensive guide to core system design principles, patterns, and trade-offs. Ideal for interview prep, architecture reviews, and technical documentation.

---

## ⚙️ Core Principles

### 1. Scalability
- **Vertical Scaling**: Add more power to existing machines.
- **Horizontal Scaling**: Add more machines to handle load.
- **Load Balancing**: Distribute traffic across servers (Round Robin, Least Connections, IP Hash).
- **Stateless Services**: Easier to scale and replicate.

### 2. Reliability & Fault Tolerance
- **Redundancy**: Duplicate components to avoid single points of failure.
- **Replication**: Master-Slave, Master-Master setups.
- **Failover**: Automatic switch to backup systems.
- **Circuit Breakers**: Prevent cascading failures.
- **Graceful Degradation**: Maintain partial functionality during failures.

### 3. Availability
- **High Availability (HA)**: Minimize downtime.
- **Active-Passive / Active-Active**: Redundancy models.
- **CAP Theorem**: Trade-offs between Consistency, Availability, and Partition Tolerance.

### 4. Performance & Latency
- **Caching**: CDN, Reverse Proxy, DB-level.
- **Cache Invalidation**: Write-through, Write-back, Write-around.
- **Throttling & Rate Limiting**: Protect resources from abuse.

### 5. Consistency
- **Strong vs Eventual Consistency**
- **Quorum Reads/Writes**
- **Distributed Transactions**: 2PC, 3PC

### 6. Data Partitioning & Sharding
- **Horizontal vs Vertical Partitioning**
- **Sharding Strategies**: Hash, Range, Directory
- **Resharding**: Rebalancing data across shards

---

## 🗃️ Data & Storage

### 7. Database Design
- **SQL vs NoSQL**
- **OLTP vs OLAP**
- **Indexing Strategies**
- **Normalization vs Denormalization**

### 8. Storage Systems
- **Block vs Object vs File Storage**
- **Blob Storage**: S3, GCS
- **RAID Levels**
- **Backup & Archival**

---

## 📡 Communication & Messaging

### 9. Messaging Patterns
- **Synchronous vs Asynchronous**
- **Message Queues**: Kafka, RabbitMQ, SQS
- **Pub/Sub Architecture**
- **Event-Driven Design**

### 10. Microservices Architecture
- **Service Discovery**
- **API Gateway**
- **Inter-service Communication**: REST, gRPC, GraphQL
- **Saga Pattern**
- **Strangler Fig Pattern**

---

## 🔐 Security & Observability

### 11. Security
- **Authentication**: OAuth2, JWT, SAML
- **Authorization**: RBAC, ABAC
- **Encryption**: TLS, At Rest, In Transit
- **Secrets Management**
- **DDoS Protection**

### 12. Monitoring & Observability
- **Metrics, Logs, Traces**
- **Health Checks**
- **Alerting**: Prometheus, Grafana, ELK
- **Distributed Tracing**: OpenTelemetry, Jaeger

---

## 🚀 Deployment & DevOps

### 13. CI/CD & Infrastructure
- **Build Pipelines**
- **Blue-Green & Canary Deployments**
- **Infrastructure as Code**: Terraform, CloudFormation
- **Containerization**: Docker
- **Orchestration**: Kubernetes

### 14. CDN & Edge Computing
- **Content Delivery Networks**
- **Edge Caching**
- **Geo-replication**

---

## 🧩 Advanced Topics

### 15. Rate Limiting & Quotas
- **Token Bucket, Leaky Bucket**
- **Sliding Window Counters**

### 16. Search Systems
- **Full-text Search**: Elasticsearch, Solr
- **Inverted Index**
- **Ranking Algorithms**

### 17. Recommendation Engines
- **Collaborative Filtering**
- **Content-Based Filtering**
- **Hybrid Models**

### 18. Analytics & Big Data
- **Batch vs Stream Processing**
- **Lambda & Kappa Architectures**
- **Data Lakes vs Warehouses**

### 19. Design Trade-offs
- **Latency vs Throughput**
- **Consistency vs Availability**
- **Simplicity vs Flexibility**

---

## 📚 References
- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Awesome Scalability](https://github.com/binhnguyennus/awesome-scalability)
- [Designing Data-Intensive Applications](https://dataintensive.net/)

---

> 💡 Tip: Use this checklist with spaced repetition or scenario-based MCQs to reinforce retention.
