# Backend Phase 07 — MongoDB & NoSQL Patterns (Tasks 216-250)

> **SQL সব problem solve করে না। NoSQL কখন ও কেন use করবে — এটা জানা critical।**
> **MongoDB = most popular document database। Schema design, aggregation, indexing — সব শিখবে।**
> **SQL + NoSQL দুটোই জানলে তুমি যেকোনো data problem solve করতে পারবে।**

---

## Section A: NoSQL Theory (Tasks 216-222)

### Task 216 — NoSQL Database Types 🟢

- Document (MongoDB), Key-Value (Redis), Column-Family (Cassandra), Graph (Neo4j), Time-Series (InfluxDB)
- When to use which type
- **Exercise:** 10 scenarios → choose SQL or NoSQL → justify

### Task 217 — CAP Theorem 🟡

- **Consistency:** Every read receives the most recent write
- **Availability:** Every request receives a response
- **Partition Tolerance:** System works despite network failures
- You can only guarantee 2 of 3 (CP, AP, CA)
- **Exercise:** Classify 10 databases by CAP — explain tradeoffs

### Task 218 — SQL vs NoSQL Decision Matrix 🟡

- When SQL: complex queries, transactions, relationships, ACID
- When NoSQL: flexible schema, high write throughput, horizontal scaling, denormalized data
- **Exercise:** Architecture decision document for 5 different apps

### Task 219 — Document Model Design 🟡

- Embedding vs Referencing: when to embed documents vs store references
- **Exercise:** Design MongoDB schemas for: blog, e-commerce, social media

### Task 220 — Data Modeling Patterns 🟡

- Attribute Pattern, Bucket Pattern, Computed Pattern, Extended Reference, Outlier Pattern, Polymorphic Pattern, Schema Versioning, Subset Pattern, Tree Pattern
- **Exercise:** Apply each pattern to real scenarios

### Task 221 — Eventual Consistency 🟡

- Strong vs Eventual consistency, read-your-writes, monotonic reads
- **Exercise:** Build system demonstrating eventual consistency behavior

### Task 222 — BASE vs ACID 🟢

- **ACID:** Atomicity, Consistency, Isolation, Durability (SQL)
- **BASE:** Basically Available, Soft state, Eventually consistent (NoSQL)

---

## Section B: MongoDB Mastery (Tasks 223-240)

### Task 223 — MongoDB Architecture 🟡

- WiredTiger storage engine, documents, collections, databases
- BSON format, \_id field, ObjectId anatomy
- **Exercise:** MongoDB internals exploration

### Task 224 — CRUD Operations Deep 🟡

- insertOne/Many, find, updateOne/Many, deleteOne/Many, replaceOne
- Operators: $set, $unset, $inc, $push, $pull, $addToSet, $each, $elemMatch
- **Exercise:** Complex CRUD operations with nested documents

### Task 225 — MongoDB Indexing 🔴

- Single field, Compound, Multikey (array), Text, Geospatial, Hashed, Wildcard
- Index intersection, covered queries, index hints
- **Exercise:** Design indexes for 10 query patterns → verify with explain()

### Task 226 — Aggregation Pipeline 🔴

- $match, $group, $project, $sort, $limit, $skip, $unwind, $lookup (join), $facet
- $bucket, $graphLookup, $merge, $out, $addFields, $replaceRoot
- **Exercise:** Complex analytics: sales reports, user behavior analysis, funnel

### Task 227 — Mongoose ODM 🟡

- Schema definition, middleware (hooks), virtuals, statics, methods
- Population (references), discriminators (inheritance)
- **Exercise:** Full Mongoose model layer with validation, hooks, population

### Task 228 — Schema Design for Performance 🟡

- Read-heavy vs Write-heavy optimization
- Embedding for read performance, referencing for write performance
- Document size limits (16MB), array growth patterns
- **Exercise:** Schema design review ও optimization for existing app

### Task 229 — Transactions in MongoDB 🟡

- Multi-document ACID transactions (replica set required)
- When to use transactions vs when schema design eliminates need
- **Exercise:** Order processing with multi-collection transaction

### Task 230 — Change Streams 🟡

- Real-time data change notifications:
  ```js
  const changeStream = db.collection('orders').watch();
  changeStream.on('change', (change) => {
    if (change.operationType === 'insert') {
      notifyUser(change.fullDocument);
    }
  });
  ```
- **Exercise:** Real-time order tracking using change streams

### Task 231 — MongoDB Full-Text Search 🟡

- Text indexes, $text, Atlas Search (Lucene-based)
- **Exercise:** Product search with faceted filtering

### Task 232 — Geospatial Queries 🟡

- 2dsphere index, $near, $geoWithin, $geoIntersects
- **Exercise:** Location-based service (find nearby, distance calculation)

### Task 233 — Time-Series Collections 🟡

- Optimized for time-series data (IoT, metrics, logs)
- **Exercise:** Metrics collection system with time-series collection

### Task 234 — GridFS — Large File Storage 🟢

- Store files > 16MB in MongoDB
- **Exercise:** File upload/download system using GridFS

### Task 235 — Replica Sets 🔴

- Primary-Secondary-Arbiter, automatic failover, read preferences
- Write concern, read concern
- **Exercise:** Setup 3-node replica set, test failover

### Task 236 — Sharding 🔴

- Shard key selection, chunks, balancer, mongos router
- Range-based vs Hash-based sharding
- **Exercise:** Design sharding strategy, understand shard key impact

### Task 237 — MongoDB Security 🟡

- Authentication, authorization, roles, encryption at rest/transit
- **Exercise:** Secure MongoDB deployment

### Task 238 — MongoDB Performance Tuning 🟡

- mongostat, mongotop, profiler, explain()
- Connection pooling, read/write optimization
- **Exercise:** Profile ও optimize 10 slow queries

### Task 239 — MongoDB Atlas (Cloud) 🟢

- Managed MongoDB: setup, scaling, monitoring, backups
- **Exercise:** Deploy to Atlas, configure monitoring

### Task 240 — Data Migration (SQL ↔ MongoDB) 🟡

- Migrate from PostgreSQL to MongoDB ও vice versa
- **Exercise:** Migration scripts between SQL ও MongoDB

---

## Section C: Other NoSQL & Multi-Model (Tasks 241-250)

### Task 241 — Redis as Primary Database 🟡

- Redis data structures for application state, Redis persistence (RDB, AOF)
- **Exercise:** Session store, leaderboard, rate limiter using Redis

### Task 242 — Elasticsearch Basics 🟡

- Document indexing, full-text search, aggregations
- **Exercise:** Log search engine using Elasticsearch

### Task 243 — Neo4j & Graph Databases 🟡

- Nodes, relationships, properties, Cypher query language
- **Exercise:** Social network recommendation engine

### Task 244 — DynamoDB Concepts 🟢

- Partition key, sort key, GSI, LSI, on-demand vs provisioned
- **Exercise:** DynamoDB table design for a URL shortener

### Task 245 — Multi-Model Architecture 🟡

- PostgreSQL (transactional) + MongoDB (documents) + Redis (cache) + Elasticsearch (search)
- **Exercise:** Design architecture using multiple databases

### Task 246 — Database Comparison Matrix 🟢

- PostgreSQL vs MySQL vs MongoDB vs Redis vs Cassandra vs DynamoDB
- **Exercise:** Comparison document with use cases, pros, cons

### Task 247 — Data Consistency Patterns 🟡

- Saga pattern, Two-Phase Commit, Eventual consistency with compensation
- **Exercise:** Distributed transaction with compensation

### Task 248 — CQRS (Command Query Responsibility Segregation) 🔴

- Separate read/write models, event sourcing companion
- **Exercise:** CQRS system (write to PostgreSQL, read from MongoDB)

### Task 249 — Database Connection Management 🟡

- Connection pooling for multiple databases, health checks, graceful shutdown
- **Exercise:** Database manager that handles PostgreSQL + MongoDB + Redis connections

### Task 250 — Build: Multi-Database E-commerce Backend ⚫

- **Phase 07 Final Project:**
  - PostgreSQL: Users, Orders, Payments (transactional)
  - MongoDB: Products, Reviews, Categories (flexible schema)
  - Redis: Sessions, Cart, Cache
  - Elasticsearch: Product search
  - Proper connection management, health checks, migrations

---

## ✅ Phase 07 Completion Checklist

- [ ] সব 35 tasks complete
- [ ] SQL vs NoSQL tradeoffs clearly explain করতে পারো
- [ ] MongoDB schema design patterns apply করতে পারো
- [ ] Aggregation pipeline complex queries লিখতে পারো
- [ ] MongoDB indexing, replication, sharding বুঝো
- [ ] Multi-database architecture design করতে পারো

> _"SQL + NoSQL দুটোই জানলে কোনো data problem তোমাকে আটকাতে পারবে না।"_
