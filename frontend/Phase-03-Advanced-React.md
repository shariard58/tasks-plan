# Phase 03 — Advanced React & Ecosystem (Tasks 101-150)

> এই Phase এ তুমি React ecosystem এর advanced tools, libraries ও patterns শিখবে।
> 🟢 Easy | 🟡 Medium | 🔴 Hard | ⚫ Expert

---

## Section A: React Router Advanced (Tasks 101-108)

### Task 101 — React Router Complete Setup 🟡

- React Router v6+ full setup:
  - Nested routes with `<Outlet>`
  - Layout routes (shared layout for route groups)
  - Index routes
  - Dynamic segments (`:id`, `:slug`)
  - 404 Not Found route
  - Programmatic navigation (`useNavigate`)
- **Tech:** react-router-dom v6+

### Task 102 — Protected Routes System 🟡

- Authentication-based routing:
  - Public routes (login, register, home)
  - Protected routes (dashboard, profile, settings)
  - Role-based routes (admin panel — admin only)
  - Redirect to login if not authenticated
  - Redirect to dashboard if already logged in
  - "Return to" URL after login
  - Loading state while checking auth
- **Tech:** React Router, Context API

### Task 103 — Route-Based Code Splitting 🟡

- Lazy loading per route:
  - `React.lazy` + `Suspense` per route
  - Route-level error boundaries
  - Prefetch routes on hover (link preloading)
  - Loading skeletons per route
  - Measure bundle size reduction
- **Focus:** Route-based performance optimization

### Task 104 — Breadcrumb Navigation System 🟡

- Dynamic breadcrumbs from route config:
  - Auto-generate breadcrumbs from nested routes
  - Custom breadcrumb labels
  - Clickable breadcrumb links
  - Dynamic segment resolution (ID → Name)
  - useMatches() hook ব্যবহার করো
- **Focus:** Complex route metadata

### Task 105 — Search Params State Management 🟡

- URL search params as state:
  - `useSearchParams` ব্যবহার করো
  - Filter state in URL (category, price range, sort)
  - Pagination in URL (?page=2&limit=10)
  - Shareable/Bookmarkable filtered URLs
  - Sync URL params with form state
  - Back/Forward button works with filters
- **Focus:** URL-driven state — production apps এ essential

### Task 106 — Animated Route Transitions 🟡

- Page transitions with Framer Motion:
  - Fade transition between routes
  - Slide left/right based on navigation direction
  - Shared layout animation (card → detail page)
  - Exit animation before route unmounts
  - `AnimatePresence` + `useLocation`
- **Tech:** React Router, Framer Motion

### Task 107 — Route Guards & Middleware Pattern 🟡

- Custom route middleware:
  - Auth guard (check authentication)
  - Permission guard (check specific permission)
  - Feature flag guard (show if feature enabled)
  - Confirmation guard (unsaved changes? confirm before leaving)
  - Compose multiple guards on a single route
- **Focus:** Guard/middleware pattern for routes

### Task 108 — Deep Linking & Navigation State 🟡

- Advanced navigation patterns:
  - Tabs that sync with URL (#tab=settings)
  - Modal routes (URL changes but modal opens over current page)
  - Scroll restoration on back navigation
  - Navigation state passing (useLocation state)
  - History stack management
- **Focus:** Production-grade navigation UX

---

## Section B: Framer Motion & Animation (Tasks 109-116)

### Task 109 — Framer Motion Fundamentals 🟢

- Basic animations:
  - `motion.div` with animate prop
  - Initial, animate, exit animations
  - Transition types (spring, tween, inertia)
  - Keyframes animation
  - Hover, tap, focus gestures
  - Variants for parent-child animation orchestration
- **Tech:** Framer Motion

### Task 110 — Layout Animations 🟡

- `layoutId` animations:
  - Card grid → expanded card animation
  - Tab indicator animation
  - Reorder list animation
  - Shared layout between routes
  - Notification stack animation
- **Focus:** Smooth layout transitions

### Task 111 — Scroll-Based Animations 🟡

- Scroll animations:
  - `useScroll` hook for scroll progress
  - Elements animate as they enter viewport
  - Parallax sections
  - Progress bar based on scroll
  - Scroll-linked opacity/scale changes
  - Sticky header with scroll-based style change
- **Tech:** Framer Motion

### Task 112 — Gesture Animations 🟡

- Interactive gesture-based UI:
  - Swipeable cards (Tinder-style)
  - Pinch to zoom
  - Drag to reorder
  - Pull to refresh animation
  - Long press actions
- **Tech:** Framer Motion

### Task 113 — Staggered & Orchestrated Animations 🟡

- Complex animation sequences:
  - Staggered list items entrance
  - Menu items cascade animation
  - Loading animation with sequenced dots
  - Hero section text + image orchestrated entrance
  - Accordion open/close with child stagger
- **Tech:** Framer Motion variants

### Task 114 — Page Transition Showcase 🟡

- 6 different page transition styles:
  1. Fade
  2. Slide (left/right)
  3. Scale (zoom in/out)
  4. Flip
  5. Curtain (wipe)
  6. Morph (shared element)
- Selectable transition style
- **Tech:** Framer Motion, React Router

### Task 115 — Animated Data Visualization 🔴

- Charts with animation:
  - Bar chart bars animate from zero
  - Line chart draws from left to right
  - Pie chart segments animate in
  - Number counter animation
  - Data change triggers smooth transition
- **Tech:** Framer Motion, SVG

### Task 116 — Micro-Interaction Collection 🟡

- 15+ micro-interactions:
  - Button press effect, Checkbox animation
  - Toggle switch, Loading button
  - Like heart animation, Bookmark animation
  - Copy to clipboard feedback
  - Delete with swipe, Notification pop
  - Tooltip appear, Dropdown open
  - Skeleton shimmer, Progress fill
  - Modal backdrop, Drawer slide
  - Snackbar slide up
- **Focus:** UI polish — senior devs এর কাজ এটাই

---

## Section C: Form Libraries & Validation (Tasks 117-124)

### Task 117 — React Hook Form Deep Dive 🟡

- Complete RHF mastery:
  - Register, handleSubmit, errors
  - Controller for custom components
  - useFieldArray for dynamic fields
  - Watch ও getValues
  - Form reset ও default values
  - Dirty ও touched state tracking
  - Performance comparison (controlled vs RHF)
- **Tech:** React Hook Form, TypeScript

### Task 118 — Zod Schema Validation 🟡

- Zod deep dive:
  - Basic schemas (string, number, boolean, date)
  - Object schemas with nested objects
  - Array schemas with min/max
  - Union ও Discriminated union schemas
  - Custom validation (refine, superRefine)
  - Transform (coerce string to number)
  - Schema composition (merge, extend, pick, omit)
  - Error messages customization
- **Tech:** Zod, TypeScript

### Task 119 — Complex Form: Job Application 🔴

- Multi-step job application form:
  - Step 1: Personal info + photo upload + crop
  - Step 2: Education (dynamic — add multiple)
  - Step 3: Experience (dynamic — add multiple with date ranges)
  - Step 4: Skills (tag input — type + select from suggestions)
  - Step 5: Cover letter (rich text editor)
  - Step 6: Review all + submit
  - Progress saved in localStorage (resume later)
  - PDF preview of application
- **Tech:** React Hook Form, Zod, Tiptap, React Cropper

### Task 120 — Form Builder Application 🔴

- Visual form builder:
  - Drag & drop form fields from palette
  - Configure each field (label, type, validation rules)
  - Preview mode
  - JSON schema export
  - Import JSON to render form
  - Generated form is fully functional with validation
- **Tech:** React Hook Form, @dnd-kit, Zod

### Task 121 — Dependent/Cascading Form Fields 🟡

- Forms with field dependencies:
  - Country → State → City (cascading dropdowns)
  - Product type → show relevant fields
  - Payment method → show relevant form section
  - Yes/No → show/hide follow-up questions
  - Conditional validation (if A, then B is required)
- **Tech:** React Hook Form, Zod

### Task 122 — Form Performance & UX Patterns 🟡

- Form UX best practices:
  - Inline validation (validate on blur, not on every keystroke)
  - Async validation (check username availability)
  - Optimistic submission
  - Form autosave (debounced)
  - Smart default values
  - Error summary with scroll-to-error
  - Accessible form errors (aria-describedby)
- **Focus:** Production-quality form UX

### Task 123 — File Upload System 🟡

- Complete file upload:
  - Drag & drop zone
  - Multiple file upload
  - File type restriction
  - File size limit
  - Image preview
  - Upload progress (simulated)
  - Retry failed uploads
  - Remove from queue
  - Chunked upload concept
- **Tech:** React Dropzone, React Hook Form

### Task 124 — Address Autocomplete Form 🟡

- Location-aware form:
  - Address input with Google Places-like autocomplete (use open API)
  - Map preview of selected address
  - Autofill city/state/country from address
  - Geolocation "Use my current location" button
  - Save multiple addresses
- **Tech:** React Hook Form, React Leaflet

---

## Section D: Data Fetching & Caching (Tasks 125-132)

### Task 125 — TanStack Query Fundamentals 🟡

- React Query basics:
  - `useQuery` for data fetching
  - Loading, error, success states
  - Query keys ও caching
  - Stale time ও cache time
  - Refetch on window focus
  - Manual refetch
  - Placeholder data ও initial data
- **Tech:** @tanstack/react-query

### Task 126 — TanStack Query Mutations 🟡

- CRUD with mutations:
  - `useMutation` for create, update, delete
  - Optimistic updates
  - Query invalidation after mutation
  - Rollback on error
  - Mutation loading/error states
  - Retry configuration
- **Tech:** @tanstack/react-query

### Task 127 — TanStack Query Advanced Patterns 🔴

- Advanced patterns:
  - Infinite queries (infinite scroll)
  - Paginated queries
  - Parallel queries
  - Dependent queries (query B depends on query A result)
  - Prefetching
  - Query cancellation
  - Offline support
  - Devtools usage
- **Tech:** @tanstack/react-query

### Task 128 — SWR vs React Query Comparison 🟡

- Same app with both libraries:
  - User list with SWR
  - User list with React Query
  - Compare: API, features, bundle size, DX
  - Revalidation strategies
  - Mutation patterns
  - Document findings
- **Tech:** SWR, @tanstack/react-query

### Task 129 — API Error Handling Patterns 🟡

- Comprehensive error handling:
  - HTTP error codes (400, 401, 403, 404, 500)
  - Network error (offline)
  - Timeout error
  - Rate limit error (429) with retry
  - Error transformation (API error → user-friendly message)
  - Global error handler
  - Toast notification for errors
- **Tech:** React Query, Axios

### Task 130 — Data Caching Strategy 🟡

- Caching deep dive:
  - In-memory cache (React Query)
  - Persistent cache (localStorage/IndexedDB)
  - Cache invalidation strategies
  - Cache time vs stale time
  - Cache warming (prefetch)
  - Cache normalization concept
  - Measure cache hit ratio
- **Focus:** Caching strategy — critical for performance

### Task 131 — Real-time Data with Polling & SSE 🟡

- Real-time data patterns:
  - Short polling (refetch every N seconds)
  - Long polling concept
  - Server-Sent Events (SSE)
  - React Query + polling
  - Dashboard with live-updating stats
  - Efficient re-render on data change
- **Focus:** Real-time data strategies

### Task 132 — GraphQL Client Basics 🟡

- GraphQL in React:
  - GraphQL query syntax
  - Apollo Client setup
  - useQuery for data fetching
  - useMutation for mutations
  - Variables in queries
  - Fragments for reusable field sets
  - Apollo cache
  - Compare REST vs GraphQL developer experience
- **Tech:** Apollo Client, GraphQL

---

## Section E: Advanced Third-Party Integrations (Tasks 133-150)

### Task 133 — TanStack Table Advanced 🔴

- Feature-rich data table:
  - Sorting (multi-column)
  - Global filter + column filters
  - Pagination (client-side ও server-side)
  - Column resizing
  - Column visibility toggle
  - Row expansion
  - Row selection
  - Pinned columns
  - Export CSV
  - Virtual rows (10k+ rows)
- **Tech:** @tanstack/react-table

### Task 134 — Zustand State Management 🟡

- Zustand deep dive:
  - Basic store creation
  - Multiple stores
  - Computed/derived state
  - Async actions
  - Middleware (persist, devtools, immer)
  - Slice pattern (organize large stores)
  - Subscribe to specific state changes
  - Compare with Context API
- **Tech:** Zustand

### Task 135 — Jotai Atomic State 🟡

- Jotai patterns:
  - Primitive atoms
  - Derived atoms (read-only)
  - Async atoms
  - Atom family (parameterized atoms)
  - Atom with storage (localStorage)
  - Compare Jotai vs Zustand vs Context
- **Tech:** Jotai

### Task 136 — Redux Toolkit Modern Redux 🟡

- RTK full setup:
  - configureStore
  - createSlice (reducer + actions)
  - createAsyncThunk
  - RTK Query (API management)
  - Entity adapter (normalized state)
  - Middleware
  - DevTools
  - Compare with Zustand
- **Tech:** Redux Toolkit
- **Focus:** Legacy React apps এ Redux আছে — জানা দরকার

### Task 137 — Motion One / Web Animations API 🟡

- Performance-focused animations:
  - Web Animations API (WAAPI) directly
  - Motion One library
  - Timeline animations
  - Spring animations
  - Scroll-triggered animations
  - Compare with Framer Motion (bundle size, performance)
- **Tech:** Motion One

### Task 138 — Radix UI Headless Components 🟡

- Radix UI primitives:
  - Dialog/Modal
  - Dropdown Menu
  - Popover
  - Tooltip
  - Select
  - Tabs
  - Accordion
  - Style with Tailwind
  - Accessible by default
- **Tech:** Radix UI, Tailwind CSS
- **Focus:** Headless component libraries — industry standard

### Task 139 — Shadcn/UI Component Setup 🟡

- shadcn/ui complete setup:
  - Install ও configure
  - Customize theme (colors, spacing, fonts)
  - Use 15+ components
  - Create custom variants
  - Dark mode implementation
  - Form integration with React Hook Form
- **Tech:** shadcn/ui, Tailwind, React Hook Form
- **Focus:** Modern component library approach (copy-paste, not dependency)

### Task 140 — React Spring Physics Animations 🟡

- Physics-based animations:
  - useSpring for single value
  - useSprings for multiple elements
  - useTrail for staggered
  - useTransition for mount/unmount
  - useChain for sequences
  - Drag with spring physics
- **Tech:** @react-spring/web

### Task 141 — React Virtual (Virtualization) 🔴

- Virtualized lists/grids:
  - Virtual list (10,000+ items)
  - Virtual grid
  - Variable size items
  - Infinite scroll with virtualization
  - Sticky headers in virtual list
  - Smooth scroll behavior
  - Performance comparison (virtual vs non-virtual)
- **Tech:** @tanstack/react-virtual

### Task 142 — PDF Viewer & Generator 🟡

- PDF handling:
  - View PDF in browser
  - Page navigation
  - Zoom controls
  - Generate PDF from React components
  - Invoice PDF generation
  - Resume PDF generation
  - Custom fonts in PDF
- **Tech:** @react-pdf/renderer, react-pdf

### Task 143 — Date Handling & Calendar Library 🟡

- Date management:
  - date-fns vs dayjs comparison
  - Relative time ("2 hours ago")
  - Date formatting per locale
  - Timezone handling
  - Date range operations
  - Calendar component with react-day-picker
  - Date input with masking
- **Tech:** date-fns, react-day-picker

### Task 144 — Internationalization (i18n) with i18next 🟡

- Full i18n setup:
  - i18next + react-i18next setup
  - Translation namespaces
  - Lazy loading translations
  - Pluralization
  - Interpolation
  - Context-based translations
  - Language detection
  - Translation management workflow
- **Tech:** i18next, react-i18next

### Task 145 — React Email Templates 🟡

- Email template builder:
  - React Email বা MJML ব্যবহার করো
  - Welcome email template
  - Order confirmation template
  - Password reset template
  - Newsletter template
  - Preview in browser
  - Responsive email output
- **Tech:** @react-email/components

### Task 146 — Real-time Collaboration Feature 🔴

- Collaborative editing concept:
  - Yjs বা Liveblocks setup
  - Real-time cursor positions
  - Collaborative text editing
  - Presence (who is online)
  - Conflict resolution demo
  - Shared state between clients
- **Tech:** Yjs অথবা Liveblocks
- **Focus:** Real-time collaboration — complex but impressive

### Task 147 — React Three Fiber (3D) Basics 🔴

- 3D in React:
  - Basic scene (camera, light, mesh)
  - 3D shapes (box, sphere, torus)
  - Textures ও materials
  - Mouse interaction (orbit controls, click)
  - Simple animation (rotation, movement)
  - 3D product viewer (rotate product)
- **Tech:** @react-three/fiber, @react-three/drei
- **Focus:** Unique portfolio piece — stands out

### Task 148 — AI Chat Interface 🟡

- ChatGPT-like UI:
  - Chat message list
  - Streaming text response (typewriter effect)
  - Code blocks with syntax highlighting in responses
  - Copy code button
  - Message editing
  - Conversation history sidebar
  - New chat button
  - Markdown rendering in responses
- **Tech:** React, react-markdown, react-syntax-highlighter

### Task 149 — Command Palette (⌘K) 🟡

- VS Code/Figma style command palette:
  - Keyboard shortcut to open (Ctrl+K)
  - Fuzzy search
  - Recent commands
  - Command categories
  - Keyboard navigation (up/down/enter)
  - Nested commands (Search files → select file → action)
  - Close on Escape or click outside
- **Tech:** React, cmdk library
- **Focus:** Power user feature — shows attention to detail

### Task 150 — Component Performance Benchmark 🔴

- Benchmark ও compare:
  - Context vs Zustand vs Jotai vs Redux (re-render count)
  - CSS Modules vs Tailwind vs styled-components (render time)
  - Controlled vs Uncontrolled forms (keystroke performance)
  - Virtual vs non-virtual lists (scroll performance)
  - React DevTools Profiler ব্যবহার করো
  - Document findings with screenshots
- **Focus:** Data-driven optimization decisions

---

## ✅ Phase 03 Completion Checklist

- [ ] React Router advanced patterns (guards, code splitting, URL state)
- [ ] Framer Motion animations confidently করতে পারো
- [ ] React Hook Form + Zod master level
- [ ] TanStack Query (React Query) ভালো পারো
- [ ] Multiple state management solutions জানো (Zustand, Jotai, Redux)
- [ ] Headless UI component pattern বোঝো (Radix UI)
- [ ] Data table, virtualization পারো
- [ ] Third-party library integration confidently করতে পারো
- [ ] Performance measure ও optimize করতে পারো

> ✅ Ready? → **Phase 04: Next.js Mastery (`Phase-04-NextJS.md`)** এ যাও
