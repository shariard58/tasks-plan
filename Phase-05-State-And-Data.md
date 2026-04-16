# Phase 05 — State Management & Data Layer (Tasks 201-250)

> Large-scale apps এ state management ও data handling হলো সবচেয়ে challenging part।
> 🟢 Easy | 🟡 Medium | 🔴 Hard | ⚫ Expert

---

## Section A: State Management Deep Dive (Tasks 201-218)

### Task 201 — State Management Mental Model 🟡

- Interactive visualization:
  - Local state (useState) — when/why
  - Lifted state — when/why
  - Context — when/why
  - External store (Zustand/Redux) — when/why
  - Server state (React Query) — when/why
  - URL state (searchParams) — when/why
  - Decision tree: "Which state type should I use?"
- **Focus:** State management decision-making

### Task 202 — Zustand Advanced Patterns 🟡

- Zustand mastery:
  - Slice pattern for large stores
  - Computed/derived state with subscriptions
  - Middleware: persist, devtools, immer
  - Async actions with loading states
  - Transient updates (no re-render)
  - Store composition
  - Testing Zustand stores
  - TypeScript strict types for stores
- **Tech:** Zustand

### Task 203 — Zustand + Immer Integration 🟡

- Immutable state updates made easy:
  - Nested state updates with Immer
  - Array manipulation (add, remove, update item)
  - Deep nested object updates
  - Compare: manual spread vs Immer
  - Performance implications
- **Tech:** Zustand, Immer

### Task 204 — Jotai Advanced Patterns 🟡

- Jotai deep dive:
  - Atom family for dynamic atoms
  - Atom effects (side effects)
  - Async atoms with Suspense
  - Atom composition patterns
  - Atom with storage (persist)
  - Jotai devtools
  - When Jotai > Zustand (and vice versa)
- **Tech:** Jotai

### Task 205 — Redux Toolkit Query (RTK Query) 🔴

- RTK Query complete:
  - API slice definition
  - Endpoints (queries ও mutations)
  - Cache management
  - Tag-based cache invalidation
  - Optimistic updates
  - Polling
  - Infinite scroll with RTK Query
  - Code generation from OpenAPI
- **Tech:** Redux Toolkit, RTK Query
- **Focus:** Enterprise-level data fetching

### Task 206 — Valtio Proxy State 🟡

- Proxy-based state:
  - Proxy state creation
  - Reactive subscriptions
  - Snapshot for reading
  - Nested proxy objects
  - Array operations
  - Compare: Proxy (Valtio) vs Snapshot (Zustand)
- **Tech:** Valtio
- **Focus:** Different mental model for state

### Task 207 — XState State Machines 🔴

- Formal state machines:
  - Define states ও transitions
  - Guards (conditional transitions)
  - Actions (side effects on transition)
  - Context (extended state)
  - Parallel states
  - Hierarchical states (nested)
  - XState visualizer
  - Real use case: checkout flow
- **Tech:** XState
- **Focus:** Complex UI logic — no more boolean soup

### Task 208 — Global State vs Server State 🟡

- Clear separation:
  - Client state: theme, UI toggles, form state
  - Server state: user data, posts, products
  - Server state tool: React Query
  - Client state tool: Zustand
  - Build app using both correctly
  - Anti-patterns: storing server data in Redux
- **Focus:** Modern state architecture

### Task 209 — URL State Management 🟡

- URL as state store:
  - Filters in URL (?category=tech&sort=date)
  - Pagination in URL (?page=2&limit=20)
  - Tab selection in URL (#tab=settings)
  - nuqs library for type-safe URL state
  - Share filtered view via URL
  - Browser back/forward works correctly
- **Tech:** nuqs (Next.js URL state)
- **Focus:** Shareable, bookmarkable application state

### Task 210 — Form State Architecture 🟡

- Complex form state patterns:
  - Multi-step form with shared state
  - Form state vs app state separation
  - Unsaved changes detection
  - Form autosave (debounced)
  - Form state restoration (browser refresh)
  - Dependent form values
  - Form validation state
- **Tech:** React Hook Form, Zustand

### Task 211 — Undo/Redo System 🔴

- Complete undo/redo:
  - Action history stack
  - Undo (pop from history, push to future)
  - Redo (pop from future, push to history)
  - Clear history
  - History limit (max 50 steps)
  - UI: undo/redo buttons with keyboard shortcuts
  - Apply to: drawing app বা text editor
- **Focus:** Command pattern implementation

### Task 212 — Offline-First State Management 🔴

- Offline support:
  - Detect online/offline status
  - Queue mutations when offline
  - Sync when back online
  - Conflict resolution (last-write-wins vs merge)
  - Persist state to IndexedDB
  - Optimistic UI while offline
  - Sync status indicator
- **Tech:** Zustand + persist, IndexedDB

### Task 213 — State Normalization 🔴

- Normalized state:
  - Entity pattern (byId ও allIds)
  - Normalize nested API response
  - Denormalize for UI rendering
  - Update normalized entities
  - Relationships between entities
  - Compare: normalized vs nested state
- **Focus:** Scalable state structure for large apps

### Task 214 — Cross-Tab State Sync 🟡

- Multi-tab synchronization:
  - BroadcastChannel API
  - localStorage event listener
  - Sync auth state across tabs (login/logout)
  - Sync theme across tabs
  - Leader election (one tab does the work)
- **Focus:** Multi-tab UX

### Task 215 — State Migration & Versioning 🟡

- Persisted state evolution:
  - State version tracking
  - Migration functions (v1 → v2 → v3)
  - Zustand persist with migrations
  - Backward compatibility
  - Reset corrupt state
- **Focus:** Real-world persisted state maintenance

### Task 216 — Optimistic UI Patterns Collection 🟡

- 8 optimistic patterns:
  1. Add item to list
  2. Remove item from list
  3. Toggle like/unlike
  4. Update item (inline edit)
  5. Reorder list (drag & drop)
  6. Add comment
  7. Change status
  8. Bulk operations
- Each with error rollback
- **Focus:** Optimistic UI mastery

### Task 217 — State Debugging Techniques 🟡

- Debugging tools ও techniques:
  - React DevTools state inspection
  - Zustand devtools
  - Redux DevTools
  - React Query DevTools
  - Custom state logger middleware
  - Time-travel debugging
  - State snapshot comparison
- **Focus:** Production debugging skills

### Task 218 — State Management Performance 🔴

- Performance optimization:
  - Selector optimization (avoid unnecessary re-renders)
  - Zustand `shallow` equality
  - React Query `select` for data transformation
  - Context splitting to prevent re-renders
  - Measure re-renders with React Profiler
  - Benchmark different solutions
  - Document optimization results
- **Focus:** State performance — senior-level skill

---

## Section B: Data Fetching Advanced (Tasks 219-232)

### Task 219 — React Query Infinite Scroll 🟡

- Production infinite scroll:
  - `useInfiniteQuery` setup
  - Cursor-based pagination
  - Offset-based pagination
  - Bidirectional infinite scroll
  - Scroll position restoration
  - Combined with virtualization
  - Loading ও error states
- **Tech:** @tanstack/react-query

### Task 220 — React Query + Optimistic Mutations 🟡

- Optimistic CRUD:
  - Create with optimistic add to list
  - Update with optimistic change
  - Delete with optimistic remove
  - Proper rollback on failure
  - Query cache manipulation
  - Cancel outgoing queries on mutation
- **Tech:** @tanstack/react-query

### Task 221 — React Query Cache Strategies 🔴

- Caching deep dive:
  - staleTime vs gcTime (cacheTime)
  - Window focus refetching
  - Network reconnect refetching
  - Polling intervals
  - Background refetching
  - Placeholder ও initial data
  - Query prefetching
  - Manual cache manipulation
- **Focus:** Cache tuning for optimal UX

### Task 222 — API Layer Architecture 🟡

- Structured API layer:
  - API client class with interceptors
  - Service layer (UserService, ProductService)
  - Type-safe API functions
  - Error transformation
  - Request/Response types
  - API versioning
  - Mock service for development
- **Focus:** Clean API architecture

### Task 223 — GraphQL with urql 🟡

- Alternative GraphQL client:
  - urql setup (lighter than Apollo)
  - Queries ও mutations
  - Document caching
  - Subscriptions
  - Custom exchanges
  - Compare with Apollo Client
- **Tech:** urql

### Task 224 — tRPC Type-Safe API 🔴

- End-to-end type safety:
  - tRPC setup with Next.js
  - Router definition
  - Queries ও mutations
  - Input validation with Zod
  - Middleware (auth)
  - Error handling
  - Subscriptions
  - No API schema needed — types flow automatically
- **Tech:** tRPC, Zod
- **Focus:** tRPC — increasingly popular in React ecosystem

### Task 225 — WebSocket Integration 🟡

- Real-time data:
  - WebSocket client setup
  - Connection management (connect, disconnect, reconnect)
  - Message handling
  - Room/channel concept
  - Heartbeat/ping-pong
  - Combine with React Query (invalidate on WS message)
- **Tech:** Socket.io client বা native WebSocket

### Task 226 — Server-Sent Events (SSE) 🟡

- SSE implementation:
  - SSE endpoint in Next.js
  - EventSource client
  - Real-time notifications
  - Live activity feed
  - Reconnection handling
  - Compare: SSE vs WebSocket vs Polling
- **Focus:** Simpler real-time alternative to WebSocket

### Task 227 — Data Pagination Patterns 🟡

- All pagination types:
  1. Offset pagination (?page=2&limit=10)
  2. Cursor pagination (?cursor=abc123&limit=10)
  3. Infinite scroll (auto-load on scroll)
  4. "Load more" button
  5. Virtual scroll with pagination
  - When to use which pattern
  - Performance comparison
- **Focus:** Pagination — every app needs it

### Task 228 — Search Implementation 🟡

- Production search:
  - Debounced search input
  - Search API endpoint
  - Full-text search concept
  - Search result highlighting
  - Search suggestions/autocomplete
  - Recent searches (localStorage)
  - Search analytics tracking
  - No results handling
- **Focus:** Search UX best practices

### Task 229 — Data Transformation Layer 🟡

- API to UI data transformation:
  - API response → UI model mapping
  - Date formatting
  - Currency formatting
  - Address formatting
  - Aggregation (sum, average, group by)
  - Data normalization
  - Memoized transformations
- **Focus:** Clean data layer

### Task 230 — File Upload Architecture 🔴

- Production file upload:
  - Presigned URL upload (S3-like concept)
  - Chunked upload for large files
  - Upload progress tracking
  - Parallel uploads
  - Resume interrupted upload
  - Image compression before upload
  - File type validation (client + server)
- **Focus:** Production file upload patterns

### Task 231 — API Rate Limiting & Retry 🟡

- Resilient API client:
  - Retry with exponential backoff
  - Jitter for distributed retry
  - Circuit breaker pattern
  - Rate limit detection (429 status)
  - Queue requests when rate limited
  - Max retries configuration
  - Timeout handling
- **Focus:** Resilient network code

### Task 232 — Mock Service Worker (MSW) 🟡

- API mocking:
  - MSW setup for development
  - Mock handlers for all endpoints
  - Request interception
  - Dynamic mock responses
  - Error scenario simulation
  - Network delay simulation
  - MSW for testing
- **Tech:** MSW (Mock Service Worker)
- **Focus:** Frontend development without backend dependency

---

## Section C: Database & ORM for Frontend (Tasks 233-242)

### Task 233 — Prisma ORM Deep Dive 🟡

- Prisma complete:
  - Schema definition (models, relations)
  - One-to-one, one-to-many, many-to-many relations
  - Prisma Client queries (findMany, findUnique, create, update, delete)
  - Filtering, sorting, pagination
  - Nested queries (include, select)
  - Raw queries (when needed)
  - Prisma Migrate
- **Tech:** Prisma, PostgreSQL/SQLite

### Task 234 — Drizzle ORM Alternative 🟡

- Drizzle ORM:
  - Schema definition (TypeScript-first)
  - Queries with Drizzle
  - Relations
  - Migrations
  - Type safety comparison with Prisma
  - Bundle size comparison
  - SQL-like query builder
- **Tech:** Drizzle ORM
- **Focus:** Lighter alternative to Prisma

### Task 235 — Database Schema Design 🟡

- Design schemas for:
  1. Blog platform (users, posts, comments, tags)
  2. E-commerce (users, products, orders, reviews)
  3. Project management (users, projects, tasks, comments)
  - Entity relationship diagrams
  - Normalization
  - Indexes for performance
  - Soft delete pattern
- **Focus:** Database design — full-stack skill

### Task 236 — Supabase as Backend 🟡

- Supabase complete:
  - Database (PostgreSQL)
  - Auth (multiple providers)
  - Storage (file upload)
  - Real-time subscriptions
  - Row Level Security (RLS)
  - Edge Functions
  - Database functions/triggers
- **Tech:** Supabase, Next.js

### Task 237 — PlanetScale / Neon Serverless DB 🟡

- Serverless database:
  - Setup serverless PostgreSQL (Neon) বা MySQL (PlanetScale)
  - Connection with Prisma
  - Branching for development
  - Connection pooling
  - Cold start optimization
  - Compare with traditional database
- **Focus:** Serverless database — modern deployment

### Task 238 — Redis Caching Layer 🟡

- Redis for frontend:
  - Upstash Redis setup (serverless Redis)
  - Cache API responses
  - Rate limiting with Redis
  - Session storage
  - Real-time counters (views, likes)
  - Cache invalidation patterns
- **Tech:** Upstash Redis, Next.js

### Task 239 — IndexedDB for Offline Storage 🟡

- Client-side database:
  - IndexedDB API basics
  - Dexie.js wrapper
  - Store data offline
  - Sync with server when online
  - Transaction handling
  - Index for fast queries
  - Storage quota management
- **Tech:** Dexie.js (IndexedDB wrapper)

### Task 240 — Full-Text Search Implementation 🔴

- Search feature:
  - Basic: LIKE queries with Prisma
  - Better: PostgreSQL full-text search
  - Best: Algolia/Meilisearch integration
  - Search indexing
  - Faceted search (filters)
  - Typo tolerance
  - Search result ranking
- **Focus:** Search implementation options

### Task 241 — Database Performance Optimization 🔴

- Optimize queries:
  - N+1 problem identification ও fix
  - Eager loading vs lazy loading
  - Query analysis (EXPLAIN)
  - Index optimization
  - Connection pooling
  - Caching frequently accessed data
  - Pagination optimization
  - Database monitoring
- **Focus:** Database performance

### Task 242 — Data Seeding & Fixtures 🟢

- Test data management:
  - Prisma seed script
  - Faker.js for realistic data
  - Seed relationships (users with posts, posts with comments)
  - Deterministic seed (same data every time)
  - Environment-specific seeds
  - Large dataset seeding (1000+ records)
- **Tech:** Prisma, Faker.js

---

## Section D: Data Architecture Projects (Tasks 243-250)

### Task 243 — Real-time Dashboard 🔴

- Live data dashboard:
  - WebSocket for real-time data
  - React Query for initial load
  - Charts that update in real-time
  - Connection status indicator
  - Reconnection handling
  - Data aggregation (last 1h, 24h, 7d)
  - Export data
- **Tech:** Next.js, React Query, WebSocket, Recharts

### Task 244 — Inventory Management System 🔴

- Stock management:
  - Product CRUD with variants (size, color)
  - Stock levels tracking
  - Low stock alerts
  - Stock history log
  - Barcode/SKU management
  - Bulk import/export (CSV)
  - Dashboard with stock analytics
- **Tech:** Next.js, Prisma, TanStack Table

### Task 245 — Content Management System (CMS) 🔴

- Headless CMS:
  - Content types (Blog Post, Page, Product)
  - Content editor (rich text)
  - Media library (image upload + management)
  - Draft/Published status
  - Content versioning
  - API for content delivery
  - Preview mode
- **Tech:** Next.js, Prisma, Tiptap

### Task 246 — Analytics Dashboard 🔴

- Data visualization:
  - Page views chart (line)
  - Top pages table
  - User demographics (pie chart)
  - Real-time active users
  - Date range filter
  - Compare periods
  - Export reports
  - Responsive dashboard
- **Tech:** Next.js, Recharts, React Query

### Task 247 — Multi-Tenant Data Architecture ⚫

- SaaS data isolation:
  - Shared database with tenant column
  - Row Level Security per tenant
  - Tenant context in middleware
  - Data isolation verification
  - Tenant-specific settings
  - Cross-tenant data (shared resources)
- **Focus:** Enterprise data architecture

### Task 248 — Event Sourcing Concept Demo 🔴

- Event-based state:
  - Events as source of truth
  - Event store (append-only)
  - State projection from events
  - Event replay
  - Undo as event reversal
  - Activity timeline from events
- **Focus:** Advanced architecture pattern

### Task 249 — Data Pipeline Visualization ⚫

- ETL-like pipeline UI:
  - Visual pipeline builder (source → transform → destination)
  - Data preview at each step
  - Drag & drop pipeline steps
  - Run pipeline (simulated)
  - Pipeline history
  - Error handling per step
- **Tech:** Next.js, @dnd-kit, Zustand

### Task 250 — Complete Data Layer Architecture ⚫

- Design a complete data layer for a medium app:
  - API client with interceptors
  - React Query for server state
  - Zustand for client state
  - URL state for filters/pagination
  - Optimistic updates
  - Error handling
  - Cache invalidation
  - Offline support concept
  - Document architecture decisions
- **Focus:** Phase 05 graduation — data architecture mastery

---

## ✅ Phase 05 Completion Checklist

- [ ] State management decision tree confidently apply করতে পারো
- [ ] Zustand advanced patterns পারো
- [ ] React Query advanced (infinite, optimistic, cache) পারো
- [ ] tRPC বা GraphQL ব্যবহার করতে পারো
- [ ] Database design ও Prisma confidently পারো
- [ ] Real-time data handling (WebSocket/SSE) পারো
- [ ] Offline-first concepts বোঝো
- [ ] API architecture clean ভাবে design করতে পারো
- [ ] Caching strategies জানো
- [ ] Data layer architecture design করতে পারো

> ✅ Ready? → **Phase 06: Testing & Quality (`Phase-06-Testing.md`)** এ যাও
