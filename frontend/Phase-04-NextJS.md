# Phase 04 — Next.js Complete Mastery (Tasks 151-200)

> Next.js হলো React এর production framework। এই Phase এ তুমি Next.js App Router সম্পূর্ণ শিখবে।
> 🟢 Easy | 🟡 Medium | 🔴 Hard | ⚫ Expert

---

## Section A: Next.js Fundamentals (Tasks 151-162)

### Task 151 — Next.js App Router Setup & Routing 🟢

- App Router project setup:
  - File-based routing (page.tsx, layout.tsx)
  - Nested layouts
  - Dynamic routes `[id]`, `[...slug]`, `[[...slug]]`
  - Route groups `(marketing)`, `(dashboard)`
  - Loading.tsx, error.tsx, not-found.tsx per route
  - Template vs Layout difference
- **Focus:** App Router file conventions

### Task 152 — Server Components vs Client Components 🟡

- Deep understanding project:
  - Server Component (default) — when/why to use
  - Client Component (`"use client"`) — when/why to use
  - Serialization boundary (what can/cannot pass from server to client)
  - Server Component importing Client Component (OK)
  - Client Component importing Server Component (children pattern)
  - Build a page mixing both optimally
- **Focus:** Server/Client boundary — Next.js এর most important concept

### Task 153 — Data Fetching Patterns in Next.js 🟡

- All data fetching approaches:
  - Server Component async fetch (no useEffect needed)
  - fetch() with caching options (force-cache, no-store, revalidate)
  - generateStaticParams for static pages
  - Dynamic rendering vs static rendering
  - Parallel data fetching (Promise.all)
  - Sequential data fetching (waterfall — when necessary)
- **Focus:** Next.js data fetching mental model

### Task 154 — Next.js Metadata & SEO 🟡

- Complete SEO setup:
  - Static metadata (export const metadata)
  - Dynamic metadata (generateMetadata function)
  - Open Graph images
  - Twitter cards
  - JSON-LD structured data
  - Sitemap generation (sitemap.ts)
  - Robots.txt (robots.ts)
  - Canonical URLs
- **Focus:** SEO — business-critical skill

### Task 155 — Static Site Generation (SSG) Blog 🟡

- Markdown blog with SSG:
  - MDX support setup
  - generateStaticParams for all posts
  - Blog listing page (statically generated)
  - Individual post pages (statically generated)
  - Table of contents auto-generation
  - Reading time calculation
  - Code blocks with syntax highlighting
  - Image optimization with next/image
- **Tech:** Next.js, MDX, rehype/remark plugins

### Task 156 — Server-Side Rendering (SSR) Dashboard 🟡

- Dynamic SSR pages:
  - Dashboard that always shows fresh data (no-store)
  - User-specific content (cookies/headers based)
  - Dynamic rendering conditions
  - Headers ও cookies access in Server Components
  - Streaming with loading.tsx
  - Error handling with error.tsx
- **Focus:** When SSR is necessary vs SSG

### Task 157 — ISR (Incremental Static Regeneration) 🟡

- ISR patterns:
  - Time-based revalidation (`revalidate: 60`)
  - On-demand revalidation (`revalidatePath`, `revalidateTag`)
  - Tag-based cache invalidation
  - ISR for product pages (stale-while-revalidate)
  - ISR for blog posts
  - Monitor revalidation with logs
- **Focus:** ISR — SSG ও SSR এর best of both worlds

### Task 158 — Next.js Image Optimization 🟢

- next/image deep dive:
  - Local images (automatic width/height)
  - Remote images (domains config)
  - Responsive images (sizes prop)
  - Fill mode for dynamic containers
  - Placeholder blur
  - Priority for LCP images
  - Art direction with `<picture>` pattern
  - Image loader customization
- **Focus:** Image optimization — huge performance impact

### Task 159 — Next.js Font Optimization 🟢

- next/font:
  - Google Fonts (next/font/google)
  - Local fonts (next/font/local)
  - Variable fonts
  - Font subsetting
  - Font display strategies
  - Apply fonts to specific components
  - Multiple font families
- **Focus:** Font optimization — CLS improvement

### Task 160 — Next.js Link & Navigation 🟡

- Navigation patterns:
  - `<Link>` component (prefetching)
  - `useRouter` for programmatic navigation
  - `usePathname`, `useSearchParams`, `useParams`
  - Active link styling
  - Scroll restoration
  - Shallow routing
  - Parallel prefetching
  - Navigation events (loading indicators)
- **Focus:** Next.js navigation API

### Task 161 — Environment Variables & Configuration 🟢

- Config management:
  - `.env`, `.env.local`, `.env.development`, `.env.production`
  - `NEXT_PUBLIC_` prefix for client-side variables
  - Server-only env vars
  - Runtime configuration
  - next.config.js deep dive (redirects, rewrites, headers)
  - Security: never expose server secrets to client
- **Focus:** Configuration management

### Task 162 — Next.js Project Structure 🟡

- Production-grade structure:
  ```
  src/
  ├── app/           (routes)
  ├── components/    (shared UI components)
  ├── features/      (feature modules)
  ├── hooks/         (custom hooks)
  ├── lib/           (utilities, configs)
  ├── services/      (API services)
  ├── stores/        (state management)
  ├── types/         (TypeScript types)
  └── styles/        (global styles)
  ```

  - Barrel exports
  - Feature-based organization
  - Shared vs feature-specific components
- **Focus:** Scalable project structure

---

## Section B: Next.js API & Server Actions (Tasks 163-174)

### Task 163 — Route Handlers (API Routes) 🟡

- API endpoints:
  - GET, POST, PUT, PATCH, DELETE handlers
  - Request parsing (body, params, searchParams)
  - Response helpers (NextResponse.json, redirect)
  - Dynamic route handlers `[id]/route.ts`
  - CORS handling
  - Rate limiting (basic implementation)
- **Focus:** Full-stack API capability

### Task 164 — Server Actions Fundamentals 🟡

- Server Actions:
  - Form submission with Server Action
  - `"use server"` directive
  - Action in separate file vs inline
  - `useFormStatus` for loading state
  - `useActionState` for action result
  - Progressive enhancement (works without JS)
- **Focus:** Server Actions — Next.js recommended mutation pattern

### Task 165 — Server Actions CRUD Application 🟡

- Notes app with Server Actions:
  - Create note (Server Action + form)
  - Read notes (Server Component)
  - Update note (Server Action)
  - Delete note (Server Action with confirmation)
  - Optimistic updates with `useOptimistic`
  - Error handling in Server Actions
  - Revalidate after mutation
- **Focus:** Real CRUD with Server Actions

### Task 166 — Server Actions File Upload 🟡

- File upload with Server Actions:
  - FormData with file
  - File validation (type, size) server-side
  - Save to local filesystem (development)
  - Upload progress indication
  - Multiple file upload
  - Image processing (resize) server-side
- **Focus:** File handling server-side

### Task 167 — API Authentication & Security 🔴

- Secure API routes:
  - JWT token validation
  - API key authentication
  - Rate limiting per IP
  - Input validation (Zod)
  - CORS configuration
  - Helmet-like security headers
  - CSRF protection concept
  - Request logging
- **Focus:** API security — OWASP awareness

### Task 168 — Webhooks Handler 🟡

- Webhook implementation:
  - Receive webhook from external service (Stripe-like)
  - Verify webhook signature
  - Process webhook payload
  - Idempotency handling
  - Queue concept for heavy processing
  - Error handling ও retry logic
- **Focus:** Webhook patterns — real-world integration

### Task 169 — Server Actions with Optimistic UI 🟡

- Optimistic update patterns:
  - `useOptimistic` hook
  - Like button (instant feedback, revert on error)
  - Add to cart (instant add, revert if fails)
  - Delete item (instant remove, restore on error)
  - Comment add (instant show, remove on error)
  - Rollback animation on error
- **Focus:** Optimistic UI — production app must-have

### Task 170 — Streaming & Progressive Loading 🔴

- Streaming SSR:
  - React Suspense boundaries
  - Streaming multiple sections independently
  - `loading.tsx` for route-level streaming
  - Parallel data loading with Suspense
  - Skeleton UIs during streaming
  - Performance measurement (TTFB, FCP, LCP)
- **Focus:** Streaming — Next.js performance superpower

### Task 171 — Edge Runtime API Routes 🟡

- Edge runtime:
  - `export const runtime = 'edge'`
  - Edge vs Node.js runtime differences
  - Limitations (no Node.js APIs)
  - When to use Edge (geolocation, A/B testing)
  - Edge-compatible data fetching
  - Performance comparison (Edge vs Node.js)
- **Focus:** Edge computing concept

### Task 172 — Next.js + Database Integration 🟡

- Database connection:
  - Prisma ORM setup with Next.js
  - Database schema design
  - CRUD operations with Prisma
  - Server Components direct database access
  - Connection pooling
  - Database seeding
  - Type-safe queries
- **Tech:** Prisma, SQLite (development) / PostgreSQL
- **Focus:** Full-stack capability

### Task 173 — Email Sending from Server Actions 🟡

- Transactional email:
  - Resend বা Nodemailer setup
  - Welcome email on registration
  - Contact form email notification
  - Email templates (React Email)
  - Async email sending
  - Error handling
- **Tech:** Resend / Nodemailer, React Email

### Task 174 — Cron Jobs & Scheduled Tasks 🟡

- Scheduled operations:
  - Vercel Cron Jobs setup
  - Daily report generation
  - Cleanup old data
  - Send reminder emails
  - Health check endpoint
  - Logging scheduled task execution
- **Focus:** Background tasks in serverless

---

## Section C: Next.js Advanced Features (Tasks 175-188)

### Task 175 — Middleware Deep Dive 🔴

- Next.js Middleware:
  - Authentication redirect
  - Geo-based content (country detection)
  - A/B testing (cookie-based variant assignment)
  - Bot detection
  - Request/Response header modification
  - URL rewriting
  - Chaining middleware logic
  - Matcher configuration
- **Focus:** Middleware — edge-level request handling

### Task 176 — Parallel Routes 🔴

- Parallel route patterns:
  - Dashboard with independent sections (`@analytics`, `@notifications`)
  - Each section loads independently
  - Different loading/error states per slot
  - Conditional rendering based on auth
  - Default.tsx for unmatched parallel routes
- **Focus:** Parallel routes — advanced layout composition

### Task 177 — Intercepting Routes 🔴

- Route interception:
  - Photo gallery modal (Instagram-style)
  - `(.)`, `(..)`, `(...)`, `(..)(..)` conventions
  - Modal on feed, full page on direct URL
  - Login modal intercepting from any page
  - Close modal → return to previous page
- **Focus:** Intercepting routes — advanced UX pattern

### Task 178 — Dynamic OG Image Generation 🟡

- `next/og` ImageResponse:
  - Blog post OG image (title, author, date)
  - Product OG image (photo, price, name)
  - User profile OG image
  - Custom fonts in OG image
  - Emoji support
  - Template system (multiple OG designs)
- **Focus:** Dynamic social preview images

### Task 179 — Internationalization (i18n) in Next.js 🔴

- Multi-language Next.js app:
  - URL-based locale (`/en/about`, `/bn/about`)
  - Middleware for locale detection
  - next-intl setup
  - Static generation for each locale
  - Language switcher
  - RTL support
  - Locale-specific formatting (dates, numbers, currency)
  - SEO per locale (hreflang tags)
- **Tech:** next-intl

### Task 180 — Next.js + Authentication (NextAuth/Auth.js) 🔴

- Complete auth system:
  - Google OAuth provider
  - GitHub OAuth provider
  - Credentials provider (email/password)
  - Session management (JWT vs Database)
  - Middleware auth protection
  - Server Component session access
  - Client Component session access
  - Role-based access control
  - Custom login/signup pages
- **Tech:** NextAuth.js / Auth.js

### Task 181 — Next.js + Supabase 🟡

- Supabase integration:
  - Supabase project setup
  - Supabase Auth (magic link, OAuth)
  - Database operations (CRUD)
  - Real-time subscriptions
  - Row Level Security (RLS)
  - File storage (image upload)
  - Server-side Supabase client
- **Tech:** Supabase, Next.js

### Task 182 — Next.js + Firebase 🟡

- Firebase integration:
  - Firebase Auth (Google, Email/Password)
  - Firestore CRUD
  - Real-time listeners
  - Firebase Storage (file upload)
  - Server-side Firebase Admin SDK
  - Client-side Firebase SDK
  - Security rules
- **Tech:** Firebase, Next.js

### Task 183 — Next.js Caching Deep Dive ⚫

- Caching layers:
  - Request Memoization (same request in single render)
  - Data Cache (fetch cache)
  - Full Route Cache (static rendering)
  - Router Cache (client-side)
  - Cache invalidation strategies
  - `unstable_cache` for non-fetch
  - Cache debugging (how to verify cache behavior)
- **Focus:** Next.js caching — most confusing topic, master it

### Task 184 — Error Handling Architecture 🟡

- Production error handling:
  - error.tsx per route segment
  - global-error.tsx for root layout errors
  - not-found.tsx customization
  - Error recovery (reset function)
  - Error reporting service integration (Sentry mock)
  - Graceful degradation
  - User-friendly error messages
- **Focus:** Production error handling

### Task 185 — Next.js Security Best Practices 🔴

- Security checklist implementation:
  - Content Security Policy (CSP) headers
  - CSRF protection
  - XSS prevention (sanitization)
  - SQL injection prevention (Prisma parameterized queries)
  - Rate limiting
  - Input validation (all server actions)
  - Secure cookies (httpOnly, secure, sameSite)
  - Environment variable security
  - Dependency audit
- **Focus:** Security — essential for production

### Task 186 — Progressive Web App (PWA) with Next.js 🟡

- PWA features:
  - Service Worker setup
  - App manifest
  - Offline fallback page
  - Cache strategies (cache-first, network-first)
  - Push notifications (concept)
  - Install prompt
  - App-like experience
- **Tech:** next-pwa, Next.js

### Task 187 — Next.js Analytics & Monitoring 🟡

- Observability:
  - Vercel Analytics integration
  - Web Vitals monitoring (LCP, FID, CLS, TTFB)
  - Custom event tracking
  - Error monitoring (Sentry integration)
  - Performance monitoring
  - User analytics dashboard
- **Focus:** Production monitoring

### Task 188 — Next.js Deployment Deep Dive 🟡

- Deployment strategies:
  - Vercel deployment
  - Self-hosted (Node.js server)
  - Docker deployment
  - Static export (output: 'export')
  - Environment-specific builds
  - Preview deployments (PR previews)
  - Rollback strategy
  - CDN configuration
- **Focus:** Deployment knowledge

---

## Section D: Next.js Full Projects (Tasks 189-200)

### Task 189 — Personal Blog Platform 🟡

- MDX-based blog:
  - Homepage with featured posts
  - Blog listing with pagination
  - Post page with TOC, reading time, share buttons
  - Category ও tag pages
  - RSS feed
  - Sitemap
  - Dark mode
  - SEO optimized
- **Tech:** Next.js, MDX, Tailwind

### Task 190 — SaaS Landing Page 🟡

- Marketing website:
  - Hero section with CTA
  - Features section with icons
  - Pricing page (monthly/yearly toggle)
  - Testimonials
  - FAQ (Accordion)
  - Contact form (Server Action)
  - Blog section
  - SEO + OG images
- **Tech:** Next.js, Framer Motion, Tailwind

### Task 191 — E-commerce Storefront 🔴

- Product storefront:
  - Product listing (SSG with ISR)
  - Product detail page (dynamic)
  - Category filtering (URL search params)
  - Shopping cart (Zustand + persist)
  - Checkout form (multi-step)
  - Order confirmation page
  - Product search with autocomplete
- **Tech:** Next.js, Zustand, React Hook Form

### Task 192 — Dashboard Application 🔴

- Admin dashboard:
  - Auth (NextAuth)
  - Dashboard home (charts, stats)
  - User management (CRUD table)
  - Content management
  - Settings page
  - Profile page
  - Role-based sidebar
  - Responsive layout
- **Tech:** Next.js, NextAuth, Prisma, Recharts, TanStack Table

### Task 193 — Real-time Chat Application 🔴

- Chat app:
  - User authentication
  - Chat rooms / Direct messages
  - Real-time messaging (Supabase Realtime বা Pusher)
  - Online/Offline status
  - Typing indicator
  - Read receipts
  - File/Image sharing
  - Message search
- **Tech:** Next.js, Supabase, Zustand

### Task 194 — Job Board Platform 🔴

- Job listing site:
  - Job listing with filters (location, type, salary range)
  - Job detail page (SSR)
  - Post job form (auth required)
  - Apply to job
  - Saved jobs
  - Company profiles
  - Search with autocomplete
  - Email notifications
- **Tech:** Next.js, Prisma, NextAuth

### Task 195 — Social Bookmarking App (Reddit-like) 🔴

- Link sharing platform:
  - Post links with title, description, tags
  - Upvote/Downvote system
  - Comments with nested replies
  - User profiles
  - Subreddit-like communities
  - Sort by: Hot, New, Top
  - Infinite scroll
- **Tech:** Next.js, Prisma, NextAuth

### Task 196 — Project Management Tool 🔴

- Jira-lite:
  - Projects (CRUD)
  - Kanban board per project
  - Task CRUD with assignee, due date, priority, labels
  - Sprint planning concept
  - Activity log
  - Team members management
  - Dashboard with project statistics
- **Tech:** Next.js, Prisma, @dnd-kit, Zustand

### Task 197 — Learning Management System (LMS) 🔴

- Course platform:
  - Course listing
  - Course detail with curriculum
  - Video player page
  - Progress tracking
  - Quiz/Assessment per module
  - Certificate generation
  - Student dashboard
  - Instructor dashboard
- **Tech:** Next.js, Prisma, React Player

### Task 198 — AI-Powered Note App ⚫

- Notion-lite with AI:
  - Block-based editor (headings, paragraphs, lists, code)
  - Drag & drop blocks
  - AI text generation (OpenAI API integration)
  - AI summarization
  - Collaborative editing concept
  - File tree navigation
  - Search across notes
  - Export as Markdown/PDF
- **Tech:** Next.js, Tiptap, OpenAI API, Prisma

### Task 199 — Multi-Tenant SaaS Starter ⚫

- SaaS boilerplate:
  - Organization/Team management
  - Invite team members
  - Role-based access (Owner, Admin, Member)
  - Organization settings
  - Billing page UI (Stripe integration concept)
  - Subdomain per organization concept
  - Shared components library
- **Tech:** Next.js, Prisma, NextAuth, Zustand

### Task 200 — Next.js Performance Audit & Optimization ⚫

- Take Task 192 dashboard ও optimize করো:
  - Lighthouse score 90+ (all categories)
  - Bundle analysis ও code splitting
  - Image optimization audit
  - Font optimization
  - Third-party script optimization
  - Core Web Vitals measurement
  - Caching strategy optimization
  - Database query optimization
  - Document before/after metrics
- **Focus:** Phase 04 graduation — performance mastery

---

## ✅ Phase 04 Completion Checklist

- [ ] Next.js App Router confidently ব্যবহার করতে পারো
- [ ] Server Components vs Client Components clearly বোঝো
- [ ] SSG, SSR, ISR কখন কোনটা ব্যবহার করবে জানো
- [ ] Server Actions ভালো পারো
- [ ] Middleware ব্যবহার করতে পারো
- [ ] Parallel ও Intercepting routes বোঝো
- [ ] NextAuth দিয়ে authentication implement করতে পারো
- [ ] Prisma দিয়ে database integration পারো
- [ ] Next.js caching strategy বোঝো
- [ ] Full-stack Next.js app deploy করতে পারো
- [ ] 3+ complete Next.js projects portfolio তে আছে

> ✅ Ready? → **Phase 05: State Management & Data (`Phase-05-State-And-Data.md`)** এ যাও
