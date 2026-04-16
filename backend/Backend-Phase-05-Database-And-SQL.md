# Backend Phase 05 — Database Theory & SQL Deep Dive (Tasks 146-185)

> **Database = Backend এর heart। Database ভালো না জানলে সব কিছু slow, insecure, ও broken।**
> **এই phase এ তুমি database theory থেকে advanced SQL সব শিখবে।**
> **যেকোনো database (PostgreSQL, MySQL, SQLite) তুমি master করতে পারবে — কারণ theory universal।**

---

## Section A: Relational Database Theory (Tasks 146-160)

### Task 146 — Database Fundamentals 🟢

- What is a database, DBMS, RDBMS? File system vs Database
- Tables, Rows, Columns, Primary Key, Foreign Key, Constraints
- **Exercise:** ER Diagram design করো for a school management system

### Task 147 — Relational Algebra 🟡

- Select (σ), Project (π), Join (⋈), Union (∪), Intersection (∩), Difference (−)
- Cartesian Product, Division, Rename
- **Exercise:** 10 relational algebra expressions → convert to SQL

### Task 148 — Normalization (1NF → BCNF) 🔴

- **1NF:** Atomic values, no repeating groups
- **2NF:** No partial dependencies (all non-key depends on FULL primary key)
- **3NF:** No transitive dependencies (non-key depends only on key, not on other non-key)
- **BCNF:** Every determinant is a candidate key
- **Denormalization:** When to break rules for performance (read-heavy systems)
- **Exercise:** Normalize a messy database from UNF → 3NF, then strategically denormalize

### Task 149 — Entity-Relationship Modeling 🟡

- Entities, Attributes, Relationships (1:1, 1:N, M:N)
- Weak entities, Composite keys, Inheritance
- **Exercise:** ER diagrams for: e-commerce, social media, hospital management, LMS

### Task 150 — ACID Properties Deep Dive 🟡

- **Atomicity:** All or nothing (transaction either fully completes or fully rolls back)
- **Consistency:** Data always valid (constraints enforced)
- **Isolation:** Concurrent transactions don't interfere
- **Durability:** Committed data survives crashes (WAL — Write-Ahead Log)
- **Exercise:** Demonstrate each ACID property with SQL examples

### Task 151 — Transaction Isolation Levels 🔴

- **Read Uncommitted:** Dirty reads possible
- **Read Committed:** No dirty reads (default in PostgreSQL)
- **Repeatable Read:** No non-repeatable reads
- **Serializable:** No phantom reads (strictest, slowest)
- **Problems:** Dirty read, Non-repeatable read, Phantom read, Lost update
- **Exercise:** Reproduce each isolation problem, then fix with proper level

### Task 152 — Indexing Theory 🔴

- **B-Tree Index:** Default, good for range queries, equality
- **Hash Index:** Good for equality only (O(1) lookup)
- **GIN (Generalized Inverted Index):** Full-text search, JSON, arrays
- **GiST (Generalized Search Tree):** Geometric, spatial data
- **BRIN (Block Range Index):** Very large tables, ordered data
- **Composite Index:** Multiple columns (order matters!)
- **Covering Index:** All needed columns in index (index-only scan)
- When to index, when NOT to index (write overhead)
- **Exercise:** Create different index types, run EXPLAIN ANALYZE, compare

### Task 153 — Query Execution Plan 🔴

- How database executes your query:
  ```
  SQL Query → Parser → Planner/Optimizer → Executor → Result
  ```
- **EXPLAIN ANALYZE** — read execution plans
- Scan types: Sequential Scan, Index Scan, Index Only Scan, Bitmap Scan
- Join types: Nested Loop, Hash Join, Merge Join
- **Exercise:** 20 queries → EXPLAIN ANALYZE → identify slow parts → optimize

### Task 154 — SQL Joins Mastery 🟡

- INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN, CROSS JOIN, SELF JOIN
- Join conditions, multiple joins, join performance
- **Exercise:** Complex multi-table joins (5+ tables)

### Task 155 — Subqueries & CTEs 🟡

- Scalar subqueries, correlated subqueries, EXISTS, IN, ANY, ALL
- Common Table Expressions (WITH clause) — readable, reusable
- Recursive CTEs (hierarchical data — org chart, categories)
- **Exercise:** Convert complex subqueries → CTEs → compare readability & performance

### Task 156 — Window Functions 🔴

- ROW_NUMBER(), RANK(), DENSE_RANK(), NTILE()
- LAG(), LEAD() — access previous/next row
- SUM(), AVG(), COUNT() OVER (PARTITION BY ... ORDER BY ...)
- FIRST_VALUE(), LAST_VALUE(), NTH_VALUE()
- **Exercise:** Analytics queries: running total, ranking, moving average, YoY comparison

### Task 157 — Aggregation & Grouping Advanced 🟡

- GROUP BY, HAVING, ROLLUP, CUBE, GROUPING SETS
- Multiple aggregations, conditional aggregation (FILTER)
- **Exercise:** Sales report with subtotals, grand totals, cross-tabulation

### Task 158 — Data Types Deep Dive 🟡

- Numeric (INT, BIGINT, DECIMAL, FLOAT), String (VARCHAR, TEXT, CHAR)
- Date/Time (TIMESTAMP, DATE, TIME, INTERVAL, TIMESTAMPTZ)
- Boolean, UUID, JSON/JSONB, Array, Enum, Composite
- **Exercise:** Choose correct data types for 10 different schemas

### Task 159 — Constraints & Data Integrity 🟡

- PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK, DEFAULT, EXCLUDE
- ON DELETE CASCADE/SET NULL/RESTRICT
- Deferrable constraints (check at end of transaction)
- **Exercise:** Design schema with all constraint types

### Task 160 — Views, Materialized Views & Functions 🟡

- Views: virtual tables (query alias)
- Materialized Views: cached query results (refresh manually/periodically)
- Functions: PL/pgSQL stored procedures
- Triggers: automatic actions on data changes
- **Exercise:** Create views for reporting, materialized view for dashboard, trigger for audit log

---

## Section B: SQL Mastery (Tasks 161-175)

### Task 161 — DDL (Data Definition Language) 🟢

- CREATE, ALTER, DROP, TRUNCATE, RENAME
- Schema management, migrations concept
- **Exercise:** Complete schema for e-commerce from scratch

### Task 162 — DML (Data Manipulation Language) 🟢

- INSERT, UPDATE, DELETE, UPSERT (INSERT ... ON CONFLICT)
- Bulk insert, conditional update, soft delete
- **Exercise:** CRUD operations with all edge cases

### Task 163 — DQL (Data Query Language) Advanced 🟡

- SELECT with: WHERE, ORDER BY, LIMIT, OFFSET, DISTINCT, LIKE, ILIKE, IN, BETWEEN, IS NULL
- CASE WHEN, COALESCE, NULLIF, GREATEST, LEAST
- String functions: CONCAT, SUBSTRING, TRIM, UPPER, LOWER, LENGTH, REPLACE
- Date functions: NOW(), EXTRACT, DATE_TRUNC, AGE, INTERVAL
- **Exercise:** 50 SQL queries (easy → hard) covering all concepts

### Task 164 — JSON/JSONB Operations 🟡

- Store, query, index JSON data:

  ```sql
  -- Store JSON
  CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    metadata JSONB DEFAULT '{}'
  );

  -- Query JSON
  SELECT * FROM products WHERE metadata->>'brand' = 'Apple';
  SELECT * FROM products WHERE metadata @> '{"colors": ["red"]}';

  -- Index JSON
  CREATE INDEX idx_metadata ON products USING GIN (metadata);
  CREATE INDEX idx_brand ON products USING BTREE ((metadata->>'brand'));
  ```

- **Exercise:** Product catalog with dynamic attributes in JSONB

### Task 165 — Full-Text Search 🟡

- tsvector, tsquery, to_tsvector, to_tsquery, plainto_tsquery
- GIN index for full-text search
- Ranking (ts_rank, ts_rank_cd)
- **Exercise:** Search engine for articles (title + body, relevance ranking)

### Task 166 — Recursive Queries 🔴

- WITH RECURSIVE for hierarchical data:
  ```sql
  -- Organization hierarchy
  WITH RECURSIVE org_tree AS (
    SELECT id, name, manager_id, 1 AS depth
    FROM employees WHERE manager_id IS NULL -- Start: CEO
    UNION ALL
    SELECT e.id, e.name, e.manager_id, ot.depth + 1
    FROM employees e JOIN org_tree ot ON e.manager_id = ot.id
  )
  SELECT * FROM org_tree ORDER BY depth, name;
  ```
- **Exercise:** Category tree, file system paths, shortest path

### Task 167 — Transactions & Locking 🔴

- BEGIN, COMMIT, ROLLBACK, SAVEPOINT
- Row-level locking: FOR UPDATE, FOR SHARE
- Advisory locks, Deadlock detection ও prevention
- **Exercise:** Banking transfer with proper transaction isolation ও locking

### Task 168 — Database Migrations 🟡

- Version-controlled schema changes:

  ```js
  // Migration file: 001_create_users.js
  exports.up = async (db) => {
    await db.query(`
      CREATE TABLE users (
        id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
        name VARCHAR(100) NOT NULL,
        email VARCHAR(255) UNIQUE NOT NULL,
        password_hash VARCHAR(255) NOT NULL,
        role VARCHAR(20) DEFAULT 'user',
        created_at TIMESTAMPTZ DEFAULT NOW(),
        updated_at TIMESTAMPTZ DEFAULT NOW()
      )
    `);
    await db.query(`CREATE INDEX idx_users_email ON users (email)`);
  };

  exports.down = async (db) => {
    await db.query('DROP TABLE IF EXISTS users');
  };
  ```

- **Exercise:** Complete migration system build করো (up, down, status, rollback)

### Task 169 — Database Seeding 🟢

- Test data generation, faker.js
- **Exercise:** Seed script that creates 10,000 realistic users, products, orders

### Task 170 — Query Optimization 🔴

- EXPLAIN ANALYZE reading mastery
- Index selection, query rewriting, avoiding N+1
- Common anti-patterns: SELECT \*, unnecessary subqueries, missing indexes
- **Exercise:** Take 10 slow queries → optimize each → show before/after EXPLAIN

### Task 171 — Stored Procedures & Triggers 🟡

- PL/pgSQL functions for complex business logic
- Triggers for audit logs, updated_at, data validation
- **Exercise:** Audit trail trigger (log all changes to audit table)

### Task 172 — Partitioning 🔴

- Range partitioning (by date), List partitioning (by status), Hash partitioning
- **Exercise:** Partition orders table by month (millions of rows)

### Task 173 — Database Connection from Node.js 🟡

- `pg` (node-postgres) library, connection pooling, parameterized queries
- **Exercise:** Database module with connection pool, query builder, transaction support

### Task 174 — ORM vs Query Builder vs Raw SQL 🟡

- Prisma, Sequelize, Knex.js, raw `pg` — when to use which
- **Exercise:** Same queries in: raw SQL, Knex.js, Prisma — compare

### Task 175 — Database Design Patterns 🟡

- Soft delete, Polymorphic associations, EAV (Entity-Attribute-Value), Audit tables
- **Exercise:** Design patterns for 5 real-world scenarios

---

## Section C: Advanced Database (Tasks 176-185)

### Task 176 — Replication 🔴

- Primary-Replica (read scaling), Synchronous vs Asynchronous
- Read from replica, write to primary
- **Exercise:** Setup PostgreSQL streaming replication (primary + 2 replicas)

### Task 177 — Sharding 🔴

- Horizontal partitioning across servers
- Shard key selection, consistent hashing, cross-shard queries
- **Exercise:** Design sharding strategy for a social media app

### Task 178 — Backup & Recovery 🟡

- pg_dump, pg_restore, WAL archiving, Point-in-Time Recovery (PITR)
- **Exercise:** Automated backup script + test recovery

### Task 179 — Database Monitoring 🟡

- pg_stat_statements, pg_stat_activity, slow query log
- Connection pool monitoring, lock monitoring
- **Exercise:** Database monitoring dashboard

### Task 180 — Connection Pool Tuning 🟡

- Pool size formula: `connections = (core_count * 2) + effective_spindle_count`
- Max connections, idle timeout, connection lifetime
- **Exercise:** Load test with different pool sizes → find optimal

### Task 181 — Database Security 🟡

- Role-based access, row-level security, column encryption
- SQL injection prevention (ALWAYS use parameterized queries!)
- **Exercise:** Secure database setup (roles, permissions, RLS)

### Task 182 — Data Warehouse Concepts 🟢

- OLTP vs OLAP, Star Schema, Snowflake Schema, Fact ও Dimension tables
- **Exercise:** Design data warehouse schema for e-commerce analytics

### Task 183 — Time-Series Data 🟡

- TimescaleDB / time-partitioned tables
- **Exercise:** IoT sensor data storage ও querying

### Task 184 — Geospatial Data 🟡

- PostGIS extension, geometric types, spatial queries
- **Exercise:** Location-based search (find nearby restaurants)

### Task 185 — Build: Database Administration Dashboard ⚫

- **Phase 05 Final Project:**
  - Schema browser (tables, columns, indexes, constraints)
  - Query executor (run SQL, see results, EXPLAIN)
  - Performance monitor (slow queries, connections, locks)
  - Migration runner (up, down, status)
  - Backup manager (create, schedule, restore)
  - User/role management

---

## ✅ Phase 05 Completion Checklist

- [ ] সব 40 tasks complete
- [ ] Normalization (1NF-BCNF) explain করতে পারো
- [ ] Complex SQL queries (joins, CTEs, window functions) লিখতে পারো
- [ ] EXPLAIN ANALYZE পড়ে query optimize করতে পারো
- [ ] Database migrations design ও implement করতে পারো
- [ ] Transaction isolation levels বুঝো ও apply করতে পারো
- [ ] Replication ও Sharding concepts clear

> _"Database slow মানে application slow। Query optimize করতে পারলে তুমি hero।"_
