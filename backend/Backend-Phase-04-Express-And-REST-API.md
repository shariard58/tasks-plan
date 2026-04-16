# Backend Phase 04 — Express.js & REST API Mastery (Tasks 106-145)

> **Express.js = Node.js backend এর foundation। কিন্তু শুধু `app.get()` জানলে হবে না।**
> **Middleware architecture, error handling, security, API design patterns — সব master করো।**
> **Phase 01 এ তুমি নিজে HTTP framework build করেছো — এখন Express এর power fully বুঝবে।**

---

## Section A: Express.js Deep Dive (Tasks 106-120)

### Task 106 — Express Internals: How It Works 🟡

- Express source code পড়ো — কিভাবে routing, middleware chain, request/response enhancement কাজ করে
- **Exercise:** Express এর simplified version build করো (Task 35 থেকে expand)

### Task 107 — Middleware Architecture Mastery 🟡

- Middleware types: Application, Router, Error-handling, Built-in, Third-party
- Middleware execution order, next() behavior, error propagation
- **Exercise:** 10টা custom middleware build করো (auth, logger, validator, rateLimit, cors, compression, helmet, timeout, requestId, responseTime)

### Task 108 — Router & Route Organization 🟡

- express.Router() দিয়ে modular routing
- Route grouping, prefixing, nested routers
- **Exercise:** Complete API routes organize করো (users, products, orders, auth)

### Task 109 — Request Object Deep Dive 🟢

- `req.params`, `req.query`, `req.body`, `req.headers`, `req.cookies`, `req.ip`, `req.method`, `req.path`, `req.originalUrl`
- `req.get()`, `req.accepts()`, `req.is()`
- **Exercise:** Request inspector middleware build করো

### Task 110 — Response Object Deep Dive 🟢

- `res.json()`, `res.send()`, `res.status()`, `res.redirect()`, `res.cookie()`, `res.clearCookie()`, `res.set()`, `res.type()`, `res.download()`, `res.sendFile()`
- Response formatting patterns
- **Exercise:** Consistent response format wrapper (success, error, paginated)

### Task 111 — Error Handling in Express 🟡

- Sync vs Async error handling, asyncHandler wrapper
- Custom error classes (AppError, NotFoundError, ValidationError, AuthError)
- Global error handler middleware
- **Exercise:** Complete error handling system (Phase 03 Task 74 express version)

### Task 112 — Request Validation with Joi/Zod 🟡

- Schema-based validation:

  ```js
  const Joi = require('joi');

  const userSchema = Joi.object({
    name: Joi.string().min(2).max(50).required(),
    email: Joi.string().email().required(),
    password: Joi.string()
      .min(8)
      .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
      .required(),
    age: Joi.number().integer().min(13).max(120),
    role: Joi.string().valid('user', 'admin', 'moderator').default('user'),
  });

  // Validation middleware
  function validate(schema) {
    return (req, res, next) => {
      const { error, value } = schema.validate(req.body, { abortEarly: false });
      if (error) {
        const details = error.details.map((d) => ({ field: d.path.join('.'), message: d.message }));
        throw new ValidationError(details);
      }
      req.body = value; // Use validated/sanitized data
      next();
    };
  }

  app.post('/api/users', validate(userSchema), createUser);
  ```

- **Exercise:** Validation middleware + schemas for entire API

### Task 113 — File Upload Handling 🟡

- Multer middleware, file validation, storage strategies, image processing
- **Exercise:** Avatar upload system (resize, crop, store, serve)

### Task 114 — Pagination Patterns 🟡

- Offset-based vs Cursor-based pagination
- Cursor-based is better for large datasets (no `OFFSET` performance issue)
- **Exercise:** Both pagination types implement করো with proper response format

### Task 115 — API Versioning Implementation 🟢

- URL versioning (`/api/v1/`, `/api/v2/`), Header versioning
- Router-based version management
- **Exercise:** API with v1 ও v2 simultaneously running

### Task 116 — Content Negotiation & Serialization 🟢

- Accept header based response format (JSON, XML, CSV)
- Response transformers/serializers
- **Exercise:** Multi-format API (one endpoint, multiple formats)

### Task 117 — HATEOAS & Richardson Maturity Model 🟢

- Level 0: HTTP, Level 1: Resources, Level 2: HTTP Verbs, Level 3: Hypermedia (HATEOAS)
- **Exercise:** HATEOAS-compliant API responses

### Task 118 — Compression & Performance Middleware 🟢

- Gzip/Brotli compression, ETag caching, response time header
- **Exercise:** Performance middleware suite

### Task 119 — Security Middleware 🟡

- Helmet.js headers, CSRF protection, XSS prevention, SQL injection prevention
- **Exercise:** Security middleware chain (build your own helmet)

### Task 120 — Express.js Alternatives 🟡

- Fastify (performance), Koa (modern), Hapi (config-driven), NestJS (enterprise)
- Build same API in Express, Fastify, ও Koa — compare
- **Exercise:** Same CRUD API in 3 frameworks → benchmark

---

## Section B: Advanced API Patterns (Tasks 121-135)

### Task 121 — RESTful Resource Design 🟡

- Nested resources, sub-resources, relationships
- `/users/:id/orders/:orderId/items`
- **Exercise:** Complete e-commerce API resource design

### Task 122 — Bulk Operations 🟡

- Batch create, update, delete
- Transaction-based bulk operations
- **Exercise:** Bulk user import (CSV → validate → create 1000 users)

### Task 123 — Search & Filtering 🟡

- Query parameter parsing, complex filters, full-text search
- `?filter[status]=active&filter[role]=admin&sort=-createdAt&fields=id,name`
- **Exercise:** Advanced query builder from URL params

### Task 124 — Idempotency Implementation 🟡

- Idempotency keys for POST requests
- Redis-based idempotency store
- **Exercise:** Idempotent payment API

### Task 125 — Webhook System 🟡

- Webhook registration, delivery, retry, signature verification
- **Exercise:** Complete webhook system (register, deliver, retry, verify)

### Task 126 — API Rate Limiting Advanced 🟡

- Per-user, per-endpoint, tiered rate limiting
- Redis-based distributed rate limiting
- **Exercise:** Full rate limiting system with Redis

### Task 127 — Long-running Operations 🟡

- Start operation → return job ID → poll for status
- `POST /api/reports` → `202 Accepted` → `GET /api/reports/:jobId` → `200 OK` with result
- **Exercise:** Report generation API (async processing)

### Task 128 — API Health & Readiness 🟢

- `/health` (liveness), `/ready` (readiness — DB connected? Redis connected?)
- **Exercise:** Health check system (check all dependencies)

### Task 129 — Request Logging & Tracing 🟡

- Structured request logs (JSON), Request ID propagation, Correlation ID
- **Exercise:** Full request tracing system (AsyncLocalStorage-based)

### Task 130 — API Documentation Auto-generation 🟢

- Swagger/OpenAPI from code, JSDoc to OpenAPI
- **Exercise:** Auto-generated API docs with Swagger UI

### Task 131 — GraphQL Server with Express 🔴

- Apollo Server / graphql-http integration
- Schema, Resolvers, Mutations, Subscriptions
- DataLoader for N+1 query problem
- **Exercise:** Complete GraphQL API (queries, mutations, subscriptions, pagination)

### Task 132 — File Download & Export 🟡

- CSV export, PDF generation, Excel export
- Streaming large exports
- **Exercise:** Export users as CSV/PDF/Excel (streaming for large data)

### Task 133 — Email Sending 🟡

- Nodemailer setup, HTML templates, queue-based sending
- **Exercise:** Email service (welcome, reset password, notifications)

### Task 134 — Scheduled Jobs Integration 🟡

- node-cron / Agenda.js for scheduled tasks
- **Exercise:** Scheduled report generation, cleanup jobs, notification scheduler

### Task 135 — API Testing Strategy 🟡

- Unit tests (services), Integration tests (API endpoints), E2E tests
- Supertest for HTTP testing
- **Exercise:** Test suite for complete API (90%+ coverage)

---

## Section C: Production API (Tasks 136-145)

### Task 136 — Express.js Production Checklist 🟡

- Security headers, HTTPS, rate limiting, input validation, error handling, logging, monitoring
- **Exercise:** Audit checklist + fix all issues

### Task 137 — API Gateway Pattern 🟡

- Single entry point, routing to services, auth, rate limiting, logging
- **Exercise:** Simple API gateway that routes to multiple backend services

### Task 138 — Multi-tenancy 🔴

- Database per tenant, Schema per tenant, Row-level isolation
- **Exercise:** Multi-tenant API (tenant identification from subdomain/header)

### Task 139 — Internationalization (i18n) 🟢

- Multi-language error messages, content localization
- **Exercise:** API with English + Bangla error messages

### Task 140 — Feature Flags 🟡

- Toggle features without deployment
- **Exercise:** Feature flag system (percentage rollout, user targeting)

### Task 141 — A/B Testing Backend 🟡

- Experiment assignment, metric tracking, statistical analysis
- **Exercise:** A/B testing framework for API responses

### Task 142 — API Analytics 🟡

- Track: requests/sec, response times, error rates, popular endpoints
- **Exercise:** Analytics middleware + dashboard endpoint

### Task 143 — Serverless Express (AWS Lambda) 🟡

- Express on Lambda using serverless-http
- **Exercise:** Deploy Express API to AWS Lambda

### Task 144 — Express + TypeScript Full Setup 🟡

- TypeScript config, typed middleware, typed routes, typed models
- **Exercise:** Full TypeScript Express API with type safety

### Task 145 — Build: Complete E-commerce API ⚫

- **Phase 04 Final Project:**
  - Users (register, login, profile, roles)
  - Products (CRUD, categories, search, filters)
  - Cart (add, remove, update quantity)
  - Orders (create, payment, status tracking)
  - Reviews (create, update, aggregate rating)
  - Admin (dashboard, user management, analytics)
  - All middleware: auth, validation, rate limiting, logging, error handling
  - API documentation (Swagger)
  - Full test suite

---

## ✅ Phase 04 Completion Checklist

- [ ] সব 40 tasks complete
- [ ] Express middleware architecture master করেছো
- [ ] REST API design principles follow করো
- [ ] Error handling, validation, security — production-ready
- [ ] Complete e-commerce API build করেছো
- [ ] API documentation auto-generated
- [ ] Test suite written

> _"Express শুধু framework না — Express হলো তোমার backend এর backbone।"_
