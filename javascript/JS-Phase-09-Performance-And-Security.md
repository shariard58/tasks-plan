# JS Phase 09 — Performance, Memory & Security (Tasks 246-270)

> কোড লিখতে পারাটা যথেষ্ট না — efficient কোড লিখতে হবে। Memory leak, slow render, security hole — এগুলো career destroyer।

---

## Section A: Memory Management (Tasks 246-254)

### Task 246 — Memory Lifecycle in JavaScript 🟡

- Memory stages:
  1. **Allocation:** variable declare, object create
  2. **Usage:** read/write
  3. **Deallocation:** garbage collection (automatic)
  - Stack memory: primitives, function calls (fast, small, automatic)
  - Heap memory: objects, arrays, functions (large, GC managed)
  - **Visualize:** Stack vs Heap for different code examples
  - `performance.memory` (Chrome only): `usedJSHeapSize`, `totalJSHeapSize`

### Task 247 — Garbage Collection Algorithms 🔴

- How GC works:
  - **Mark and Sweep** (modern JS):
    1. Start from roots (global, stack, closures)
    2. Mark all reachable objects
    3. Sweep (free) unmarked objects
  - **Generational GC** (V8):
    - Young generation (nursery): short-lived objects, frequent minor GC
    - Old generation: survived multiple GCs, rare major GC
    - Scavenger for young, Mark-Compact for old
  - **Incremental/Concurrent GC:** doesn't block main thread
  - **Build:** Script that demonstrates GC behavior (allocate → nullify → check memory)

### Task 248 — Memory Leaks — Detection & Prevention 🔴

- Common memory leak patterns:
  1. **Global variables:** `function leak() { data = hugeArray; }` (no `let/const`)
  2. **Forgotten timers:**
     ```js
     setInterval(() => {
       /* references large data */
     }, 1000);
     // Never cleared!
     ```
  3. **Detached DOM nodes:**
     ```js
     const el = document.getElementById('big-table');
     document.body.removeChild(el);
     // Still referenced by variable `el`!
     ```
  4. **Closures holding references:**
     ```js
     function createHandler() {
       const hugeData = new Array(1000000);
       return () => console.log(hugeData.length); // holds hugeData forever
     }
     ```
  5. **Event listeners not removed:**
     ```js
     element.addEventListener('click', handler);
     // Element removed, but handler still referenced
     ```
  6. **Map/Set with object keys (use WeakMap/WeakSet instead)**
  - **Build:** 6 leaky programs → find ও fix each leak
  - **Tool:** Chrome DevTools Memory panel → take heap snapshots

### Task 249 — Chrome DevTools Memory Profiling 🟡

- Memory debugging:
  1. **Heap Snapshot:** find what's in memory
     - Take snapshot → filter → find large objects
     - Compare snapshots → find growing allocations
  2. **Allocation Timeline:** see allocations over time
  3. **Allocation Sampling:** statistical profiling
  - **Workflow:**
    1. Take snapshot A
    2. Do the action
    3. Take snapshot B
    4. Compare A ও B → find leaked objects
  - **Challenge:** Profile 5 web apps → find ও report memory issues

### Task 250 — WeakMap/WeakSet for Memory-Safe Patterns 🟡

- Avoid leaks with weak references:

  ```js
  // BAD: Map keeps objects alive forever
  const cache = new Map();
  cache.set(domElement, metadata); // domElement can never be GC'd

  // GOOD: WeakMap allows GC
  const cache = new WeakMap();
  cache.set(domElement, metadata); // domElement can be GC'd if no other refs
  ```

  - **Pattern 1:** DOM element metadata
  - **Pattern 2:** Object-private data
  - **Pattern 3:** Memoization cache (auto-cleanup)
  - **Pattern 4:** Listener tracking
  - **Build:** Memory-safe event manager using WeakMap

### Task 251 — ArrayBuffer & Memory-Efficient Data 🟡

- Efficient binary data:
  - Regular array: `[1, 2, 3]` — each element is full JS value (8+ bytes)
  - TypedArray: `new Uint8Array([1, 2, 3])` — 1 byte per element
  - **When to use:** large datasets, images, audio, binary protocols
  - **Build:** Memory-efficient data table (TypedArray vs regular Array benchmark)
  - **Build:** Binary file reader (parse image headers)

### Task 252 — Object Pooling 🟡

- Reuse objects instead of creating new:
  ```js
  class ObjectPool {
    #pool = [];
    #factory;
    #reset;

    constructor(factory, reset, initialSize = 10) {
      this.#factory = factory;
      this.#reset = reset;
      for (let i = 0; i < initialSize; i++) this.#pool.push(factory());
    }

    acquire() {
      return this.#pool.length > 0 ? this.#pool.pop() : this.#factory();
    }

    release(obj) {
      this.#reset(obj);
      this.#pool.push(obj);
    }
  }
  ```

  - Use cases: game objects, particles, database connections
  - **Build:** Particle system with object pooling (compare with/without)

### Task 253 — String Memory & Interning 🟡

- String optimization:
  - String interning: V8 reuses identical strings
  - String concatenation: `+` vs template literal vs `Array.join()`
  - Long strings: `slice()` may keep reference to original (V8 optimization)
  - `JSON.parse(JSON.stringify(str))` — force new string allocation
  - **Benchmark:** String building methods — which is fastest?

### Task 254 — Build: Memory Leak Detector 🔴

- **Build:** Tool that detects leaks:

  ```js
  const detector = new LeakDetector();

  detector.startTracking();
  // ... run code ...
  const report = detector.stopTracking();

  console.log(report);
  // { heapGrowth: '5MB', suspectedLeaks: [...], suggestions: [...] }
  ```

  - Track heap size over time
  - Compare snapshots
  - Identify growing allocations
  - Suggest fixes

---

## Section B: Performance Optimization (Tasks 255-264)

### Task 255 — Runtime Performance Fundamentals 🟡

- JS performance factors:
  - Parsing ও compilation time
  - Execution time (CPU)
  - Memory allocation ও GC pressure
  - DOM operations (reflow, repaint)
  - Network (latency, bandwidth)
  - **Tool:** `performance.now()` for micro-benchmarking:
    ```js
    const start = performance.now();
    doWork();
    console.log(`Took ${performance.now() - start}ms`);
    ```
  - **Warning:** Micro-benchmarks can be misleading (JIT, GC, warm-up)
  - **Build:** Benchmarking utility with warm-up, iterations, statistical analysis

### Task 256 — V8 Hidden Classes & Inline Caching 🔴

- V8 optimizations:
  - **Hidden Classes:** V8 creates internal classes for object shapes

    ```js
    // GOOD: consistent shape
    function Point(x, y) {
      this.x = x;
      this.y = y;
    }

    // BAD: different shapes
    const a = {};
    a.x = 1;
    a.y = 2;
    const b = {};
    b.y = 2;
    b.x = 1; // Different hidden class!
    ```

  - **Inline Caching:** V8 caches property lookup for known shapes
  - **Monomorphic** (fast) → **Polymorphic** (slower) → **Megamorphic** (slowest)
  - **Rules:**
    1. Always initialize properties in same order
    2. Don't add properties after construction
    3. Don't delete properties
    4. Use consistent types for same property
  - **Build:** Demo showing performance difference between optimized ও deoptimized code

### Task 257 — DOM Performance 🟡

- Rendering pipeline: JS → Style → Layout → Paint → Composite
  - **Reflow (Layout):** expensive — triggered by reading/changing layout properties
    - `offsetHeight`, `scrollTop`, `getComputedStyle()` — trigger forced reflow
  - **Repaint:** less expensive — visual changes only
  - **Composite:** cheapest — `transform`, `opacity` only
  - **Best practices:**
    1. Batch DOM reads, then batch DOM writes
    2. Use `requestAnimationFrame` for visual updates
    3. Use `will-change` for animation hints
    4. Use `transform` instead of `top/left` for animations
    5. Use `DocumentFragment` for batch insertions
    6. Use `display: none` before complex manipulations
  - **Build:** 10,000 item list — optimize from 500ms to <16ms render time

### Task 258 — Virtual Scrolling / Windowing 🔴

- Render only visible items:
  ```js
  class VirtualList {
    constructor(container, items, itemHeight) {
      this.container = container;
      this.items = items;
      this.itemHeight = itemHeight;
      this.visibleCount = Math.ceil(container.clientHeight / itemHeight);

      container.addEventListener('scroll', () => this.render());
      this.render();
    }

    render() {
      const scrollTop = this.container.scrollTop;
      const startIndex = Math.floor(scrollTop / this.itemHeight);
      const endIndex = startIndex + this.visibleCount + 1;

      // Only render items[startIndex..endIndex]
      // Set container scrollHeight to total height
      // Position visible items absolutely
    }
  }
  ```

  - **Build:** Virtual list that handles 1,000,000 items smoothly
  - **Build:** Virtual grid (rows ও columns)

### Task 259 — Web Workers for CPU-Intensive Tasks 🟡

- Offload heavy work:
  - **Identify:** What should go to worker?
    - Image processing, data parsing, sorting, encryption, compression
  - **NOT for workers:** DOM manipulation, small calculations
  - **Transferable objects:** zero-copy data transfer:
    ```js
    const buffer = new ArrayBuffer(1024);
    worker.postMessage(buffer, [buffer]); // Transfer, not copy
    // buffer is now empty in main thread!
    ```
  - **Build:** Image filter pipeline in Web Worker (grayscale, blur, sepia)
  - **Benchmark:** Main thread vs Worker for heavy computation

### Task 260 — Lazy Loading & Code Splitting 🟡

- Load code on demand:
  - Dynamic import: `const module = await import('./heavy-module.js')`
  - Lazy load images: `<img loading="lazy">`
  - Lazy load components: `IntersectionObserver` + dynamic import
  - Route-based splitting in SPA
  - **Build:** Page that lazy loads:
    1. Images (IntersectionObserver)
    2. Components (dynamic import when visible)
    3. Heavy libraries (loaded only when needed)
  - **Measure:** Page load time before ও after

### Task 261 — Caching Strategies 🟡

- Cache everything possible:
  - **Memoization:** function result cache
  - **HTTP cache:** Cache-Control, ETag, Last-Modified
  - **Service Worker cache:** offline-first
  - **localStorage cache:** API response cache with TTL
  - **In-memory cache:** LRU cache for frequent lookups
  - **Build:** Multi-layer cache system:

    ```js
    const cache = createCache({
      layers: [
        { type: 'memory', maxSize: 100 },
        { type: 'localStorage', ttl: '1h' },
        { type: 'network', revalidate: true },
      ],
    });

    const data = await cache.get('key', fetchFn);
    // Checks memory → localStorage → network
    ```

### Task 262 — requestAnimationFrame & Animation Performance 🟡

- Smooth 60fps animations:

  ```js
  let startTime;

  function animate(timestamp) {
    if (!startTime) startTime = timestamp;
    const elapsed = timestamp - startTime;
    const progress = Math.min(elapsed / duration, 1);

    // Use CSS transform (composite only, no reflow)
    element.style.transform = `translateX(${progress * 300}px)`;

    if (progress < 1) requestAnimationFrame(animate);
  }

  requestAnimationFrame(animate);
  ```

  - Easing functions: linear, easeIn, easeOut, easeInOut, bounce, elastic
  - **Build:** Animation engine:
    - Supports multiple concurrent animations
    - Custom easing functions
    - Animation chaining ও grouping
    - Pause/resume/cancel
    - **Output:** Smooth animation library

### Task 263 — Debouncing & Throttling Performance Patterns 🟡

- (Revisited from Phase 03, now with performance focus):
  - **Debounce patterns:**
    - Search input: debounce API calls
    - Resize handler: debounce layout calculations
    - Save draft: debounce auto-save
  - **Throttle patterns:**
    - Scroll handler: throttle position calculations
    - Mouse move: throttle UI updates
    - API polling: throttle request frequency
  - **Build:** Performance comparison page:
    - Without debounce/throttle: show dropped frames, slow UI
    - With: show smooth 60fps

### Task 264 — Performance Audit & Optimization Project 🔴

- **Build:** Performance-optimized web app:
  1. Start with deliberately slow app (10+ performance issues)
  2. Profile with DevTools (Performance tab)
  3. Identify bottlenecks
  4. Fix each issue:
     - DOM batch updates
     - Virtual scrolling
     - Web Worker for computation
     - Lazy loading
     - Caching
     - rAF for animations
     - Debounce/throttle event handlers
  5. Before/After metrics
  - **Output:** Case study document with screenshots ও metrics

---

## Section C: JavaScript Security (Tasks 265-270)

### Task 265 — XSS (Cross-Site Scripting) 🔴

- How XSS works:
  - **Stored XSS:** Attacker injects script in database → served to users
  - **Reflected XSS:** Script in URL → reflected in response
  - **DOM-based XSS:** Client-side JS inserts untrusted data into DOM
  - **Examples:**

    ```js
    // VULNERABLE:
    element.innerHTML = userInput;
    document.write(userInput);
    eval(userInput);

    // SAFE:
    element.textContent = userInput;
    element.setAttribute('data-val', sanitize(userInput));
    ```

  - **Prevention:**
    1. Never use `innerHTML` with user data
    2. Use `textContent` for text
    3. Sanitize HTML (DOMPurify)
    4. Content Security Policy (CSP) headers
    5. HttpOnly cookies (JS can't access)
  - **Build:** XSS demo ও defense page (show attack ও prevention)

### Task 266 — CSRF (Cross-Site Request Forgery) 🟡

- How CSRF works:
  - User logged in to bank.com
  - Visits evil.com
  - evil.com has: `<form action="bank.com/transfer" method="POST">`
  - Browser sends cookies automatically!
  - **Prevention:**
    1. CSRF token (unique per session/request)
    2. SameSite cookie attribute
    3. Check Origin/Referer header
    4. Double-submit cookie
  - **Build:** CSRF token middleware for your Node.js server

### Task 267 — Prototype Pollution Attack 🔴

- (Deep dive from Task 116):

  ```js
  // Attacker sends: {"__proto__": {"isAdmin": true}}

  function merge(target, source) {
    for (const key in source) {
      if (typeof source[key] === 'object') {
        target[key] = target[key] || {};
        merge(target[key], source[key]); // Recursively merges __proto__!
      } else {
        target[key] = source[key];
      }
    }
  }

  merge({}, JSON.parse(attackerInput));
  // Now ALL objects have isAdmin: true!
  ```

  - **Build:** Safe merge function ও test with attacks

### Task 268 — Secure Coding Patterns 🟡

- Defense in depth:
  1. **Input validation:** whitelist, not blacklist
  2. **Output encoding:** context-aware escaping
  3. **Parameterized queries:** never concatenate SQL
  4. **Principle of least privilege:** minimal permissions
  5. **Content Security Policy:** restrict resource loading
  6. **Subresource Integrity:** verify CDN resources
  7. **HTTPS everywhere:** encrypt transport
  8. **Secure headers:** Helmet-like headers
  - **Build:** Security middleware with all protections:
    ```js
    app.use(
      security({
        csp: { defaultSrc: ["'self'"] },
        hsts: { maxAge: 31536000 },
        noSniff: true,
        xssProtection: true,
        frameGuard: 'deny',
      })
    );
    ```

### Task 269 — Dependency Security 🟡

- Supply chain security:
  - `npm audit` — check vulnerabilities
  - `npm audit fix` — auto-fix
  - Lock files: always commit `package-lock.json`
  - Pin versions: avoid `^` for critical dependencies
  - Review dependencies: check maintainer, downloads, issues
  - `npx` safety: verify package before running
  - `node --permission` — experimental permission model
  - **Build:** Dependency scanner script:
    ```js
    // Scan node_modules for:
    // - Known vulnerabilities (CVE)
    // - Suspicious postinstall scripts
    // - Unused dependencies
    // - Outdated packages
    ```

### Task 270 — Security Audit Project ⚫

- Phase 09 graduation:
  - **Build:** Deliberately vulnerable app (OWASP Top 10 JS vulnerabilities)
  - Then fix ALL vulnerabilities:
    1. XSS → sanitization + CSP
    2. CSRF → tokens + SameSite
    3. Injection → parameterized queries
    4. Prototype pollution → safe merge
    5. Insecure deserialization → validation
    6. Broken authentication → proper JWT + bcrypt
    7. Sensitive data exposure → encryption + secure headers
    8. Missing rate limiting → add limiter
    9. Dependency vulnerabilities → audit + fix
    10. Security misconfiguration → proper headers
  - **Output:** Security audit report with before/after

---

## ✅ Phase 09 Completion Checklist

- [ ] Memory lifecycle ও GC algorithms বোঝো
- [ ] 6 common memory leak patterns detect ও fix করতে পারো
- [ ] Chrome DevTools Memory profiling পারো
- [ ] WeakMap/WeakSet memory-safe patterns জানো
- [ ] V8 hidden classes ও inline caching বোঝো
- [ ] DOM performance optimization পারো
- [ ] Virtual scrolling implement করতে পারো
- [ ] Web Workers effectively ব্যবহার করতে পারো
- [ ] Caching strategies implement করতে পারো
- [ ] XSS, CSRF, prototype pollution বোঝো ও prevent করতে পারো
- [ ] Secure coding patterns follow করো
- [ ] Performance audit ও security audit করতে পারো

> ✅ Ready? → **JS Phase 10: Pure JS Projects — 30 Real-World Builds**
