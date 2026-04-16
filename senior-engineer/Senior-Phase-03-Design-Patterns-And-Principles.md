# Senior Skills Phase 03 — Software Engineering Principles & Design Patterns (Tasks 81-130)

> **Code কাজ করলেই হলো না — code CLEAN, MAINTAINABLE, EXTENSIBLE হতে হবে।**
> **SOLID, Design Patterns, Clean Code — এগুলো জানলে তোমার code professional level।**
> **Interview এ design patterns জিজ্ঞেস করবে। Production এ Clean Code লাগবে। দুটোই শেখো।**

---

## Section A: Clean Code (Tasks 81-95)

### Task 81 — Clean Code Principles 🟢

- কেন Clean Code গুরুত্বপূর্ণ: "Code is read more than it is written"
- Read "Clean Code" by Robert C. Martin (Chapter 1-3 at minimum)
- **Exercise:** 5 টা "dirty code" example নাও, clean করো

### Task 82 — Meaningful Names 🟢

- Variables: `userData` না `d`, `isActive` না `flag`
- Functions: verb + noun — `getUserById()`, `calculateTotalPrice()`
- Classes: noun — `UserRepository`, `PaymentService`
- Constants: UPPER_SNAKE — `MAX_RETRY_COUNT`, `API_BASE_URL`
- **Exercise:** Bad naming এর 20 টা example fix করো

### Task 83 — Functions — Small & Single Purpose 🟡

- Each function does ONE thing, does it well, does it only
- Max 20 lines per function, max 3 parameters
- ```typescript
  // ❌ Bad: does too many things
  function processOrder(order) {
    validateOrder(order);
    calculateTotal(order);
    chargePayment(order);
    sendEmail(order);
    updateInventory(order);
  }

  // ✅ Good: orchestrates single-purpose functions
  function processOrder(order) {
    const validated = validateOrder(order);
    const total = calculateTotal(validated);
    const payment = await chargePayment(validated, total);
    await Promise.all([sendConfirmation(validated, payment), updateInventory(validated)]);
  }
  ```

- **Exercise:** 10 টা long function refactor করো small functions এ

### Task 84 — Comments — When ও How 🟢

- Good comments: WHY, not WHAT (code should explain WHAT)
- Bad comments: commented-out code, obvious comments, misleading
- ```typescript
  // ❌ Bad: obvious comment
  // Get user by ID
  function getUserById(id) { ... }

  // ✅ Good: explains WHY
  // We retry 3 times because the payment gateway has intermittent 503 errors
  const MAX_RETRIES = 3;
  ```

- **Exercise:** একটা project এর সব bad comments remove/improve করো

### Task 85 — Error Handling 🟡

- Don't return null, throw meaningful errors
- Error classes hierarchy, error codes
- Never catch ও ignore errors silently
- **Exercise:** Error handling system with custom errors ও proper logging

### Task 86 — Code Smells 🟡

- Long method, Large class, God object, Feature envy, Data clumps
- Primitive obsession, Switch statements, Parallel inheritance
- Shotgun surgery, Divergent change
- **Exercise:** Identify code smells in a codebase, document ও fix 10 smells

### Task 87 — Refactoring Techniques 🟡

- Extract method, Extract class, Inline, Move method
- Replace conditional with polymorphism
- Replace magic numbers with constants
- Introduce parameter object, Preserve whole object
- **Exercise:** Refactor a messy module using 10 different techniques

### Task 88 — DRY (Don't Repeat Yourself) 🟢

- Duplication = bug factory. Extract shared logic
- But: wrong abstraction > duplication (AHA: Avoid Hasty Abstractions)
- **Exercise:** Find duplication in code, extract shared functions/classes

### Task 89 — KISS (Keep It Simple, Stupid) 🟢

- Simplest solution that works
- Over-engineering is the enemy
- **Exercise:** Over-engineered code example simplify করো

### Task 90 — YAGNI (You Aren't Gonna Need It) 🟢

- Don't build for hypothetical future requirements
- Build what you need NOW
- **Exercise:** Identify YAGNI violations in code, remove unnecessary abstractions

### Task 91 — Separation of Concerns 🟡

- Each module/class/function handles ONE concern
- Layers: Controller → Service → Repository → Database
- **Exercise:** Refactor a mixed-concern module into proper layers

### Task 92 — Immutability ও Pure Functions 🟡

- Pure functions: same input → same output, no side effects
- Immutable data: never mutate, always create new
- **Exercise:** Refactor mutable code to immutable patterns

### Task 93 — Defensive Programming 🟡

- Validate inputs at boundaries, assert invariants
- Fail fast, fail loudly
- **Exercise:** Add defensive checks to a module's public API

### Task 94 — Code Formatting ও Consistency 🟢

- Prettier, ESLint, EditorConfig — automate formatting
- Team coding standards document
- **Exercise:** Setup: Prettier + ESLint + EditorConfig for team project

### Task 95 — Technical Debt 🟡

- What it is, how it accumulates, when to pay it off
- Tech debt tracking: document ও prioritize
- **Exercise:** Audit a codebase for technical debt, create payoff plan

---

## Section B: SOLID Principles (Tasks 96-110)

### Task 96 — Single Responsibility Principle (SRP) 🟡

- A class should have only ONE reason to change
- ```typescript
  // ❌ Bad: UserService does everything
  class UserService {
    createUser() { ... }
    sendEmail() { ... }      // Email concern
    generateReport() { ... } // Report concern
    logActivity() { ... }    // Logging concern
  }

  // ✅ Good: Each class = one responsibility
  class UserService { createUser() { ... } }
  class EmailService { sendEmail() { ... } }
  class ReportService { generateReport() { ... } }
  class ActivityLogger { logActivity() { ... } }
  ```

- **Exercise:** একটা God class কে SRP follow করে break করো

### Task 97 — Open/Closed Principle (OCP) 🟡

- Open for extension, Closed for modification
- ```typescript
  // ❌ Bad: modify existing code for new payment
  class PaymentProcessor {
    process(type: string) {
      if (type === "stripe") { ... }
      else if (type === "paypal") { ... }
      // Have to modify this file for every new payment type!
    }
  }

  // ✅ Good: extend without modifying
  interface PaymentGateway {
    process(amount: number): Promise<Result>;
  }
  class StripeGateway implements PaymentGateway { ... }
  class PayPalGateway implements PaymentGateway { ... }
  class BkashGateway implements PaymentGateway { ... } // NEW: no existing code changed!
  ```

- **Exercise:** if/else chain কে OCP follow করে refactor করো

### Task 98 — Liskov Substitution Principle (LSP) 🟡

- Subtypes must be substitutable for their base types
- ```typescript
  // ❌ Bad: Square violates Rectangle contract
  class Rectangle {
    setWidth(w: number) { this.width = w; }
    setHeight(h: number) { this.height = h; }
  }
  class Square extends Rectangle {
    setWidth(w: number) { this.width = w; this.height = w; } // VIOLATES LSP!
  }

  // ✅ Good: separate abstractions
  interface Shape { area(): number; }
  class Rectangle implements Shape { ... }
  class Square implements Shape { ... }
  ```

- **Exercise:** LSP violation খুঁজে বের করো ও fix করো

### Task 99 — Interface Segregation Principle (ISP) 🟡

- Clients should not depend on interfaces they don't use
- ```typescript
  // ❌ Bad: fat interface
  interface Worker {
    work(): void;
    eat(): void;
    sleep(): void;
  }
  // Robot has to implement eat() and sleep()? 🤔

  // ✅ Good: segregated interfaces
  interface Workable { work(): void; }
  interface Eatable { eat(): void; }
  interface Sleepable { sleep(): void; }

  class Human implements Workable, Eatable, Sleepable { ... }
  class Robot implements Workable { ... } // Only what it needs
  ```

- **Exercise:** Fat interface কে segregate করো

### Task 100 — Dependency Inversion Principle (DIP) 🔴

- High-level modules should NOT depend on low-level modules. Both depend on abstractions.
- ```typescript
  // ❌ Bad: directly depends on PostgreSQL
  class UserService {
    private db = new PostgreSQLDatabase(); // Tightly coupled!
  }

  // ✅ Good: depends on abstraction
  interface UserRepository {
    findById(id: string): Promise<User>;
    save(user: User): Promise<void>;
  }
  class UserService {
    constructor(private repo: UserRepository) {} // Inject abstraction
  }
  // Can use PostgreSQL, MongoDB, InMemory — UserService doesn't care
  ```

- **Exercise:** Dependency Injection system build করো

### Task 101 — Composition Over Inheritance 🟡

- Prefer composing objects over extending classes
- ```typescript
  // ❌ Inheritance chain: Animal → Dog → GuideDog → ... (fragile)

  // ✅ Composition
  class Dog {
    constructor(
      private movement: WalkBehavior,
      private sound: BarkBehavior
    ) {}
  }
  ```

- **Exercise:** Deep inheritance hierarchy কে composition এ convert করো

### Task 102 — Law of Demeter (Principle of Least Knowledge) 🟡

- Don't talk to strangers: `a.b.c.d()` = bad
- **Exercise:** Law of Demeter violations fix করো

### Task 103 — Tell, Don't Ask 🟡

- Tell objects what to do, don't ask for their state
- ```typescript
  // ❌ Ask: get state, make decision outside
  if (user.getBalance() >= amount) {
    user.setBalance(user.getBalance() - amount);
  }

  // ✅ Tell: let object decide
  user.charge(amount); // User internally handles balance check
  ```

- **Exercise:** "Ask" code কে "Tell" pattern এ refactor করো

### Task 104 — Coupling ও Cohesion 🟡

- Low coupling (modules independent), High cohesion (related things together)
- **Exercise:** Coupling/cohesion analysis of a codebase

### Task 105 — SOLID in Real Projects 🔴

- Apply ALL 5 principles to a real project
- **Exercise:** Take an existing project, audit ও refactor for SOLID compliance

### Task 106 — Dependency Injection Container 🔴

- IoC container: auto-resolve dependencies
- Libraries: tsyringe, inversify, or manual DI
- **Exercise:** Build DI container from scratch, then use a library

### Task 107 — Repository Pattern 🟡

- Abstract data access behind repository interface
- **Exercise:** Repository pattern for User, Product, Order

### Task 108 — Service Layer Pattern 🟡

- Business logic in services, separate from controllers ও data access
- **Exercise:** Service layer with proper error handling

### Task 109 — Unit of Work Pattern 🔴

- Group multiple operations into single transaction
- **Exercise:** Unit of Work for multi-table operations

### Task 110 — SOLID Audit Tool ⚫

- **Exercise:** Create checklist/tool to audit any project for SOLID violations

---

## Section C: Design Patterns — GoF (Tasks 111-130)

### Task 111 — Singleton Pattern 🟢

- Single instance globally: DB connection, Logger, Config
- ```typescript
  class Database {
    private static instance: Database;
    private constructor() {} // private constructor
    static getInstance(): Database {
      if (!Database.instance) Database.instance = new Database();
      return Database.instance;
    }
  }
  ```
- Pitfalls: testing difficulty, hidden dependencies
- **Exercise:** Singleton for config manager ও database connection

### Task 112 — Factory Method Pattern 🟡

- Create objects without specifying exact class
- ```typescript
  interface Notification { send(message: string): void; }
  class EmailNotification implements Notification { ... }
  class SMSNotification implements Notification { ... }
  class PushNotification implements Notification { ... }

  class NotificationFactory {
    static create(type: "email" | "sms" | "push"): Notification {
      switch (type) {
        case "email": return new EmailNotification();
        case "sms": return new SMSNotification();
        case "push": return new PushNotification();
      }
    }
  }
  ```

- **Exercise:** Factory for payment gateways ও notification channels

### Task 113 — Abstract Factory Pattern 🟡

- Factory of factories — create families of related objects
- **Exercise:** UI theme factory (Light ও Dark theme with matching components)

### Task 114 — Builder Pattern 🟡

- Step-by-step construction of complex objects
- ```typescript
  const query = new QueryBuilder()
    .select('name', 'email')
    .from('users')
    .where('age', '>', 18)
    .orderBy('name')
    .limit(10)
    .build();
  ```
- **Exercise:** Builder for Query, HTTPRequest, Configuration

### Task 115 — Prototype Pattern 🟢

- Clone existing objects instead of creating new
- **Exercise:** Deep clone system for complex objects

### Task 116 — Adapter Pattern 🟡

- Convert interface of one class to another expected interface
- ```typescript
  // Old system uses XML, new system expects JSON
  class XMLToJSONAdapter implements JSONDataSource {
    constructor(private xmlSource: XMLDataSource) {}
    getData(): JSON {
      const xml = this.xmlSource.getXMLData();
      return this.convertXMLtoJSON(xml);
    }
  }
  ```
- **Exercise:** Adapter for different payment gateways (unified interface)

### Task 117 — Decorator Pattern 🟡

- Add behavior to objects dynamically without modifying them
- ```typescript
  // Base: simple logger
  // Decorator: add timestamp, add color, add file output
  const logger = new FileLogger(new TimestampLogger(new ColorLogger(new ConsoleLogger())));
  ```
- **Exercise:** Decorator chain for logging, caching, validation

### Task 118 — Facade Pattern 🟢

- Simple interface to complex subsystem
- **Exercise:** Facade for email sending (SMTP, templates, queuing, tracking)

### Task 119 — Proxy Pattern 🟡

- Control access to an object (caching proxy, logging proxy, access control proxy)
- **Exercise:** Caching proxy for API calls

### Task 120 — Composite Pattern 🟡

- Tree structure: treat individual objects ও compositions uniformly
- **Exercise:** File system with files ও directories (both implement same interface)

### Task 121 — Bridge Pattern 🟡

- Separate abstraction from implementation
- **Exercise:** Bridge between notification types ও delivery channels

### Task 122 — Observer Pattern 🟡

- When one object changes, all dependents are notified
- ```typescript
  class EventEmitter {
    private listeners = new Map<string, Function[]>();
    on(event: string, fn: Function) { ... }
    emit(event: string, data: any) { ... }
  }
  ```
- **Exercise:** Type-safe event system with Observer pattern

### Task 123 — Strategy Pattern 🟡

- Define family of algorithms, encapsulate each, make interchangeable
- ```typescript
  interface SortStrategy { sort<T>(data: T[]): T[]; }
  class QuickSort implements SortStrategy { ... }
  class MergeSort implements SortStrategy { ... }

  class Sorter {
    constructor(private strategy: SortStrategy) {}
    sort<T>(data: T[]): T[] { return this.strategy.sort(data); }
  }
  ```

- **Exercise:** Strategy for pricing, sorting, validation, authentication

### Task 124 — Command Pattern 🟡

- Encapsulate request as object (undo/redo, queue, log)
- **Exercise:** Command pattern with undo/redo for text editor

### Task 125 — State Pattern 🟡

- Object behavior changes based on internal state
- **Exercise:** State machine for order (draft → pending → paid → shipped → delivered)

### Task 126 — Template Method Pattern 🟡

- Define algorithm skeleton, let subclasses override steps
- **Exercise:** Template for data import (read → validate → transform → save)

### Task 127 — Iterator Pattern 🟢

- Sequential access to elements without exposing internals
- **Exercise:** Custom iterator for paginated API results

### Task 128 — Chain of Responsibility 🟡

- Pass request along chain of handlers (middleware pattern!)
- **Exercise:** Validation chain, Middleware chain, Approval chain

### Task 129 — Mediator Pattern 🟡

- Reduce direct communication between objects — go through mediator
- **Exercise:** Chat room mediator between users

### Task 130 — Build: Design Patterns Library ⚫

- **Phase 3 Final Project:**
  - Implement ALL 20+ design patterns in TypeScript
  - Each pattern: explanation, code, real-world use case, tests
  - Documentation: when to use, when NOT to use, pros/cons
  - Real application: Apply 10+ patterns to a single project (E-commerce)
    - Factory: create products
    - Strategy: pricing/discount algorithms
    - Observer: order status notifications
    - State: order state machine
    - Builder: complex query builder
    - Decorator: middleware chain
    - Repository: data access
    - Adapter: payment gateways

---

## ✅ Phase 3 Completion Checklist

- [ ] সব 50 tasks complete
- [ ] Clean Code principles follow করে code লিখতে পারো
- [ ] SOLID principles explain ও apply করতে পারো
- [ ] 20+ Design Patterns implement করতে পারো TypeScript এ
- [ ] Code smell identify ও refactor করতে পারো
- [ ] Interview এ design patterns confidently answer করতে পারো

> _"Clean Code + SOLID + Patterns = Professional Developer। এগুলো জানলে তোমার code দেখেই বোঝা যাবে — এই লোক experienced।"_
