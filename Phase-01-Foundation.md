# Phase 01 — HTML, CSS & JavaScript Deep Foundation (Tasks 1-50)

> এই Phase এ তোমার foundation rock-solid হবে। Foundation weak হলে React/Next.js এ গিয়ে struggle করবে।
> 🟢 Easy | 🟡 Medium | 🔴 Hard

---

## Section A: HTML & Semantic Web (Tasks 1-8)

### Task 1 — Semantic HTML Resume Page 🟢

- নিজের resume/CV একটা single HTML page এ বানাও
- শুধু HTML ব্যবহার করো (no CSS yet)
- Semantic tags: `<header>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`, `<nav>`
- Proper heading hierarchy (h1 → h2 → h3)
- `<table>` দিয়ে skills/education section
- **Focus:** Semantic HTML কেন important সেটা বুঝো

### Task 2 — Accessible Form Page 🟢

- Registration form বানাও with all input types
- Text, email, password, number, date, range, color, file, textarea, select, radio, checkbox
- `<label>` properly associate করো (for/id)
- `<fieldset>` ও `<legend>` ব্যবহার করো
- `required`, `pattern`, `minlength`, `maxlength` attributes
- **Focus:** HTML5 form validation ও accessibility

### Task 3 — SEO-Optimized Blog Page 🟢

- একটা blog article page বানাও
- Open Graph meta tags (`og:title`, `og:description`, `og:image`)
- Twitter Card meta tags
- Structured data (JSON-LD) for article
- Proper `<meta>` tags (description, viewport, charset)
- **Focus:** SEO fundamentals বোঝো

### Task 4 — HTML5 Media Player Page 🟢

- `<video>` ও `<audio>` elements ব্যবহার করো
- Custom controls ছাড়াই browser default controls
- `<picture>` element with `<source>` for responsive images
- `<figure>` ও `<figcaption>` ব্যবহার করো
- Lazy loading images (`loading="lazy"`)
- **Focus:** HTML5 media elements বোঝো

### Task 5 — HTML Email Template 🟡

- Table-based HTML email template বানাও
- Inline CSS only (no external stylesheets)
- Email-compatible layout (table > div)
- Responsive email (media queries inline)
- Test different email clients considerations
- **Focus:** Email HTML এর limitations ও constraints বোঝো

### Task 6 — Web Accessibility Audit Page 🟡

- একটা e-commerce product page বানাও
- ARIA roles ও attributes properly ব্যবহার করো (`aria-label`, `aria-describedby`, `role`)
- Skip navigation link
- Keyboard navigable (tab order)
- Screen reader friendly (test with browser tools)
- Color contrast proper রাখো
- **Focus:** WCAG 2.1 guidelines বোঝো

### Task 7 — Canvas Drawing Basics 🟡

- HTML5 `<canvas>` element ব্যবহার করো
- Basic shapes আঁকো (rectangle, circle, triangle, line)
- Text render করো canvas এ
- Simple animation (bouncing ball)
- Mouse click event handle করো canvas এ
- **Focus:** Canvas API fundamentals

### Task 8 — SVG Interactive Infographic 🟡

- Inline SVG দিয়ে একটা infographic বানাও
- SVG shapes: `<rect>`, `<circle>`, `<path>`, `<text>`, `<line>`
- SVG animation with `<animate>` বা CSS
- Hover effects on SVG elements
- Responsive SVG (viewBox)
- **Focus:** SVG fundamentals বোঝো

---

## Section B: CSS Mastery (Tasks 9-22)

### Task 9 — CSS Box Model Visualizer 🟢

- একটা interactive page বানাও যেখানে box model visually দেখা যায়
- margin, border, padding, content area color-coded
- Input fields দিয়ে values change করলে box update হবে
- `box-sizing: border-box` vs `content-box` demonstrate করো
- **Tech:** HTML, CSS, Vanilla JS

### Task 10 — Flexbox Holy Grail Layout 🟢

- Classic "Holy Grail" layout বানাও (header, footer, main + 2 sidebars)
- শুধু Flexbox ব্যবহার করো (no Grid)
- Responsive: mobile এ stack হবে, desktop এ side by side
- Sticky header ও footer
- Sidebar collapse on mobile
- **Focus:** Flexbox deep understanding

### Task 11 — CSS Grid Magazine Layout 🟡

- Newspaper/Magazine style layout বানাও
- CSS Grid template areas ব্যবহার করো
- Featured article (spans multiple columns)
- Sidebar with ads
- Grid auto-fill/auto-fit for card grid
- Named grid lines ব্যবহার করো
- **Focus:** CSS Grid deep understanding

### Task 12 — CSS Custom Properties Theme System 🟡

- Theme system বানাও CSS custom properties (variables) দিয়ে
- 4টা theme: Light, Dark, Ocean Blue, Forest Green
- CSS variables দিয়ে colors, fonts, spacing define করো
- JavaScript দিয়ে theme switch করো
- Smooth transition between themes
- LocalStorage এ theme save করো
- **Focus:** CSS Custom Properties ও theming architecture

### Task 13 — Pure CSS Animation Showcase 🟡

- 10টা different CSS animation বানাও একটা page এ
- Loading spinners (3 types)
- Bouncing dots, Pulse effect, Wave animation
- Typing text effect, Flip card
- Hover animations (scale, rotate, skew)
- @keyframes ভালো করে বুঝো
- **Focus:** CSS Animations ও Transitions mastery

### Task 14 — Responsive Navigation Patterns 🟡

- 4টা different navigation pattern বানাও একই page এ:
  1. Hamburger menu (mobile slide-in)
  2. Mega menu (desktop dropdown with columns)
  3. Tab navigation (horizontal scrollable on mobile)
  4. Bottom navigation bar (mobile app style)
- CSS only (no JS for basic toggle — use checkbox hack)
- **Focus:** Responsive navigation patterns

### Task 15 — CSS Clamp & Container Queries 🟡

- Fluid typography with `clamp()`
- Container queries ব্যবহার করো (`@container`)
- Card component যেটা container size অনুযায়ী layout change করে
- `min()`, `max()`, `clamp()` ব্যবহার করো spacing ও sizing এ
- **Focus:** Modern CSS features

### Task 16 — CSS Specificity & Cascade Playground 🟢

- একটা page বানাও যেখানে CSS specificity demonstrate হবে
- Different selectors (element, class, id, inline, !important)
- Specificity score calculator UI
- Examples showing cascade order
- :is(), :where(), :has() pseudo-classes ব্যবহার করো
- **Focus:** CSS Specificity ও Cascade deeply বোঝো

### Task 17 — Tailwind CSS Component Library 🟡

- Tailwind CSS install করো
- 15টা reusable components বানাও:
  - Button (primary, secondary, outline, ghost, icon)
  - Input (text, with icon, error state)
  - Card (basic, with image, pricing)
  - Badge, Alert, Avatar, Tooltip
- Responsive variants
- Dark mode variants
- **Tech:** Tailwind CSS
- **Focus:** Utility-first CSS methodology

### Task 18 — CSS Grid Dashboard Layout 🟡

- Admin dashboard layout with CSS Grid
- Resizable sidebar (CSS resize property)
- Dashboard cards with different grid spans
- Responsive breakpoints (mobile, tablet, desktop)
- Grid template areas for layout regions
- **Focus:** Complex Grid layouts

### Task 19 — CSS Scroll-Driven Animations 🔴

- Scroll-based animations with `animation-timeline: scroll()`
- Progress bar that fills on scroll
- Elements that fade/slide in on scroll
- Parallax effect pure CSS
- Scroll snap sections
- **Focus:** Modern CSS scroll features

### Task 20 — CSS Architecture — BEM + SCSS 🟡

- একটা multi-page website বানাও
- BEM naming convention strictly follow করো
- SCSS ব্যবহার করো (variables, mixins, nesting, partials)
- 7-1 SCSS architecture pattern
- File structure: abstracts, base, components, layout, pages
- **Focus:** Scalable CSS architecture

### Task 21 — Pure CSS Art 🟡

- শুধু CSS দিয়ে একটা illustration বানাও (animal, landscape, বা character)
- Box shadows, gradients, border-radius ব্যবহার করো
- Pseudo-elements (::before, ::after) ব্যবহার করো
- No images, no SVG — pure CSS only
- **Focus:** CSS creativity ও deep understanding

### Task 22 — CSS Print Stylesheet 🟢

- একটা invoice/report page বানাও
- Print stylesheet আলাদা করো (`@media print`)
- Print-specific styles (no background, proper margins)
- Page breaks control (`break-before`, `break-after`)
- Hide unnecessary elements (nav, footer, buttons)
- **Focus:** Print CSS understanding

---

## Section C: JavaScript Deep Dive (Tasks 23-42)

### Task 23 — JavaScript Data Structures 🟡

- নিজে implement করো (built-in ব্যবহার না করে):
  - Stack (push, pop, peek, isEmpty)
  - Queue (enqueue, dequeue, front, isEmpty)
  - Linked List (add, remove, find, traverse)
  - Hash Table (set, get, delete, collision handling)
- প্রতিটার visual representation DOM এ render করো
- **Focus:** Data structures ভালো করে বোঝো

### Task 24 — Array Method Mastery 🟡

- নিজের implementation বানাও এগুলোর:
  - `map()`, `filter()`, `reduce()`, `find()`, `some()`, `every()`
  - `flat()`, `flatMap()`, `groupBy()`
- প্রতিটা method এর interactive demo page
- Edge cases handle করো
- Performance comparison (custom vs native)
- **Focus:** Higher-order functions deeply বোঝো

### Task 25 — Closure & Scope Playground 🟡

- Interactive page বানাও যেখানে closure demonstrate হবে
- Counter with private state (closure)
- Module pattern example
- Memoization function implement করো
- Event listener closure trap demonstrate + fix
- Loop closure problem demonstrate + fix (var vs let)
- **Focus:** Closure, Scope Chain, Lexical Environment বোঝো

### Task 26 — Promise & Async/Await Deep Dive 🟡

- নিজের Promise implementation বানাও (simplified)
- Promise chaining visualizer (UI তে show করো কোন promise কখন resolve হলো)
- Promise.all, Promise.race, Promise.allSettled, Promise.any demonstrate করো
- Async/Await error handling patterns
- Retry mechanism with exponential backoff
- **Focus:** Asynchronous JavaScript mastery

### Task 27 — Event System & Custom Events 🟡

- Custom Event Emitter class বানাও (on, off, emit, once)
- Event bubbling ও capturing visualizer
- Event delegation interactive demo
- Custom DOM events create ও dispatch করো
- Throttle ও Debounce implement করো from scratch
- **Focus:** Event system deeply বোঝো

### Task 28 — DOM Manipulation Library 🔴

- নিজের mini jQuery-like library বানাও:
  - `$(selector)` — element select
  - `.addClass()`, `.removeClass()`, `.toggleClass()`
  - `.css()`, `.attr()`, `.text()`, `.html()`
  - `.on()`, `.off()` — event handling
  - `.animate()` — simple animation
  - `.ajax()` — fetch wrapper
- Chaining support
- **Focus:** DOM API deeply বোঝো

### Task 29 — Fetch API & HTTP Client 🟡

- নিজের HTTP client class বানাও (Axios-like):
  - GET, POST, PUT, PATCH, DELETE methods
  - Request/Response interceptors
  - Automatic JSON parsing
  - Error handling with custom error classes
  - Request timeout support
  - Base URL configuration
- JSONPlaceholder API ব্যবহার করে test করো
- **Focus:** HTTP ও Fetch API deeply বোঝো

### Task 30 — Local Storage Manager 🟡

- Advanced localStorage wrapper বানাও:
  - Set with expiry time (TTL)
  - Get with expiry check
  - Namespace support (prefix)
  - Data encryption (simple base64)
  - Storage size tracking
  - Event listener for storage changes
  - Fallback to in-memory storage
- Interactive demo page
- **Focus:** Browser storage APIs

### Task 31 — JavaScript Design Patterns 🔴

- Interactive demo page বানাও এই patterns এর:
  - Singleton (Theme Manager)
  - Observer (Event System)
  - Factory (UI Component Creator)
  - Strategy (Sorting algorithms)
  - Decorator (Logger)
  - Module Pattern (Counter)
  - Pub/Sub (Message Bus)
- প্রতিটার code + visual demo
- **Focus:** Design patterns বোঝো — React এ পরে কাজে লাগবে

### Task 32 — Regular Expression Tester 🟡

- Regex tester tool বানাও (like regex101):
  - Input field for regex pattern
  - Input field for test string
  - Real-time match highlighting
  - Match groups display
  - Common regex patterns library
  - Regex explanation (basic)
- **Focus:** Regular expressions ভালো করে বোঝো

### Task 33 — Web Workers Calculator 🟡

- Heavy computation Web Worker দিয়ে করো:
  - Fibonacci calculator (large numbers)
  - Image processing (grayscale, blur — pixel manipulation)
  - Sorting visualizer (large array sort in worker)
  - Main thread blocked না হওয়া demonstrate করো
  - Worker ও main thread communication
- **Focus:** Web Workers ও multi-threading

### Task 34 — Intersection Observer Projects 🟡

- 3টা project:
  1. Infinite scroll (load more items on scroll)
  2. Lazy loading images (load when visible)
  3. Scroll-triggered animations (elements animate when they enter viewport)
- Custom hook-style reusable function বানাও
- **Focus:** Intersection Observer API

### Task 35 — Drag & Drop File Manager 🟡

- Desktop file manager style UI:
  - Drag & drop files between folders
  - Create new folder
  - Rename files/folders
  - Multi-select with Ctrl/Shift click
  - Context menu (right-click)
  - HTML Drag and Drop API ব্যবহার করো
- **Focus:** Drag & Drop API, complex DOM interactions

### Task 36 — JavaScript Class & Prototypal Inheritance 🟡

- UI Component system বানাও classes দিয়ে:
  - Base `Component` class
  - `Button`, `Input`, `Modal`, `Card` extends Component
  - Private fields (#) ব্যবহার করো
  - Static methods
  - Getter/Setter
  - instanceof ও prototype chain demonstrate করো
- **Focus:** OOP in JavaScript

### Task 37 — Functional Programming Toolkit 🟡

- FP utility library বানাও:
  - `compose()`, `pipe()`
  - `curry()`, `partial()`
  - `memoize()`
  - `debounce()`, `throttle()`
  - `deepClone()`, `deepMerge()`
  - `groupBy()`, `chunk()`, `zip()`
- সব pure functions হতে হবে
- **Focus:** Functional programming concepts

### Task 38 — Error Handling & Debugging 🟡

- Error handling patterns demonstrate করো:
  - Custom Error classes (ValidationError, NetworkError, AuthError)
  - try/catch/finally patterns
  - Global error handler (window.onerror, unhandledrejection)
  - Error boundary concept (manual)
  - Error logging system (console.group, console.table)
  - Stack trace reading practice
- **Focus:** Production-grade error handling

### Task 39 — Module System Comparison 🟡

- Same project 3 ভাবে structure করো:
  1. IIFE Module Pattern
  2. CommonJS (require/module.exports)
  3. ES Modules (import/export)
- Demonstrate: named exports, default exports, re-exports
- Dynamic import (`import()`) ব্যবহার করো
- Tree shaking concept বোঝো
- **Focus:** Module systems deeply বোঝো

### Task 40 — Service Worker & Offline App 🔴

- একটা notes app বানাও যেটা offline কাজ করে:
  - Service Worker register ও install করো
  - Cache static assets
  - Cache API responses
  - Offline fallback page
  - Background sync (when back online)
  - App manifest for PWA
- **Focus:** Service Workers ও PWA basics

### Task 41 — WebSocket Chat Application 🔴

- Real-time chat app:
  - WebSocket connection establish করো
  - Send ও receive messages
  - Online user list
  - Typing indicator
  - Reconnection logic
  - Message history
- Backend: Simple Node.js WebSocket server (ws package)
- **Focus:** WebSocket API ও real-time communication

### Task 42 — JavaScript Performance Profiling 🟡

- একটা intentionally slow page বানাও then optimize করো:
  - Memory leak demonstrate ও fix করো
  - requestAnimationFrame vs setTimeout for animation
  - Virtual scrolling implement করো (render only visible items)
  - Document fragment ব্যবহার করো batch DOM manipulation এ
  - Performance.now() দিয়ে benchmark করো
- **Focus:** JavaScript performance optimization

---

## Section D: Build Tools & Modern Workflow (Tasks 43-50)

### Task 43 — Git Mastery Project 🟢

- একটা project এ সব Git features practice করো:
  - Branch create, merge, rebase
  - Resolve merge conflicts
  - Interactive rebase (squash commits)
  - Cherry-pick
  - Stash
  - Git bisect (find bug)
  - Meaningful commit messages (Conventional Commits)
- **Focus:** Git workflow mastery

### Task 44 — Vite Project Setup from Scratch 🟡

- Vite project manually configure করো:
  - TypeScript support
  - Path aliases (@/ → src/)
  - Environment variables (.env)
  - CSS Modules setup
  - SVG as component
  - Build optimization (chunking)
  - Preview production build locally
- **Focus:** Build tools understanding

### Task 45 — ESLint & Prettier Configuration 🟡

- Project এ ESLint + Prettier setup করো:
  - Custom ESLint rules
  - Airbnb বা custom style guide
  - TypeScript ESLint
  - Auto-fix on save
  - Husky + lint-staged (pre-commit hooks)
  - commitlint (commit message format)
- **Focus:** Code quality automation

### Task 46 — NPM Package Creation 🟡

- নিজের একটা NPM package বানাও:
  - Utility library (5-10 useful functions)
  - TypeScript দিয়ে লেখো
  - Build with tsup বা Rollup
  - Type declarations generate করো
  - README with documentation
  - Publish to npm (optional: scoped package)
- **Focus:** Package creation ও distribution

### Task 47 — Monorepo Setup with Turborepo 🔴

- Monorepo structure বানাও:
  - Shared UI component library
  - Web app (Next.js)
  - Documentation site
  - Shared TypeScript config
  - Shared ESLint config
  - Turborepo pipeline configuration
- **Tech:** Turborepo, pnpm workspaces
- **Focus:** Monorepo architecture

### Task 48 — CI/CD Pipeline with GitHub Actions 🟡

- GitHub Actions workflow বানাও:
  - Lint check on PR
  - Type check on PR
  - Run tests on PR
  - Build check
  - Auto deploy to Vercel on merge to main
  - PR comment with build status
- **Focus:** CI/CD basics

### Task 49 — Docker for Frontend 🟡

- Frontend app Docker এ run করো:
  - Dockerfile লেখো (multi-stage build)
  - .dockerignore file
  - Docker Compose with nginx
  - Development container
  - Production-optimized image
  - Environment variables in Docker
- **Focus:** Containerization basics

### Task 50 — Webpack Deep Dive 🔴

- Webpack manually configure করো (CRA/Vite ছাড়া):
  - Entry, Output, Loaders, Plugins
  - Babel loader for JSX/TypeScript
  - CSS/SCSS loaders
  - Image/Font asset modules
  - Code splitting (dynamic imports)
  - Bundle analyzer
  - Dev server with HMR
  - Production optimization (minify, tree-shake)
- **Focus:** Webpack internals বোঝো

---

## ✅ Phase 01 Completion Checklist

- [ ] সব 50টা task complete হয়েছে
- [ ] প্রতিটা task Git এ commit আছে
- [ ] HTML semantic elements confidently ব্যবহার করতে পারো
- [ ] CSS Flexbox ও Grid দুটোই ভালো পারো
- [ ] CSS animations ও transitions পারো
- [ ] JavaScript closures, promises, async/await clearly বোঝো
- [ ] DOM manipulation হাতের মতো পারো
- [ ] Git comfortably ব্যবহার করতে পারো
- [ ] Build tools (Vite, Webpack) বোঝো
- [ ] TypeScript basics জানো (Phase 02 তে deep dive হবে)

> ✅ সব complete? → **Phase 02: React Core (`Phase-02-React-Core.md`)** এ যাও
