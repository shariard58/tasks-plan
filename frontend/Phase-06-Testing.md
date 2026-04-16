# Phase 06 — Testing & Quality Assurance (Tasks 251-300)

> Testing ছাড়া production app চলে না। এই Phase এ তুমি testing expert হবে।
> 🟢 Easy | 🟡 Medium | 🔴 Hard | ⚫ Expert

---

## Section A: Unit Testing with Vitest (Tasks 251-264)

### Task 251 — Vitest Setup & Basics 🟢

- Vitest project setup:
  - Install ও configure Vitest
  - Test file conventions (_.test.ts, _.spec.ts)
  - describe, it/test, expect
  - Matchers (toBe, toEqual, toBeTruthy, toContain, toThrow)
  - beforeEach, afterEach, beforeAll, afterAll
  - Test grouping with describe
- **Tech:** Vitest

### Task 252 — Testing Pure Functions 🟢

- Unit test practice:
  - String utilities (capitalize, slugify, truncate)
  - Number utilities (clamp, round, currency format)
  - Array utilities (chunk, unique, flatten, groupBy)
  - Date utilities (formatDate, isToday, daysAgo)
  - Object utilities (pick, omit, deepClone, deepMerge)
  - Edge cases ও boundary values test করো
- **Focus:** Unit testing fundamentals

### Task 253 — Testing Async Code 🟡

- Async test patterns:
  - Testing Promises
  - Testing async/await functions
  - Testing setTimeout/setInterval (fake timers)
  - Testing fetch calls (mocking)
  - Testing error scenarios
  - Testing race conditions
- **Tech:** Vitest

### Task 254 — Mocking & Stubbing 🟡

- Mock strategies:
  - vi.fn() for function mocks
  - vi.mock() for module mocks
  - vi.spyOn() for method spying
  - Mock implementation
  - Mock return values (mockReturnValue, mockResolvedValue)
  - Mock localStorage, fetch, Date
  - Restore mocks (mockRestore, mockClear, mockReset)
- **Focus:** Mocking mastery

### Task 255 — Testing Custom Hooks 🟡

- Hook testing:
  - renderHook from @testing-library/react
  - Test useCounter hook
  - Test useLocalStorage hook
  - Test useFetch hook (with mock API)
  - Test useDebounce hook (with fake timers)
  - Test hook state changes (act())
  - Test hook cleanup
- **Tech:** @testing-library/react-hooks

### Task 256 — Snapshot Testing 🟢

- Snapshot patterns:
  - Component snapshot tests
  - Inline snapshots
  - Updating snapshots
  - When snapshot testing is useful vs not
  - Serializer customization
  - toMatchSnapshot vs toMatchInlineSnapshot
- **Focus:** Snapshot testing pros ও cons

### Task 257 — Testing TypeScript Code 🟡

- TypeScript-specific testing:
  - Testing generic functions
  - Testing type guards
  - Testing discriminated unions
  - expectTypeOf (type-level testing)
  - Testing error types
  - Testing utility types behavior
- **Tech:** Vitest, TypeScript

### Task 258 — Test Patterns & Anti-Patterns 🟡

- Good vs bad tests:
  - AAA pattern (Arrange, Act, Assert)
  - One assertion per test concept
  - Don't test implementation details
  - Test behavior, not structure
  - Avoid test interdependence
  - DRY vs readable tests
  - Test naming conventions
  - Test data factories
- **Focus:** Writing maintainable tests

### Task 259 — Code Coverage Deep Dive 🟡

- Coverage mastery:
  - Statement, branch, function, line coverage
  - Coverage reports (HTML, JSON, text)
  - Coverage thresholds configuration
  - Istanbul ignore hints
  - What 100% coverage means (and doesn't mean)
  - Critical path coverage vs total coverage
  - Coverage as CI gate
- **Focus:** Coverage strategy

### Task 260 — Testing Error Handling 🟡

- Error test scenarios:
  - Testing thrown errors
  - Testing rejected promises
  - Testing error boundaries
  - Testing validation errors
  - Testing network errors (fetch failure)
  - Testing timeout scenarios
  - Testing error messages
- **Focus:** Comprehensive error testing

### Task 261 — Testing State Management 🟡

- Store testing:
  - Testing Zustand stores
  - Testing Redux slices/reducers
  - Testing React Query hooks (wrapper)
  - Testing Context providers
  - Testing state transitions
  - Testing side effects in stores
  - Reset store between tests
- **Focus:** State management testing

### Task 262 — API Client Testing 🟡

- HTTP client tests:
  - Mock fetch/axios
  - Test request configuration (headers, body)
  - Test response handling
  - Test error handling (4xx, 5xx)
  - Test interceptors
  - Test retry logic
  - Test timeout
  - Test with MSW
- **Tech:** MSW, Vitest

### Task 263 — Testing Utilities & Factories 🟡

- Test infrastructure:
  - Test data factory (createMockUser, createMockPost)
  - Custom test matchers
  - Test helper functions
  - Shared test setup (setupFiles)
  - Custom render with providers
  - Test fixture management
- **Focus:** Scalable test infrastructure

### Task 264 — Property-Based Testing 🔴

- Generative testing:
  - fast-check library setup
  - Property: "sort should always return sorted array"
  - Property: "encode then decode equals original"
  - Property: "filter length <= original length"
  - Shrinking (finding minimal failing case)
  - When property-based > example-based
- **Tech:** fast-check

---

## Section B: Component Testing (Tasks 265-278)

### Task 265 — React Testing Library Setup 🟢

- RTL basics:
  - render, screen, cleanup
  - Queries: getByRole, getByText, getByLabelText, getByTestId
  - Priority: role > label > text > testid
  - userEvent for interactions
  - waitFor for async operations
  - within for scoped queries
- **Tech:** @testing-library/react

### Task 266 — Testing Basic Components 🟢

- Component test practice:
  - Button component (click, disabled, loading)
  - Input component (type, change, validation error)
  - Card component (renders children, props)
  - Badge component (variant, children)
  - Avatar component (image, fallback initials)
- **Focus:** Basic component testing patterns

### Task 267 — Testing Interactive Components 🟡

- User interaction tests:
  - Dropdown (open, select, close)
  - Modal (open, close, backdrop click, Escape)
  - Tabs (click, active state, keyboard navigation)
  - Accordion (expand, collapse)
  - Tooltip (hover to show, leave to hide)
  - Toggle/Switch (click to toggle)
- **Tech:** RTL, userEvent

### Task 268 — Testing Forms 🟡

- Form testing:
  - Fill form fields
  - Submit form
  - Validation errors display
  - Required field validation
  - Async validation (debounced)
  - Form reset
  - Multi-step form navigation
  - File upload input
  - React Hook Form testing
- **Focus:** Comprehensive form testing

### Task 269 — Testing with API Calls 🟡

- Component + API tests:
  - Mock API with MSW
  - Test loading state
  - Test success state (data displayed)
  - Test error state
  - Test empty state
  - Test refetch
  - Test mutation (create, update, delete)
  - Test optimistic updates
- **Tech:** MSW, RTL

### Task 270 — Testing React Router Components 🟡

- Router-dependent tests:
  - Custom render with MemoryRouter
  - Test navigation (Link click → URL change)
  - Test route params access
  - Test redirect behavior
  - Test protected routes
  - Test 404 page
  - Test breadcrumbs
- **Tech:** RTL, React Router

### Task 271 — Testing Context Consumers 🟡

- Context-aware tests:
  - Custom render with providers
  - Test theme toggle (ThemeContext)
  - Test auth-dependent UI (AuthContext)
  - Test cart operations (CartContext)
  - Multiple context providers in tests
  - Mock context values
- **Focus:** Testing components with dependencies

### Task 272 — Testing Animations 🟡

- Animation testing strategies:
  - Mock Framer Motion
  - Test animation triggers
  - Test animation completion callbacks
  - Test animated presence (mount/unmount)
  - Snapshot testing animated states
  - CSS transition testing
- **Tech:** RTL, Framer Motion

### Task 273 — Accessibility Testing 🟡

- Automated a11y testing:
  - jest-axe / vitest-axe for automated checks
  - Test keyboard navigation
  - Test ARIA attributes
  - Test focus management
  - Test screen reader text
  - Test color contrast (programmatic)
  - Test form accessibility
  - Create a11y test checklist
- **Tech:** vitest-axe, RTL
- **Focus:** Accessibility testing — increasingly required

### Task 274 — Visual Regression Testing 🟡

- Screenshot comparison:
  - Chromatic with Storybook
  - Percy/Applitools concept
  - Playwright screenshot comparison
  - Component visual snapshots
  - Responsive breakpoint screenshots
  - Dark mode vs light mode comparison
  - Accepting/Rejecting visual changes
- **Focus:** Visual regression — catches CSS bugs

### Task 275 — Testing Drag & Drop 🔴

- Complex interaction testing:
  - Simulate drag events
  - Test drag start, drag over, drop
  - Test reorder results
  - Test cross-container drag
  - @dnd-kit testing strategies
  - Keyboard-based drag & drop testing
- **Tech:** RTL, @dnd-kit

### Task 276 — Testing Data Tables 🟡

- Table component tests:
  - Render data correctly
  - Sorting (click header → sorted)
  - Filtering (type → filtered results)
  - Pagination (click next → new data)
  - Row selection (checkbox)
  - Column visibility toggle
  - Empty state
- **Tech:** RTL, TanStack Table

### Task 277 — Testing Charts 🟡

- Chart testing strategies:
  - Mock Recharts/Chart.js
  - Test chart renders with data
  - Test tooltip appears on hover
  - Test legend interaction
  - Test empty data state
  - Snapshot testing for chart SVG
- **Focus:** Testing non-trivial visual components

### Task 278 — Component Test Architecture 🟡

- Scalable test structure:
  - Test file organization
  - Shared test utilities
  - Custom render functions
  - Test data factories
  - Mock service modules
  - Test constants
  - CI configuration for tests
  - Testing documentation
- **Focus:** Maintainable test codebase

---

## Section C: Integration & E2E Testing (Tasks 279-292)

### Task 279 — Integration Testing Concepts 🟡

- Integration vs Unit vs E2E:
  - What is an integration test
  - Testing component + hook + context together
  - Testing feature (multiple components)
  - Testing user flows
  - When integration > unit tests
  - Testing trophy concept (by Kent C. Dodds)
- **Focus:** Testing strategy

### Task 280 — Playwright Setup & Basics 🟡

- E2E testing:
  - Playwright install ও configure
  - First test: navigate → click → assert
  - Locators (getByRole, getByText, locator)
  - Actions (click, fill, press, select)
  - Assertions (toBeVisible, toHaveText, toHaveURL)
  - Test configuration (browsers, devices)
- **Tech:** Playwright

### Task 281 — Playwright Page Object Model 🟡

- POM pattern:
  - Page class for each page
  - Methods for page actions
  - Locators as properties
  - Reusable across tests
  - Login page object
  - Dashboard page object
  - Navigation helpers
- **Tech:** Playwright
- **Focus:** Maintainable E2E tests

### Task 282 — E2E: Authentication Flow 🟡

- Test complete auth:
  - Register new user
  - Login with credentials
  - Access protected page
  - Logout
  - Invalid credentials error
  - Session persistence (refresh page)
  - Password reset flow
- **Tech:** Playwright

### Task 283 — E2E: E-commerce User Journey 🔴

- Test shopping flow:
  - Browse products
  - Filter products
  - Add to cart
  - Update cart quantity
  - Remove from cart
  - Checkout form
  - Order confirmation
  - Order history
- **Tech:** Playwright

### Task 284 — E2E: Form Submissions 🟡

- Test complex forms:
  - Multi-step form completion
  - Validation error scenarios
  - File upload
  - Dynamic form fields
  - Form autosave
  - Draft saving
- **Tech:** Playwright

### Task 285 — E2E: Responsive Testing 🟡

- Cross-device testing:
  - Desktop viewport tests
  - Tablet viewport tests
  - Mobile viewport tests
  - Navigation changes per breakpoint
  - Touch interactions (mobile)
  - Orientation change
- **Tech:** Playwright

### Task 286 — E2E: API Mocking in Playwright 🟡

- Network interception:
  - Mock API responses
  - Test error scenarios (500, 404)
  - Test slow network
  - Test offline scenario
  - Record ও replay API calls
  - Conditional mocking
- **Tech:** Playwright

### Task 287 — E2E: Visual Testing with Playwright 🟡

- Screenshot testing:
  - Full page screenshots
  - Component screenshots
  - Compare screenshots
  - Threshold configuration
  - Update snapshots
  - CI integration
- **Tech:** Playwright

### Task 288 — E2E: Performance Testing 🟡

- Performance in E2E:
  - Measure page load time
  - Measure Time to Interactive
  - Check for layout shifts
  - Lighthouse CI integration
  - Performance budgets
  - Trace viewer analysis
- **Tech:** Playwright, Lighthouse CI

### Task 289 — Cypress Alternative Exploration 🟡

- Cypress vs Playwright:
  - Same tests in Cypress
  - Component testing in Cypress
  - Cypress Studio (visual test creation)
  - Compare: DX, speed, features, debugging
  - Document comparison findings
- **Tech:** Cypress
- **Focus:** Know both major E2E tools

### Task 290 — API Testing 🟡

- Backend API testing:
  - Supertest for Next.js API routes
  - Test CRUD endpoints
  - Test authentication middleware
  - Test validation
  - Test error responses
  - Test rate limiting
- **Tech:** Supertest / Vitest

### Task 291 — Load Testing Basics 🟡

- Performance under load:
  - k6 বা Artillery setup
  - Basic load test script
  - Measure response times under load
  - Identify bottlenecks
  - Stress test (find breaking point)
  - Generate load test report
- **Tech:** k6 / Artillery
- **Focus:** Performance testing awareness

### Task 292 — Testing in CI/CD 🟡

- Automated testing pipeline:
  - Unit tests in GitHub Actions
  - Integration tests in CI
  - E2E tests in CI (Playwright)
  - Test parallelization
  - Test result artifacts
  - Flaky test detection
  - Test coverage reporting
  - PR status checks
- **Tech:** GitHub Actions, Vitest, Playwright

---

## Section D: Quality Assurance (Tasks 293-300)

### Task 293 — ESLint Custom Rules 🟡

- Lint rules:
  - Create custom ESLint rule
  - eslint-plugin-react-hooks rules
  - Import order rules
  - No unused imports
  - Naming conventions
  - Complexity limits
  - Custom rule for project conventions
- **Tech:** ESLint

### Task 294 — Type-Level Testing 🟡

- TypeScript type testing:
  - expectTypeOf assertions
  - Test utility types
  - Test function signatures
  - Test generic constraints
  - Ensure types are correct at compile time
- **Tech:** Vitest expectTypeOf / tsd

### Task 295 — Storybook Interaction Testing 🟡

- Storybook tests:
  - play function for interactions
  - Test component states in stories
  - Chromatic for visual testing
  - A11y addon tests
  - Test coverage from Storybook
- **Tech:** Storybook, @storybook/test

### Task 296 — Error Monitoring Setup 🟡

- Production error tracking:
  - Sentry SDK integration
  - Error boundary + Sentry
  - Source maps for readable stack traces
  - Error grouping ও alerts
  - Performance monitoring
  - User feedback on error
  - Release tracking
- **Tech:** Sentry

### Task 297 — Code Quality Metrics 🟡

- Quality measurement:
  - SonarQube/SonarCloud setup
  - Code smell detection
  - Duplicate code detection
  - Cognitive complexity
  - Technical debt tracking
  - Quality gate configuration
- **Tech:** SonarQube / SonarCloud

### Task 298 — Pre-commit Quality Hooks 🟡

- Git hooks:
  - Husky setup
  - lint-staged (lint only staged files)
  - Pre-commit: lint + type check
  - Pre-push: run tests
  - Commitlint (conventional commits)
  - Auto-fix on commit
- **Tech:** Husky, lint-staged, commitlint

### Task 299 — Testing Documentation 🟡

- Document testing strategy:
  - Testing philosophy document
  - What to test ও what not to test
  - Test naming conventions
  - Test file organization
  - Mock strategy document
  - CI testing configuration
  - Coverage goals
- **Focus:** Team-level testing documentation

### Task 300 — Complete Test Suite for Production App ⚫

- Take one Phase 04 project ও add complete tests:
  - 80%+ code coverage
  - Unit tests for utilities ও hooks
  - Component tests for all UI components
  - Integration tests for features
  - E2E tests for critical user journeys
  - Accessibility tests
  - Performance tests
  - CI pipeline with all tests
  - Testing report
- **Focus:** Phase 06 graduation — testing mastery

---

## ✅ Phase 06 Completion Checklist

- [ ] Vitest confidently ব্যবহার করতে পারো
- [ ] React Testing Library ভালো পারো
- [ ] Custom hooks test করতে পারো
- [ ] MSW দিয়ে API mock করতে পারো
- [ ] Playwright E2E tests লিখতে পারো
- [ ] Accessibility testing পারো
- [ ] CI/CD তে tests run করতে পারো
- [ ] Test strategy design করতে পারো
- [ ] 80%+ coverage achieve করতে পারো
- [ ] Testing documentation লিখতে পারো

> ✅ Ready? → **Phase 07: Performance & Optimization (`Phase-07-Performance.md`)** এ যাও
