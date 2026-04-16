# Backend Phase 11 — Message Queues & Event-Driven Architecture (Tasks 351-380)

> **Modern backend = Event-driven backend। Services communicate via messages, not direct calls।**
> **RabbitMQ, Kafka, BullMQ — queue systems জানলে তুমি distributed systems build করতে পারবে।**
> **Event Sourcing, CQRS, Saga — advanced patterns জানলে তুমি architect level।**

---

## Section A: Message Queue Fundamentals (Tasks 351-360)

### Task 351 — Why Message Queues? 🟢

- Decouple services, async processing, load leveling, retry, reliability
- **Exercise:** Identify 10 use cases for message queues in your app

### Task 352 — Queue Patterns 🟡

- Point-to-Point (one consumer), Pub/Sub (multiple consumers), Request-Reply
- Fan-out, Fan-in, Work Queue, Priority Queue, Dead Letter Queue
- **Exercise:** Implement each pattern diagram

### Task 353 — RabbitMQ Deep Dive 🔴

- Exchanges (Direct, Fanout, Topic, Headers), Queues, Bindings, Routing Keys
- Message acknowledgment, prefetch, durability, TTL, dead letter
- **Exercise:** Complete RabbitMQ setup: producer, consumer, exchanges, error handling

### Task 354 — Apache Kafka Deep Dive 🔴

- Topics, Partitions, Offsets, Brokers, Consumer Groups, Replication
- Kafka vs RabbitMQ: when to use which
- At-most-once, At-least-once, Exactly-once delivery
- **Exercise:** Kafka setup: producer, consumer, consumer groups, stream processing

### Task 355 — BullMQ (Redis-based Queue) 🟡

- Job queue built on Redis: priority, delay, retry, rate limiting, concurrency
- **Exercise:** Background job system (email, reports, image processing)

### Task 356 — Message Serialization 🟢

- JSON, Protobuf, Avro, MessagePack for message format
- Schema registry, backward/forward compatibility
- **Exercise:** Compare serialization formats for queue messages

### Task 357 — Dead Letter Queues 🟡

- Messages that fail processing go to DLQ for investigation
- **Exercise:** DLQ system with retry, alerting, manual replay

### Task 358 — Message Ordering & Deduplication 🟡

- Partition-based ordering (Kafka), message deduplication
- **Exercise:** Exactly-once processing with deduplication

### Task 359 — Queue Monitoring 🟡

- Queue depth, processing rate, consumer lag, error rate
- **Exercise:** Queue monitoring dashboard

### Task 360 — Queue Selection Guide 🟢

- RabbitMQ: complex routing, RPC, moderate throughput
- Kafka: high throughput, event streaming, replay
- BullMQ: simple jobs, Node.js native, Redis-based
- SQS: AWS managed, serverless
- **Exercise:** Decision matrix for queue selection

---

## Section B: Event-Driven Patterns (Tasks 361-375)

### Task 361 — Event Sourcing 🔴

- Store events instead of current state:

  ```
  Instead of:  users table → { id: 1, name: "John", email: "john@x.com" }

  Store events:
    1. UserCreated { id: 1, name: "John", email: "john@old.com" }
    2. EmailChanged { id: 1, email: "john@x.com" }
    3. NameChanged { id: 1, name: "John Doe" }

  Current state = replay all events
  ```

- **Exercise:** Event sourcing for order management (create, update, cancel, refund)

### Task 362 — CQRS Implementation 🔴

- Write model (events/commands) separate from Read model (denormalized views)
- Write to event store → project to read database → query from read DB
- **Exercise:** CQRS system (write: PostgreSQL events, read: MongoDB denormalized)

### Task 363 — Saga Pattern 🔴

- Distributed transaction across services:

  ```
  Choreography: services react to events
  Order Created → Payment Charged → Inventory Reserved → Shipping Started

  Orchestration: central coordinator manages flow
  Saga Orchestrator → Payment Service → Inventory Service → Shipping Service
  (Each step has compensating action for rollback)
  ```

- **Exercise:** Order saga: create order → charge payment → reserve inventory (with compensation)

### Task 364 — Outbox Pattern 🟡

- Reliable event publishing: write to DB ও event in same transaction
- Outbox table → background process reads ও publishes to queue
- **Exercise:** Outbox pattern for reliable event delivery

### Task 365 — Event Bus (In-Process) 🟡

- EventEmitter-based in-process event bus
- **Exercise:** Domain event bus for clean architecture

### Task 366 — Event Schema Evolution 🟡

- Backward compatibility, schema versioning, event upcasting
- **Exercise:** Event schema migration without downtime

### Task 367 — Idempotent Consumers 🟡

- Handle duplicate messages safely
- **Exercise:** Idempotent message handler (check processed IDs)

### Task 368 — Event Replay 🟡

- Rebuild read models from event history
- **Exercise:** Replay engine for rebuilding projections

### Task 369 — Stream Processing 🔴

- Real-time event processing, windowing, aggregation
- **Exercise:** Real-time analytics using Kafka Streams or custom

### Task 370 — Webhook Delivery System 🟡

- Reliable webhook sending: queue → deliver → retry → DLQ
- **Exercise:** Webhook delivery service with retry ও monitoring

### Task 371 — Async API Patterns 🟡

- Fire and forget, Request-Reply, Publish-Subscribe, Event notification
- **Exercise:** Implement all async patterns

### Task 372 — Message Broker High Availability 🔴

- RabbitMQ clustering, Kafka replication, failover
- **Exercise:** HA message broker setup

### Task 373 — Backpressure in Event Systems 🟡

- Consumer slower than producer — buffer, drop, or slow down
- **Exercise:** Backpressure handling in queue consumers

### Task 374 — Event-Driven Microservice Communication 🔴

- Domain events between services, eventual consistency
- **Exercise:** 3 microservices communicating via events

### Task 375 — Build: Event-Driven Order System ⚫

- **Phase 11 Final Project:**
  - Order Service → Payment Service → Inventory Service → Notification Service
  - Communication via RabbitMQ/Kafka
  - Saga pattern (with compensation)
  - Event sourcing for order history
  - Dead letter queue for failed events
  - Monitoring: event flow, consumer lag, errors

---

## Section C: Advanced (Tasks 376-380)

### Task 376 — Distributed Tracing in Event Systems 🟡

- Correlation IDs across services ও queues
- **Exercise:** End-to-end tracing from API → queue → consumer

### Task 377 — Event Store Implementation 🔴

- Build your own event store (append-only, snapshots, subscriptions)
- **Exercise:** Event store with PostgreSQL + projections

### Task 378 — CDC (Change Data Capture) 🟡

- Database changes → events (Debezium, PostgreSQL logical replication)
- **Exercise:** CDC pipeline: PostgreSQL → Kafka → Elasticsearch

### Task 379 — Exactly-Once Processing 🔴

- Idempotent consumers + transactional outbox = exactly-once semantics
- **Exercise:** Prove exactly-once processing in your system

### Task 380 — Event System Architecture Document ⚫

- Document complete event-driven architecture decisions, patterns, tradeoffs

---

## ✅ Phase 11 Completion Checklist

- [ ] সব 30 tasks complete
- [ ] RabbitMQ ও Kafka both use করতে পারো
- [ ] Event Sourcing ও CQRS implement করতে পারো
- [ ] Saga pattern (orchestration ও choreography) বুঝো
- [ ] Dead letter queues, retry, idempotency handle করতে পারো

> _"Message queue জানলে তুমি distributed systems build করতে পারো — এটা senior skill।"_
