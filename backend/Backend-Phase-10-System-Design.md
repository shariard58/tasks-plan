# Backend Phase 10 — System Design & Architecture (Tasks 316-350)

> **System Design = Interview Pass + Career Growth। FAANG/top companies এ এটা #1 skill।**
> **একটা system কিভাবে millions of users serve করবে — এটা design করতে পারলে তুমি architect।**
> **এই phase senior backend devs কে separate করে juniors থেকে।**

---

## Section A: System Design Fundamentals (Tasks 316-330)

### Task 316 — Scalability Concepts 🟡

- Vertical scaling (bigger machine) vs Horizontal scaling (more machines)
- Stateless vs Stateful services
- **Exercise:** Scale a simple API from 100 to 1M users — document changes

### Task 317 — Load Balancing 🟡

- Algorithms: Round Robin, Weighted, Least Connections, IP Hash, Consistent Hashing
- L4 (TCP) vs L7 (HTTP) load balancing
- **Exercise:** Load balancer comparison with benchmarks

### Task 318 — Database Scaling Patterns 🔴

- Read replicas, Write-through cache, Sharding, Partitioning
- Connection pooling, Query optimization at scale
- **Exercise:** Scale database from 1M to 100M rows — document approach

### Task 319 — Consistent Hashing 🔴

- Hash ring, virtual nodes, node addition/removal with minimal redistribution
- **Exercise:** Consistent hashing implementation (used in load balancing, caching, sharding)

### Task 320 — CDN & Edge Computing 🟡

- Content Delivery Networks, edge caching, origin shield
- **Exercise:** CDN strategy for global application

### Task 321 — Rate Limiting at Scale 🟡

- Distributed rate limiting with Redis, sliding window algorithm
- **Exercise:** Rate limiter for 100K+ requests/sec

### Task 322 — API Gateway 🟡

- Routing, auth, rate limiting, transformation, aggregation
- Kong, AWS API Gateway, custom gateway
- **Exercise:** Build API gateway that routes to multiple services

### Task 323 — Service Discovery 🟡

- Client-side vs Server-side discovery
- Consul, etcd, Kubernetes DNS
- **Exercise:** Service registry ও discovery system

### Task 324 — Circuit Breaker Pattern 🟡

- Prevent cascade failures when dependent service is down:
  ```
  Closed (normal) → Open (failing) → Half-Open (testing) → Closed
  ```
- **Exercise:** Circuit breaker library (with timeout, retry, fallback)

### Task 325 — Bulkhead Pattern 🟢

- Isolate failures: separate thread pools/connections per service
- **Exercise:** Bulkhead implementation for external API calls

### Task 326 — Back Pressure 🟡

- When producer is faster than consumer — slow down or buffer
- **Exercise:** Back pressure system in queue processing

### Task 327 — Idempotency at Scale 🟡

- Idempotency keys, exactly-once processing
- **Exercise:** Distributed idempotency system

### Task 328 — Data Replication Patterns 🔴

- Single-leader, Multi-leader, Leaderless replication
- Conflict resolution strategies
- **Exercise:** Document replication strategy for global application

### Task 329 — Consensus Algorithms 🔴

- Raft algorithm (leader election, log replication)
- Paxos (theoretical foundation)
- **Exercise:** Simplified Raft implementation

### Task 330 — CAP Theorem in Practice 🟡

- Real-world tradeoffs: consistency vs availability
- CP systems: PostgreSQL, Redis Cluster
- AP systems: Cassandra, DynamoDB
- **Exercise:** Design system for both CP ও AP requirements

---

## Section B: System Design Interview Problems (Tasks 331-345)

### Task 331 — Design: URL Shortener (like bit.ly) 🟡

- Requirements: shorten URL, redirect, analytics, custom aliases
- Scale: 100M URLs, 10K writes/sec, 100K reads/sec
- Components: API, Hash generation, Database, Cache, Analytics

### Task 332 — Design: Rate Limiter 🟡

- Requirements: per-user, per-API, sliding window, distributed
- Scale: 1M+ users, sub-millisecond latency

### Task 333 — Design: Chat System (like WhatsApp) 🔴

- Requirements: 1:1 chat, group chat, online status, message delivery, read receipts
- Components: WebSocket servers, Message queue, Database, Push notifications

### Task 334 — Design: News Feed (like Facebook/Twitter) 🔴

- Requirements: Post, Follow, Feed generation, Ranking
- Fan-out on write vs Fan-out on read
- Components: Feed service, Timeline cache, Ranking algorithm

### Task 335 — Design: Notification System 🟡

- Requirements: Email, SMS, Push, In-app, User preferences, Templates
- Scale: 1M+ notifications/day, priority, retry

### Task 336 — Design: File Storage (like S3/Google Drive) 🔴

- Requirements: Upload, Download, Share, Versioning, Large files
- Components: Metadata DB, Block storage, CDN, Dedup

### Task 337 — Design: Video Streaming (like YouTube) 🔴

- Requirements: Upload, Transcode, Stream, Search, Recommendations
- Components: Transcoding pipeline, CDN, Adaptive bitrate

### Task 338 — Design: E-commerce Platform 🔴

- Requirements: Catalog, Cart, Order, Payment, Inventory, Search
- Distributed transactions, consistency, high availability

### Task 339 — Design: Search Engine 🔴

- Requirements: Crawl, Index, Search, Rank
- Inverted index, PageRank, Distributed indexing

### Task 340 — Design: Payment System 🔴

- Requirements: Process payments, Refunds, Reconciliation
- Exactly-once processing, audit trail, PCI compliance

### Task 341 — Design: Authentication System at Scale 🟡

- Requirements: Login, SSO, MFA, Session management, 1B+ users
- Token service, Session store, Key management

### Task 342 — Design: Metrics/Monitoring System 🟡

- Requirements: Collect, Store, Query, Alert, Dashboard
- Time-series DB, aggregation, real-time processing

### Task 343 — Design: Job Scheduler (like Cron at scale) 🟡

- Requirements: Schedule jobs, Execute at time, Retry, Monitor
- Distributed scheduling, exactly-once execution

### Task 344 — Design: API Rate Limiter Service 🟡

- Requirements: Multi-tenant, configurable, distributed
- Redis-based, Lua scripts, sliding window

### Task 345 — Design: Real-time Analytics Dashboard 🔴

- Requirements: Event ingestion, Processing, Aggregation, Dashboard
- Stream processing, pre-computation, WebSocket updates

---

## Section C: Architecture Patterns (Tasks 346-350)

### Task 346 — Clean Architecture 🟡

- Entities → Use Cases → Interface Adapters → Frameworks
- Dependency Rule: inner layers know nothing about outer layers
- **Exercise:** Refactor existing app to Clean Architecture

### Task 347 — Domain-Driven Design (DDD) 🔴

- Bounded Contexts, Aggregates, Entities, Value Objects, Domain Events
- Ubiquitous Language, Anti-Corruption Layer
- **Exercise:** DDD design for e-commerce domain

### Task 348 — Event-Driven Architecture 🟡

- Event producers, consumers, event store, eventual consistency
- **Exercise:** Event-driven order processing system

### Task 349 — Hexagonal Architecture (Ports & Adapters) 🟡

- Core domain → Ports (interfaces) → Adapters (implementations)
- **Exercise:** Hexagonal architecture for user service

### Task 350 — Architecture Decision Records (ADR) ⚫

- **Phase 10 Final Project:**
  - Document 15 architecture decisions for a system:
    1. Why PostgreSQL over MySQL
    2. Why Redis for caching
    3. Why JWT over sessions
    4. Monolith vs Microservices decision
    5. Event sourcing: yes or no
       ... (10 more)
  - Each ADR: Context, Decision, Consequences
  - Design complete system architecture for a SaaS product

---

## ✅ Phase 10 Completion Checklist

- [ ] সব 35 tasks complete
- [ ] 15 system design problems solve করতে পারো
- [ ] Scalability patterns (load balancing, caching, sharding) explain করতে পারো
- [ ] Architecture patterns (Clean, DDD, Event-Driven) apply করতে পারো
- [ ] System design interview confident

> _"System Design জানলে তুমি Senior → Principal → Architect path এ আছো।"_
