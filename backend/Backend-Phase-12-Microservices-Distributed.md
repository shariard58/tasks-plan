# Backend Phase 12 — Microservices & Distributed Systems (Tasks 381-410)

> **Monolith → Microservices transition = most important architecture decision।**
> **কখন microservices করবে, কিভাবে করবে, কী pitfalls আছে — সব জানবে।**
> **Distributed systems theory জানলে তুমি Netflix/Uber level architect।**

---

## Section A: Microservices (Tasks 381-395)

### Task 381 — Monolith vs Microservices 🟡

- When monolith is better (most startups!), when to migrate
- Pros: independent deployment, scaling, tech diversity
- Cons: complexity, network latency, data consistency, debugging
- **Exercise:** Decision document: when to use monolith vs microservices

### Task 382 — Microservice Design Principles 🟡

- Single Responsibility, Bounded Context, Loose Coupling, High Cohesion
- API-first design, Independent database per service
- **Exercise:** Decompose monolithic e-commerce into microservices

### Task 383 — Inter-Service Communication 🟡

- Synchronous: REST, gRPC, GraphQL Federation
- Asynchronous: Message queues, Event bus
- **Exercise:** Same flow using REST vs gRPC vs events — compare

### Task 384 — API Gateway for Microservices 🟡

- Request routing, Authentication, Rate limiting, Response aggregation
- **Exercise:** API gateway that routes to 3 services

### Task 385 — Service Mesh (Istio/Linkerd concepts) 🟡

- Sidecar proxy, mTLS, traffic management, observability
- **Exercise:** Understand service mesh concepts, implement sidecar pattern

### Task 386 — Data Management in Microservices 🔴

- Database per service, Saga for distributed transactions
- Data duplication, eventual consistency, CQRS
- **Exercise:** Data strategy for 5-service architecture

### Task 387 — gRPC for Service Communication 🟡

- Protocol Buffers, streaming, deadlines, interceptors
- **Exercise:** gRPC service-to-service communication

### Task 388 — GraphQL Federation 🔴

- Multiple GraphQL services → unified graph
- **Exercise:** Federated GraphQL (user service + product service + order service)

### Task 389 — Service Discovery & Load Balancing 🟡

- Client-side (Ribbon), Server-side (Nginx), DNS-based, Consul, K8s
- **Exercise:** Service registry with health checks

### Task 390 — Circuit Breaker & Resilience 🟡

- Circuit breaker, Retry with backoff, Timeout, Fallback, Bulkhead
- **Exercise:** Resilience library (all patterns)

### Task 391 — Distributed Logging 🟡

- Centralized logging (ELK Stack), Correlation IDs, Structured logs
- **Exercise:** Centralized logging for 3 services

### Task 392 — Distributed Tracing 🟡

- OpenTelemetry, Jaeger, traces ও spans
- **Exercise:** Distributed tracing across services

### Task 393 — Configuration Management 🟡

- Centralized config (Consul, etcd), Feature flags, Environment-based
- **Exercise:** Centralized configuration for all services

### Task 394 — Strangler Fig Pattern 🟡

- Gradually migrate from monolith to microservices
- Route by route migration, parallel running
- **Exercise:** Plan monolith → microservices migration strategy

### Task 395 — Microservices Testing 🟡

- Unit, Integration, Contract (Pact), E2E, Chaos testing
- **Exercise:** Test pyramid for microservices

---

## Section B: Distributed Systems Theory (Tasks 396-410)

### Task 396 — Distributed Systems Challenges 🟡

- Network partitions, Clock skew, Partial failures, No global state
- **Exercise:** Document all distributed systems challenges

### Task 397 — Consistency Models 🔴

- Strong, Sequential, Causal, Eventual consistency
- Linearizability vs Serializability
- **Exercise:** Implement ও demonstrate each consistency model

### Task 398 — Distributed Consensus (Raft) 🔴

- Leader election, Log replication, Safety
- **Exercise:** Simplified Raft implementation

### Task 399 — Vector Clocks & Lamport Timestamps 🔴

- Ordering events in distributed systems
- **Exercise:** Vector clock implementation for conflict detection

### Task 400 — Gossip Protocol 🟡

- Decentralized information propagation
- **Exercise:** Simple gossip protocol for cluster membership

### Task 401 — Distributed Locking 🔴

- Redlock, ZooKeeper locks, database-based locks
- Fencing tokens for correctness
- **Exercise:** Distributed lock with fencing

### Task 402 — Distributed Caching 🟡

- Consistent hashing, Cache invalidation across nodes
- **Exercise:** Distributed cache with consistent hashing

### Task 403 — Distributed Transactions 🔴

- Two-Phase Commit (2PC), Three-Phase Commit (3PC), Saga
- **Exercise:** Compare approaches, implement 2PC

### Task 404 — Distributed ID Generation 🟡

- Snowflake IDs, UUIDs, ULID, database sequences
- **Exercise:** Distributed ID generator (like Twitter Snowflake)

### Task 405 — Leader Election 🟡

- Bully algorithm, Ring algorithm, Raft-based
- **Exercise:** Leader election for worker processes

### Task 406 — Distributed Rate Limiting 🟡

- Token bucket across multiple nodes
- **Exercise:** Distributed rate limiter with Redis

### Task 407 — Chaos Engineering 🔴

- Principles, tools (Chaos Monkey), fault injection
- **Exercise:** Chaos test your microservices (kill service, add latency, drop packets)

### Task 408 — Observability in Distributed Systems 🟡

- Metrics (Prometheus), Logging (ELK), Tracing (Jaeger) — the three pillars
- **Exercise:** Full observability stack for distributed system

### Task 409 — Distributed Systems Debugging 🟡

- Timeline reconstruction, causal analysis, distributed profiling
- **Exercise:** Debug a distributed system issue end-to-end

### Task 410 — Build: Microservices E-commerce ⚫

- **Phase 12 Final Project:**
  - 5 Services: User, Product, Order, Payment, Notification
  - API Gateway (routing, auth, rate limiting)
  - Communication: gRPC (sync) + RabbitMQ (async)
  - Database per service (PostgreSQL + MongoDB)
  - Saga for order processing
  - Distributed tracing (OpenTelemetry)
  - Centralized logging (ELK)
  - Circuit breakers, retries, timeouts
  - Docker Compose for local development

---

## ✅ Phase 12 Completion Checklist

- [ ] সব 30 tasks complete
- [ ] Microservice architecture design করতে পারো
- [ ] Service-to-service communication (gRPC, events) implement করতে পারো
- [ ] Distributed systems theory (CAP, consensus, consistency) বুঝো
- [ ] Observability (logs, metrics, traces) setup করতে পারো
- [ ] 5-service microservices system build করেছো

> _"Microservices জানলে তুমি architect। Distributed systems theory জানলে তুমি principal engineer।"_
