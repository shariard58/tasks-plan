# Phase 07 — Performance & Optimization (Tasks 301-350)

> Performance হলো senior developer এর identity। এই Phase শেষে তুমি performance expert হবে।
> 🟢 Easy | 🟡 Medium | 🔴 Hard | ⚫ Expert

---

## Section A: Web Performance Fundamentals (Tasks 301-314)

### Task 301 — Core Web Vitals Deep Dive 🟡

- Understand ও measure:
  - LCP (Largest Contentful Paint) — main content কত fast load হয়
  - INP (Interaction to Next Paint) — interaction কত responsive
  - CLS (Cumulative Layout Shift) — visual stability
  - TTFB (Time to First Byte) — server response time
  - FCP (First Contentful Paint)
  - Measure with: Lighthouse, PageSpeed Insights, Chrome DevTools
  - Build a page → optimize each metric → document before/after
- **Focus:** Core Web Vitals — Google ranking factor

### Task 302 — Chrome DevTools Performance Tab 🟡

- Performance profiling:
  - Record performance trace
  - Analyze flame chart
  - Identify long tasks
  - Main thread blocking
  - Layout thrashing detection
  - Memory timeline
  - Network waterfall analysis
  - Coverage tab (unused CSS/JS)
- **Focus:** Performance debugging tool mastery

### Task 303 — Network Performance Optimization 🟡

- Network optimization:
  - Resource hints: preload, prefetch, preconnect, dns-prefetch
  - HTTP/2 multiplexing benefits
  - Compression (gzip, brotli)
  - CDN usage
  - Service Worker caching strategies
  - Stale-while-revalidate pattern
  - Critical path optimization
- **Focus:** Network-level optimization

### Task 304 — Image Optimization Complete Guide 🟡

- Image performance:
  - Format selection (WebP, AVIF, PNG, JPEG, SVG)
  - Responsive images (srcset, sizes)
  - next/image optimization
  - Lazy loading (native ও Intersection Observer)
  - LQIP (Low Quality Image Placeholder)
  - BlurHash placeholders
  - Art direction with `<picture>`
  - Image CDN (Cloudinary, imgproxy)
  - Measure impact on LCP
- **Focus:** Images — usually biggest performance bottleneck

### Task 305 — Font Performance 🟡

- Font optimization:
  - Font loading strategies (swap, optional, fallback)
  - Font subsetting (only needed characters)
  - Variable fonts (one file, multiple weights)
  - System font stack as fallback
  - next/font optimization
  - FOIT vs FOUT
  - Font preloading
  - Measure CLS impact from fonts
- **Focus:** Font-related layout shifts

### Task 306 — JavaScript Bundle Optimization 🔴

- Bundle size reduction:
  - Bundle analyzer (webpack-bundle-analyzer, vite plugin)
  - Tree shaking (how it works, how to enable)
  - Code splitting (dynamic imports)
  - Route-based splitting
  - Component-based splitting (React.lazy)
  - Library alternatives (date-fns vs moment, lodash-es vs lodash)
  - Dead code elimination
  - Side effects in package.json
  - Measure: before/after bundle sizes
- **Focus:** Bundle size — directly impacts load time

### Task 307 — CSS Performance 🟡

- CSS optimization:
  - Critical CSS extraction
  - CSS code splitting
  - Remove unused CSS (PurgeCSS concept)
  - CSS containment
  - content-visibility: auto
  - will-change property (use sparingly)
  - Composite layers
  - Avoid expensive selectors
  - CSS-in-JS performance implications
- **Focus:** CSS rendering performance

### Task 308 — Rendering Performance 🔴

- Rendering optimization:
  - Reflow vs Repaint
  - Layout thrashing (read-write-read-write pattern)
  - requestAnimationFrame for visual updates
  - CSS transforms vs position changes
  - Composite layers (GPU acceleration)
  - Reduce DOM size
  - Avoid forced synchronous layouts
  - Chrome Performance tab → identify rendering issues
- **Focus:** Rendering pipeline understanding

### Task 309 — Memory Management 🔴

- Memory optimization:
  - Identify memory leaks (DevTools Memory tab)
  - Common leak patterns: event listeners, timers, closures, DOM references
  - WeakMap/WeakSet for cache
  - Garbage collection understanding
  - Memory profiling (heap snapshots)
  - Fix memory leaks in React (cleanup in useEffect)
  - Memory budget concept
- **Focus:** Memory leaks — production app killer

### Task 310 — Third-Party Script Management 🟡

- Third-party optimization:
  - Identify slow third-party scripts
  - Defer non-critical scripts
  - next/script strategies (beforeInteractive, afterInteractive, lazyOnload)
  - Self-host critical third-party resources
  - Facade pattern (YouTube embed → image until click)
  - Third-party script impact measurement
  - Content Security Policy
- **Focus:** Third-party scripts — often biggest performance hit

### Task 311 — Service Worker Caching Strategies 🔴

- Caching patterns:
  - Cache First (static assets)
  - Network First (API data)
  - Stale While Revalidate (balance)
  - Cache Only (offline-first)
  - Network Only (always fresh)
  - Cache with background update
  - Cache versioning ও cleanup
  - Workbox library usage
- **Tech:** Workbox / Service Worker API

### Task 312 — Prefetching & Preloading Strategies 🟡

- Resource loading optimization:
  - Route prefetching (Next.js Link)
  - Data prefetching (React Query prefetchQuery)
  - Hover-based prefetch
  - Viewport-based prefetch (Intersection Observer)
  - Module preloading
  - DNS prefetch for external domains
  - Measure prefetch effectiveness
- **Focus:** Perceived performance improvement

### Task 313 — Animation Performance 🟡

- Smooth animations:
  - 60fps target
  - CSS animations vs JS animations
  - Transform ও opacity (GPU-accelerated)
  - requestAnimationFrame usage
  - will-change property
  - Reduce animation during reduced-motion preference
  - Performance monitor in DevTools
  - Jank detection ও fix
- **Focus:** Animation without janks

### Task 314 — Performance Budget System 🟡

- Performance budgets:
  - Define budgets (bundle size, load time, LCP, CLS)
  - bundlesize / size-limit configuration
  - Lighthouse CI budget configuration
  - CI check for budget violations
  - Budget monitoring dashboard
  - Alert when budget exceeded
  - Quarterly budget review process
- **Focus:** Proactive performance management

---

## Section B: React Performance (Tasks 315-328)

### Task 315 — React Profiler Deep Dive 🟡

- Profiling React apps:
  - React DevTools Profiler tab
  - Flame chart reading
  - Ranked chart (slowest components)
  - Why did this render? (highlight updates)
  - Record interactions
  - Identify unnecessary re-renders
  - `<Profiler>` component for programmatic profiling
- **Focus:** React-specific performance debugging

### Task 316 — Preventing Unnecessary Re-Renders 🟡

- Re-render optimization:
  - React.memo (when ও when not)
  - useMemo for expensive computations
  - useCallback for callback stability
  - Context splitting (avoid provider re-renders)
  - Move state down (local vs global)
  - Content lift up (children as props)
  - Measure: render count before/after
- **Focus:** Most common React performance issue

### Task 317 — Virtualization for Large Lists 🔴

- Virtual scrolling:
  - @tanstack/react-virtual setup
  - Virtual list (10,000+ items)
  - Virtual grid
  - Variable-size items
  - Infinite scroll with virtualization
  - Sticky header in virtual list
  - Search/Filter with virtual list
  - Measure: DOM node count, scroll fps
- **Tech:** @tanstack/react-virtual

### Task 318 — Code Splitting Strategies 🟡

- Splitting approaches:
  - Route-based splitting (React.lazy per route)
  - Component-based splitting (heavy components)
  - Library-based splitting (import on demand)
  - Vendor chunk splitting
  - Prefetch splitted chunks
  - Loading states for lazy components
  - Named chunks for debugging
  - Measure: initial bundle reduction
- **Focus:** Load only what you need

### Task 319 — React Server Components Performance 🔴

- RSC performance benefits:
  - Zero client-side JS for server components
  - Streaming SSR benefits
  - Selective hydration
  - Server-side data fetching (no client waterfalls)
  - Component-level caching
  - Measure: JS bundle reduction with RSC
  - Compare: CSR vs SSR vs RSC
- **Focus:** RSC performance model

### Task 320 — Concurrent Features Performance 🔴

- React concurrent performance:
  - useTransition for non-urgent updates
  - useDeferredValue for expensive renders
  - Prioritized rendering
  - Input responsiveness during heavy computation
  - List filtering without blocking input
  - Transition pending state UI
  - Measure: INP improvement
- **Focus:** Concurrent React for better INP

### Task 321 — Memoization Strategies 🟡

- Memoization deep dive:
  - useMemo — when it helps, when it hurts
  - useCallback — stable function references
  - React.memo — prevent child re-renders
  - Memoized selectors (Zustand, Redux)
  - React Query `select` for derived data
  - Custom memoize function
  - Memoization cost analysis
- **Focus:** Strategic memoization

### Task 322 — Context Performance Optimization 🟡

- Context without performance issues:
  - Split contexts by update frequency
  - Memoize context value
  - Separate state ও dispatch contexts
  - Context selector pattern (use-context-selector)
  - When to move from Context to Zustand
  - Measure: re-render reduction
- **Focus:** Context at scale

### Task 323 — Form Performance 🟡

- High-performance forms:
  - Controlled vs Uncontrolled (performance)
  - React Hook Form (minimizes re-renders)
  - Debounced validation
  - Lazy field validation
  - Large form optimization (100+ fields)
  - Dynamic form performance
  - Measure: keystroke delay
- **Focus:** Form responsiveness

### Task 324 — Image Gallery Performance 🟡

- Optimized image experience:
  - Lazy loading images
  - Progressive loading (blur → full)
  - Responsive images (serve right size)
  - Image decoding (decode() API)
  - Virtual grid for gallery
  - Preload next images
  - Memory management for image-heavy pages
- **Focus:** Image-heavy page optimization

### Task 325 — SSR Performance Optimization 🔴

- Server-side performance:
  - Reduce server response time
  - Streaming for faster TTFB
  - Cache server-rendered pages
  - Reduce hydration cost
  - Partial hydration concept
  - React Server Components (zero hydration)
  - CDN caching for static pages
  - Measure: TTFB, FCP improvement
- **Focus:** Server-side rendering performance

### Task 326 — Webpack/Vite Performance Configuration 🔴

- Build optimization:
  - Minification (terser, esbuild)
  - Compression (gzip, brotli)
  - Source map strategy (production)
  - Cache busting (content hash)
  - Module concatenation
  - Tree shaking optimization
  - Build speed optimization
  - Analyze build output
- **Focus:** Build-time optimization

### Task 327 — Database Query Performance (Frontend Impact) 🟡

- Data loading performance:
  - Avoid N+1 queries (select + include properly)
  - Pagination vs loading all
  - Limit selected fields
  - Database indexes impact
  - Cache frequently accessed data
  - Optimistic UI to mask latency
  - Parallel data fetching
- **Focus:** Data loading impact on frontend

### Task 328 — Performance Monitoring Dashboard ⚫

- Build a performance monitoring tool:
  - Collect Web Vitals (LCP, INP, CLS)
  - Send metrics to API
  - Dashboard showing metrics over time
  - Percentile charts (p50, p75, p90)
  - Alert on performance regression
  - Compare across pages
  - Real user monitoring (RUM) concept
- **Focus:** Production performance monitoring

---

## Section C: Accessibility Performance (Tasks 329-338)

### Task 329 — WCAG 2.1 Compliance Audit 🟡

- Accessibility checklist:
  - Perceivable (text alternatives, captions, contrast)
  - Operable (keyboard, enough time, no seizures)
  - Understandable (readable, predictable, error prevention)
  - Robust (compatible with assistive tech)
  - Audit an existing project
  - Fix all issues
  - Document findings
- **Focus:** WCAG compliance — legal requirement in many markets

### Task 330 — Keyboard Navigation System 🟡

- Complete keyboard support:
  - Tab order management
  - Focus trap in modals
  - Arrow key navigation in lists/menus
  - Escape to close modals/dropdowns
  - Enter/Space for buttons
  - Skip navigation link
  - Focus visible styles
  - Roving tabindex pattern
- **Focus:** Keyboard navigation

### Task 331 — Screen Reader Optimization 🟡

- Screen reader friendly:
  - Proper ARIA roles
  - aria-label, aria-describedby, aria-live
  - Announce dynamic content changes
  - Form error announcements
  - Page title updates on navigation
  - Hidden text for screen readers only
  - Test with NVDA/VoiceOver
- **Focus:** Screen reader experience

### Task 332 — Color & Contrast System 🟡

- Accessible color system:
  - WCAG AA contrast ratios (4.5:1 text, 3:1 large text)
  - Check all color combinations
  - Color blindness simulation
  - Don't rely on color alone
  - Focus indicator contrast
  - Dark mode accessibility
  - Tool: aXe, Lighthouse accessibility
- **Focus:** Color accessibility

### Task 333 — Accessible Components Library 🔴

- Build accessible versions of:
  - Modal/Dialog (focus trap, Escape, backdrop click)
  - Dropdown menu (keyboard navigation, ARIA)
  - Tabs (ARIA tabs pattern, arrow keys)
  - Accordion (ARIA expanded, keyboard)
  - Tooltip (accessible name, keyboard trigger)
  - Combobox/Autocomplete (ARIA combobox)
  - All must pass aXe audit
- **Focus:** Accessible component patterns

### Task 334 — Reduced Motion Support 🟢

- Motion preferences:
  - `prefers-reduced-motion` media query
  - Reduce/Remove animations
  - Alternative static transitions
  - useMediaQuery hook for reduced motion
  - Framer Motion `useReducedMotion`
  - Test with OS settings
- **Focus:** Inclusive design

### Task 335 — Focus Management 🟡

- Focus control:
  - Focus on route change
  - Focus after modal close (return to trigger)
  - Focus after dynamic content insert
  - Focus ring styling (visible but aesthetic)
  - Focus trap library (focus-trap-react)
  - Manage focus in SPA navigation
- **Tech:** focus-trap-react

### Task 336 — Accessible Forms Deep Dive 🟡

- Form accessibility:
  - Labels properly associated
  - Error messages linked (aria-describedby)
  - Required fields indicated (aria-required)
  - Fieldset/Legend for groups
  - Autocomplete attributes
  - Input instructions
  - Error summary with links
  - Success confirmation for screen readers
- **Focus:** Form accessibility

### Task 337 — ARIA Live Regions 🟡

- Dynamic content accessibility:
  - aria-live="polite" vs "assertive"
  - Toast notifications announced
  - Search results count announced
  - Loading state announced
  - Chat messages announced
  - Form validation errors announced
  - Status updates
- **Focus:** Dynamic content for screen readers

### Task 338 — Accessibility Testing Automation 🟡

- Automated a11y testing:
  - aXe integration in tests (vitest-axe)
  - Storybook accessibility addon
  - Lighthouse accessibility audit in CI
  - Playwright accessibility testing
  - Custom a11y test helpers
  - Accessibility CI gate (must pass)
  - Report generation
- **Focus:** Continuous accessibility testing

---

## Section D: Performance Projects (Tasks 339-350)

### Task 339 — Optimize a Slow React App 🔴

- Given: intentionally slow app (provided boilerplate)
  - Fix unnecessary re-renders (10+ components)
  - Implement virtualization
  - Add code splitting
  - Optimize images
  - Reduce bundle size
  - Document each optimization + impact
  - Before/After Lighthouse scores
- **Focus:** Real-world optimization

### Task 340 — Build a Performance-First E-commerce 🔴

- E-commerce with 95+ Lighthouse:
  - SSG for product pages
  - Optimized images (WebP, responsive)
  - Font optimization
  - Code splitting per route
  - Lazy load below-fold content
  - Prefetch on hover
  - Service Worker for offline
  - 95+ all Lighthouse categories
- **Focus:** Performance-first development

### Task 341 — Infinite Scroll with Best Performance 🟡

- Optimized infinite scroll:
  - Virtualization (only render visible)
  - Intersection Observer for trigger
  - Request deduplication
  - Memory cleanup (remove far-away items)
  - Scroll position restoration
  - Loading skeleton
  - Error handling + retry
- **Focus:** Production-grade infinite scroll

### Task 342 — Real-time Dashboard Performance 🔴

- High-frequency data updates:
  - WebSocket receiving data every second
  - Chart updating without jank
  - Table updating without re-render flash
  - Memory management for continuous data
  - Throttle UI updates
  - Web Worker for data processing
  - 60fps while updating
- **Focus:** Real-time performance

### Task 343 — Large Form Performance 🟡

- 100+ field form:
  - No keystroke delay
  - React Hook Form (uncontrolled)
  - Lazy field validation
  - Section-based rendering
  - Virtual form fields
  - Measure ও maintain < 16ms per keystroke
- **Focus:** Large form responsiveness

### Task 344 — Map Application Performance 🟡

- Map with 1000+ markers:
  - Marker clustering
  - Viewport-based loading
  - Lazy tile loading
  - Smooth pan ও zoom
  - Popup on demand (not preloaded)
  - Memory cleanup for offscreen markers
- **Tech:** React Leaflet

### Task 345 — Media-Heavy Page Performance 🟡

- Page with 100+ images/videos:
  - Lazy load all media
  - Placeholder strategy
  - Responsive serving
  - Video poster frames
  - Pause offscreen videos
  - Memory management
  - Bandwidth-aware loading
- **Focus:** Media optimization

### Task 346 — Search Performance Optimization 🟡

- Fast search experience:
  - Debounced input (300ms)
  - Client-side index (Fuse.js)
  - Highlight matches without re-render
  - Virtual results list
  - Instant client results → then server results
  - Cancel previous requests
  - Measure: search response time
- **Tech:** Fuse.js

### Task 347 — Build Pipeline Optimization 🟡

- Faster builds:
  - Vite vs Webpack build speed
  - Turbopack exploration
  - Parallel processing
  - Cache optimization
  - Dependency optimization
  - Build time monitoring
  - CI build speed optimization
- **Focus:** Developer experience performance

### Task 348 — Progressive Enhancement 🟡

- Works without JS, better with JS:
  - Form works without JavaScript (Server Action)
  - Navigation works without JavaScript
  - Content readable without CSS
  - Enhanced features with JS (animations, interactions)
  - `<noscript>` fallback
  - Test with JavaScript disabled
- **Focus:** Resilient web development

### Task 349 — Performance Regression Prevention ⚫

- CI performance gates:
  - Lighthouse CI in GitHub Actions
  - Bundle size check (size-limit)
  - Performance budgets enforcement
  - Web Vitals monitoring
  - Automated performance reports on PR
  - Block merge if performance regresses
  - Historical performance tracking
- **Focus:** Prevent performance regressions

### Task 350 — Performance Case Study ⚫

- Pick any 3 tasks from Phases 1-6:
  - Audit with Lighthouse, DevTools, bundle analyzer
  - Identify top 5 performance issues
  - Fix each issue
  - Document: issue, solution, impact
  - Before/After screenshots ও scores
  - Write blog-post style performance report
- **Focus:** Phase 07 graduation — performance mastery

---

## ✅ Phase 07 Completion Checklist

- [ ] Core Web Vitals বোঝো ও optimize করতে পারো
- [ ] Chrome DevTools Performance tab confidently ব্যবহার করতে পারো
- [ ] Bundle optimization পারো
- [ ] Image, font, third-party script optimization পারো
- [ ] React performance (re-renders, virtualization, code splitting)
- [ ] Accessibility WCAG 2.1 basics জানো
- [ ] Keyboard navigation implement করতে পারো
- [ ] Performance monitoring setup করতে পারো
- [ ] CI performance gates configure করতে পারো
- [ ] Performance case study লিখতে পারো

> ✅ Ready? → **Phase 08: System Design (`Phase-08-System-Design.md`)** এ যাও
