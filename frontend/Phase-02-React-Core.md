# Phase 02 — React Core & TypeScript (Tasks 51-100)

> এই Phase এ React ও TypeScript solid হবে। প্রতিটা task TypeScript এ করো।
> 🟢 Easy | 🟡 Medium | 🔴 Hard

---

## Section A: TypeScript Foundation (Tasks 51-58)

### Task 51 — TypeScript Type System Playground 🟢

- Basic types: string, number, boolean, array, tuple, enum
- Union types, Intersection types, Literal types
- Type aliases vs Interfaces — কখন কোনটা ব্যবহার করবে
- Optional (?), Readonly, Record, Partial, Required, Pick, Omit
- Interactive examples with console output
- **Focus:** TypeScript basics solidly বোঝো

### Task 52 — TypeScript Generics Mastery 🟡

- Generic functions লেখো: identity, merge, pick
- Generic interfaces: `ApiResponse<T>`, `Paginated<T>`
- Generic constraints (`extends`)
- Conditional types (`T extends string ? X : Y`)
- Mapped types, Template literal types
- **Focus:** Generics ছাড়া real-world TypeScript হয় না

### Task 53 — TypeScript Utility Types Builder 🟡

- নিজে implement করো (built-in utility types):
  - `MyPartial<T>`, `MyRequired<T>`, `MyReadonly<T>`
  - `MyPick<T, K>`, `MyOmit<T, K>`, `MyExclude<T, U>`
  - `MyReturnType<T>`, `MyParameters<T>`
  - `MyRecord<K, V>`
- প্রতিটার use case demonstrate করো
- **Focus:** TypeScript type-level programming

### Task 54 — TypeScript with DOM 🟡

- Type-safe DOM manipulation:
  - `document.querySelector` with proper generic types
  - Event handler types (MouseEvent, KeyboardEvent, FormEvent)
  - Custom type guards for DOM elements
  - Type-safe event delegation
  - HTMLElement subtypes (HTMLInputElement, etc.)
- **Focus:** TypeScript + DOM interaction patterns

### Task 55 — TypeScript API Client 🟡

- Fully typed HTTP client:
  - Type-safe API endpoints (path + method → response type)
  - Generic fetch wrapper with error types
  - Zod schema validation for runtime type checking
  - Type-safe query parameters
  - Discriminated unions for API responses (success | error)
- **Tech:** TypeScript, Zod
- **Focus:** Real-world TypeScript patterns

### Task 56 — TypeScript Enum Alternatives & Patterns 🟡

- Const assertions (`as const`)
- Discriminated unions vs enums
- Exhaustive switch statements (never type)
- Builder pattern with TypeScript
- Branded/Opaque types (type-safe IDs)
- **Focus:** Advanced TypeScript patterns

### Task 57 — Type-Safe Event Emitter 🟡

- Event emitter with full type safety:
  - Event name → payload type mapping
  - `.on()`, `.off()`, `.emit()` fully typed
  - Autocomplete for event names
  - Type error if wrong payload passed
- **Focus:** Advanced generics real-world usage

### Task 58 — TypeScript Config Deep Dive 🟢

- `tsconfig.json` এর সব important options বোঝো ও configure করো:
  - `strict` mode ও এর sub-options
  - `paths` for module aliases
  - `target` ও `lib` options
  - `moduleResolution` (node vs bundler)
  - `declaration`, `sourceMap`
  - Project references for monorepo
- **Focus:** tsconfig.json mastery

---

## Section B: React Fundamentals (Tasks 59-72)

### Task 59 — React from Scratch Concepts 🟡

- React ছাড়া React-like rendering বোঝো:
  - Virtual DOM concept নিজে implement করো (simplified)
  - createElement function বানাও
  - Simple diff algorithm
  - Reconciliation concept বোঝো
  - তারপর React এর actual createElement দেখো
- **Focus:** React internally কিভাবে কাজ করে বোঝো

### Task 60 — Component Composition Patterns 🟢

- 10টা components বানাও proper composition দিয়ে:
  - `<Card>` with `<Card.Header>`, `<Card.Body>`, `<Card.Footer>`
  - `<Tabs>` with `<Tabs.Tab>`, `<Tabs.Panel>`
  - `<Accordion>` with `<Accordion.Item>`
  - Children prop, Render props, Compound components
  - Proper TypeScript types for all components
- **Focus:** Component composition — React এর core concept

### Task 61 — useState Deep Dive 🟢

- 8টা mini-projects with useState:
  1. Counter with step size
  2. Toggle group (multiple toggles, only one active)
  3. Form with multiple fields (single state object vs multiple states)
  4. Undo/Redo with state history
  5. Debounced input
  6. Shopping cart item quantity
  7. Color picker (RGB sliders)
  8. Lazy initialization demo (`useState(() => expensiveComputation())`)
- **Focus:** useState সব patterns বোঝো

### Task 62 — useEffect Mastery 🟡

- 8টা useEffect scenarios:
  1. Fetch data on mount
  2. Fetch data when dependency changes
  3. Cleanup: event listener
  4. Cleanup: timer/interval
  5. Cleanup: WebSocket connection
  6. Sync with localStorage
  7. Document title update
  8. Race condition handling (AbortController)
- **Focus:** useEffect lifecycle ও cleanup patterns

### Task 63 — useRef Deep Dive 🟡

- 6টা useRef use cases:
  1. Focus input on mount
  2. Scroll to element
  3. Previous value tracking
  4. Interval reference (start/stop timer)
  5. DOM measurement (element width/height)
  6. Uncontrolled form with useRef
- forwardRef + useImperativeHandle ব্যবহার করো
- **Focus:** useRef শুধু DOM ref না, mutable container

### Task 64 — useMemo & useCallback Deep Dive 🟡

- Performance optimization demos:
  1. Expensive calculation memoization (useMemo)
  2. List filtering memoization
  3. Callback memoization for child components (useCallback)
  4. React.memo with useCallback
  5. When NOT to use useMemo/useCallback (premature optimization)
  6. React DevTools Profiler দিয়ে re-renders measure করো
- **Focus:** কখন optimize করবে, কখন করবে না

### Task 65 — useReducer Complex State 🟡

- 4টা useReducer projects:
  1. Complex form state (multiple fields, validation, submission)
  2. Shopping cart (add, remove, update quantity, apply coupon)
  3. Multi-step wizard (next, prev, skip, validate)
  4. Undo/Redo system with action history
- Proper TypeScript types for actions ও state
- Discriminated union for action types
- **Focus:** Complex state management without libraries

### Task 66 — Context API Architecture 🟡

- Multi-context application:
  - ThemeContext (dark/light mode)
  - AuthContext (user, login, logout)
  - CartContext (items, add, remove)
  - NotificationContext (show, dismiss)
  - Context composition (nested providers vs single)
  - Performance issue ও solution (split contexts)
- **Focus:** Context API patterns ও limitations

### Task 67 — Custom Hooks Library 🟡

- 15টা production-quality custom hooks:
  - `useToggle`, `useCounter`
  - `useLocalStorage`, `useSessionStorage`
  - `useDebounce`, `useThrottle`
  - `useFetch` (with cache, retry, abort)
  - `useClickOutside`, `useHover`
  - `useMediaQuery`, `useWindowSize`
  - `useIntersectionObserver`
  - `usePrevious`, `useMount`
  - `useKeyPress`
- প্রতিটার demo page
- **Focus:** Custom hooks mastery — React developer এর key skill

### Task 68 — Controlled vs Uncontrolled Components 🟢

- Same form 2 ভাবে বানাও:
  1. Fully controlled (useState for every field)
  2. Fully uncontrolled (useRef)
  3. Mixed approach
- Performance comparison
- কখন কোনটা ব্যবহার করবে
- **Focus:** Controlled vs Uncontrolled deeply বোঝো

### Task 69 — React Event Handling Patterns 🟡

- Event handling deep dive:
  - Synthetic events vs native events
  - Event pooling (legacy) vs current behavior
  - Event delegation in React
  - Preventing default ও stopping propagation
  - Keyboard navigation (arrow keys, Enter, Escape)
  - Touch events for mobile
  - Custom event system within React
- **Focus:** React event system deeply বোঝো

### Task 70 — Error Boundaries & Error Handling 🟡

- Complete error handling system:
  - Class-based Error Boundary component
  - Different fallback UIs for different errors
  - Error logging service (mock)
  - Recovery mechanism (retry button)
  - Async error handling in event handlers
  - Error boundary with React Router
- **Focus:** Production-grade error handling in React

### Task 71 — React Portal Projects 🟡

- 4টা Portal use cases:
  1. Modal/Dialog system
  2. Tooltip that renders at document root
  3. Notification toasts (top-right corner)
  4. Context menu (right-click)
- Portal with event bubbling demo
- Accessible modals (focus trap, Escape to close)
- **Focus:** Portal কখন ও কেন ব্যবহার করবে

### Task 72 — Lists & Keys Deep Dive 🟡

- List rendering edge cases:
  - Why index as key is bad (demo with reordering)
  - Key-based state reset trick
  - Virtualized list (render only visible items) — নিজে implement করো
  - Large list performance (1000+ items)
  - Drag & drop list reordering
- **Focus:** Keys এর importance ও list performance

---

## Section C: React UI Projects (Tasks 73-86)

### Task 73 — Component Library Starter 🟡

- নিজের component library শুরু করো:
  - Button (variants: solid, outline, ghost, link | sizes: sm, md, lg)
  - Input (text, password with show/hide, search, with icon)
  - Select (custom dropdown, searchable)
  - Checkbox, Radio, Switch
  - All components: disabled state, loading state, error state
  - Storybook setup (or custom demo page)
- **Tech:** React, TypeScript, CSS Modules/Tailwind
- **Focus:** Reusable component design

### Task 74 — Responsive Dashboard 🟡

- Admin dashboard with:
  - Collapsible sidebar with nested menu items
  - Top navbar with user profile dropdown
  - Breadcrumbs navigation
  - Dashboard cards (stats)
  - Data table placeholder
  - Responsive: sidebar → bottom nav on mobile
- **Tech:** React, TypeScript, Tailwind

### Task 75 — Advanced Form System 🔴

- Form system with:
  - Dynamic form fields (add/remove)
  - Conditional fields (show field B if field A = "yes")
  - Multi-select with tags
  - Date range picker
  - File upload with preview
  - Form wizard (multi-step)
  - Real-time validation
- **Tech:** React Hook Form, Zod, TypeScript

### Task 76 — E-commerce Product Page 🟡

- Complete product page:
  - Image gallery with zoom (hover zoom effect)
  - Thumbnail carousel
  - Size/Color variant selector (changes image)
  - Quantity selector
  - Add to cart with animation
  - Product tabs (Description, Reviews, Specs)
  - Related products carousel
- **Tech:** React, TypeScript, Swiper

### Task 77 — Social Media Feed 🟡

- Instagram/Twitter-like feed:
  - Post card (image, text, user info, timestamp)
  - Like with heart animation
  - Comment section (expandable)
  - Share button with copy link
  - Infinite scroll
  - Post creation form (text + image)
  - Skeleton loading
- **Tech:** React, TypeScript

### Task 78 — Real-time Notification Center 🟡

- Notification system:
  - Bell icon with unread count badge
  - Dropdown notification list
  - Different notification types (info, success, warning, error, mention)
  - Mark as read / Mark all as read
  - Time ago format ("2 hours ago")
  - Notification sound (optional)
  - Notification preferences page
- **Tech:** React, TypeScript, date-fns

### Task 79 — Calendar Component from Scratch 🔴

- নিজে calendar component বানাও (library ছাড়া):
  - Month view with proper day grid
  - Navigate months (prev/next)
  - Year view
  - Today highlight
  - Selected date(s) highlight
  - Date range selection
  - Disabled dates
  - Events on dates (dots)
  - Keyboard navigation
- **Focus:** Complex component building from scratch

### Task 80 — Autocomplete/Combobox Component 🔴

- Production-quality autocomplete:
  - Type to search with debounce
  - Async data fetching
  - Keyboard navigation (up/down/enter/escape)
  - Highlighted matching text
  - Loading state
  - No results state
  - Multi-select mode
  - Custom option rendering
  - Accessible (ARIA combobox pattern)
- **Focus:** Complex accessible component

### Task 81 — Data Visualization Dashboard 🟡

- Charts dashboard:
  - Line chart (trend data)
  - Bar chart (comparison data)
  - Pie chart (distribution)
  - Area chart (stacked)
  - Tooltip on hover
  - Legend with toggle
  - Date range filter
  - Export chart as image
- **Tech:** React, Recharts, TypeScript

### Task 82 — Drag & Drop Kanban Board 🔴

- Trello clone:
  - Multiple columns (customizable)
  - Drag cards between columns
  - Drag to reorder within column
  - Add/Edit/Delete cards
  - Add/Edit/Delete columns
  - Card labels (color tags)
  - Card due date
  - Card assignee
  - Search cards
  - LocalStorage persistence
- **Tech:** React, @dnd-kit, TypeScript

### Task 83 — Rich Text Editor 🔴

- WYSIWYG editor:
  - Bold, Italic, Underline, Strikethrough
  - Headings (H1-H6)
  - Ordered/Unordered lists
  - Links (add/edit/remove)
  - Image embed
  - Code block with syntax highlight
  - Blockquote
  - Undo/Redo
  - HTML source view
  - Word/Character count
- **Tech:** React, Tiptap, TypeScript

### Task 84 — Image Gallery with Lightbox 🟡

- Pinterest-style gallery:
  - Masonry layout
  - Category filter with animation
  - Lightbox on click (prev/next/close)
  - Keyboard navigation in lightbox
  - Zoom in lightbox
  - Image lazy loading
  - Infinite scroll load more
  - Download button
- **Tech:** React, TypeScript, Framer Motion

### Task 85 — Music/Podcast Player 🟡

- Audio player application:
  - Playlist management (add, remove, reorder)
  - Play/Pause/Next/Previous
  - Progress bar with seek (click to jump)
  - Volume control with mute toggle
  - Shuffle ও Repeat modes
  - Current track info (title, artist, album art)
  - Mini player ও expanded player view
  - Keyboard shortcuts (space = play/pause)
- **Tech:** React, TypeScript, HTML5 Audio API

### Task 86 — Multi-Language Support System 🟡

- i18n system build করো:
  - Language switcher (English, Bangla, Arabic)
  - Translation files (JSON)
  - RTL support for Arabic
  - Pluralization support
  - Date/Number formatting per locale
  - Context-based translation provider
  - Dynamic language loading
- **Tech:** React, TypeScript, Custom i18n (no library first, then compare with i18next)

---

## Section D: React Patterns & Architecture (Tasks 87-100)

### Task 87 — Render Props Pattern 🟡

- 4টা render props component:
  1. `<Mouse>` — tracks mouse position
  2. `<Toggle>` — manages boolean state
  3. `<Fetch>` — handles data fetching
  4. `<Form>` — manages form state
- তারপর same functionality custom hooks এ convert করো
- **Focus:** Render props pattern ও evolution to hooks

### Task 88 — Higher-Order Components (HOC) 🟡

- 4টা HOC বানাও:
  1. `withAuth` — check if user is authenticated
  2. `withLoading` — show loading while data fetches
  3. `withTheme` — inject theme prop
  4. `withErrorBoundary` — wrap with error boundary
- TypeScript দিয়ে properly type করো (ComponentProps inference)
- **Focus:** HOC pattern বোঝো — legacy code এ দেখবে

### Task 89 — Compound Component Pattern 🟡

- 3টা compound component set:
  1. `<Select>` / `<Select.Option>` / `<Select.Group>`
  2. `<Accordion>` / `<Accordion.Item>` / `<Accordion.Trigger>` / `<Accordion.Content>`
  3. `<Menu>` / `<Menu.Item>` / `<Menu.Separator>` / `<Menu.SubMenu>`
- Context API ব্যবহার করো internal state share করতে
- **Focus:** Library-quality component APIs design করা

### Task 90 — State Machine Pattern with XState-like Logic 🔴

- State machines manually implement করো:
  1. Traffic light (red → green → yellow → red)
  2. Fetch machine (idle → loading → success/error)
  3. Multi-step form (step1 → step2 → step3 → submitted)
  4. Media player (stopped → playing → paused)
- Visualize state transitions
- **Focus:** State machines — complex UI logic manage করার best way

### Task 91 — Provider Pattern & Dependency Injection 🟡

- Multi-provider architecture:
  - Feature flags provider
  - Analytics provider
  - Configuration provider
  - A/B test provider
  - Combine all providers elegantly (avoid nesting hell)
  - Provider composition utility function
- **Focus:** Scalable provider architecture

### Task 92 — Container/Presentational Pattern 🟡

- Same feature 2 ভাবে:
  1. Traditional container/presentational split
  2. Modern hooks approach
- User dashboard feature:
  - Container: fetches data, handles logic
  - Presentational: pure UI, receives props
  - Compare: readability, testability, reusability
- **Focus:** Architecture patterns evolution বোঝো

### Task 93 — React Suspense Patterns 🔴

- Suspense deep dive:
  - Suspense for lazy loading components
  - Suspense for data fetching (React 18+)
  - Nested Suspense boundaries
  - SuspenseList for coordinated loading
  - Fallback UI design (skeletons, spinners)
  - Error boundary + Suspense combination
- **Focus:** Suspense architecture — future of React data loading

### Task 94 — Headless Component Pattern 🔴

- Headless (logic-only) components/hooks:
  - `useDropdown` — dropdown logic without UI
  - `useModal` — modal state management
  - `usePagination` — pagination logic
  - `useTable` — table sort/filter/pagination logic
  - Build UI separately that consumes these hooks
  - Same hook, different UIs
- **Focus:** Separation of logic and UI — key to reusable code

### Task 95 — React Performance Audit 🔴

- Performance problems তৈরি করো, তারপর fix করো:
  - Unnecessary re-renders detect করো (React DevTools)
  - Context splitting to prevent re-renders
  - Windowing/Virtualization for large lists
  - Code splitting with React.lazy
  - Image optimization
  - Bundle size analysis
  - Measure with Lighthouse
- **Focus:** Performance debugging ও optimization

### Task 96 — Micro-Frontend Concept Demo 🔴

- 2টা independent React apps যেটা একটা shell app এ compose হবে:
  - App 1: Product listing
  - App 2: Shopping cart
  - Shell: Navigation + renders both apps
  - Module Federation concept (Webpack) অথবা iframe approach
  - Shared state between micro-frontends
- **Focus:** Micro-frontend architecture concept

### Task 97 — Storybook Component Documentation 🟡

- Component library Storybook এ document করো:
  - Button, Input, Card, Modal components
  - Stories for each variant/state
  - Controls (knobs) for interactive props
  - Actions for event logging
  - Accessibility addon
  - MDX documentation pages
- **Tech:** Storybook, React, TypeScript
- **Focus:** Component documentation — team এ কাজ করতে essential

### Task 98 — React Strict Mode Deep Dive 🟡

- Strict Mode behaviors বোঝো ও demonstrate করো:
  - Double rendering in development
  - Double effect execution
  - Deprecated API warnings
  - Why strict mode catches bugs
  - Fix code that breaks under strict mode
  - useEffect cleanup importance
- **Focus:** React Strict Mode কেন important

### Task 99 — React 18+ Concurrent Features 🔴

- React 18+ features:
  - useTransition — non-urgent state updates
  - useDeferredValue — deferred rendering
  - Automatic batching demo
  - Concurrent rendering concept
  - startTransition usage
  - Large list filtering with useTransition (keep UI responsive)
- **Focus:** React concurrent features — interview favourite topic

### Task 100 — Full React Application Architecture 🔴

- Complete app with proper architecture:
  - Feature-based folder structure
  - Barrel exports (index.ts)
  - Shared components, hooks, utils, types
  - Route-based code splitting
  - Error boundaries per feature
  - Loading states management
  - Environment-based configuration
  - README with architecture documentation
- **Focus:** Production React app architecture — Phase 02 এর graduation project

---

## ✅ Phase 02 Completion Checklist

- [ ] TypeScript confidently লিখতে পারো (generics সহ)
- [ ] React Hooks সব পারো (useState, useEffect, useRef, useMemo, useCallback, useReducer)
- [ ] Custom hooks production-quality বানাতে পারো
- [ ] Component patterns সব জানো (composition, compound, headless, HOC, render props)
- [ ] Context API effectively ব্যবহার করতে পারো
- [ ] React performance optimize করতে পারো
- [ ] Error boundaries implement করতে পারো
- [ ] TypeScript + React confidently ব্যবহার করতে পারো
- [ ] Component library structure বোঝো
- [ ] React 18+ concurrent features জানো

> ✅ Ready? → **Phase 03: Advanced React (`Phase-03-Advanced-React.md`)** এ যাও
