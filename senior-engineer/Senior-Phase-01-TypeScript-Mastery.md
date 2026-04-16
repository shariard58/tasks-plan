# Senior Skills Phase 01 — TypeScript Mastery (Tasks 1-50)

> **2026 এ TypeScript ছাড়া professional project নেই। JavaScript জানো, এখন TypeScript master করো।**
> **TypeScript = JavaScript + Types। এটা জানলে bug 50% কমবে, productivity 2x বাড়বে।**
> **React, Node.js, Express — সব TypeScript এ লেখা হয় এখন। এটা optional না, mandatory।**

---

## Section A: TypeScript Fundamentals (Tasks 1-15)

### Task 1 — TypeScript কেন? Setup ও First Program 🟢

- JavaScript এর problem: runtime errors, no intellisense, no type safety
- TypeScript = compile-time type checking + better DX
- Setup: `npm init -y && npm i -D typescript @types/node ts-node`
- tsconfig.json basic configuration
- **Exercise:** TypeScript project setup, first `.ts` file compile ও run করো

### Task 2 — Basic Types 🟢

- `string`, `number`, `boolean`, `null`, `undefined`, `void`, `never`, `any`, `unknown`
- Type annotations vs Type inference
- ```typescript
  let name: string = 'Rahim'; // annotation
  let age = 25; // inference (TypeScript জানে এটা number)
  let data: unknown = fetchData(); // unknown = safe any
  ```
- **Exercise:** 20 টা variable declare করো different types দিয়ে

### Task 3 — Arrays ও Tuples 🟢

- ```typescript
  let numbers: number[] = [1, 2, 3];
  let names: Array<string> = ['a', 'b'];
  let tuple: [string, number] = ['Rahim', 25]; // fixed length + types
  let readonly_arr: readonly number[] = [1, 2]; // immutable
  ```
- **Exercise:** Array ও Tuple ব্যবহার করে student data manage করো

### Task 4 — Objects ও Type Aliases 🟢

- ```typescript
  type User = {
    id: number;
    name: string;
    email: string;
    isActive?: boolean; // optional
    readonly createdAt: Date; // read-only
  };
  ```
- Optional properties (`?`), readonly, nested objects
- **Exercise:** Complex object types: User, Product, Order

### Task 5 — Interfaces 🟢

- ```typescript
  interface IUser {
    id: number;
    name: string;
    greet(): string;
  }
  // Interface extends
  interface IAdmin extends IUser {
    role: string;
    permissions: string[];
  }
  ```
- Interface vs Type Alias: কখন কোনটা use করবে
- **Exercise:** Interface hierarchy: IAnimal → IDog → IGuideDog

### Task 6 — Union ও Intersection Types 🟡

- ```typescript
  // Union: either this OR that
  type Status = 'active' | 'inactive' | 'banned';
  type ID = string | number;

  // Intersection: this AND that
  type Employee = Person & Worker;
  // Has all properties of both Person and Worker
  ```

- **Exercise:** Type-safe API response: `Success<T> | Error`

### Task 7 — Literal Types ও Type Narrowing 🟡

- ```typescript
  type Direction = 'up' | 'down' | 'left' | 'right';

  function move(direction: Direction) {
    // TypeScript knows direction can ONLY be these 4 values
  }

  // Type narrowing
  function process(value: string | number) {
    if (typeof value === 'string') {
      // TypeScript knows it's string here
      value.toUpperCase();
    } else {
      // TypeScript knows it's number here
      value.toFixed(2);
    }
  }
  ```

- **Exercise:** Type-safe event handler with narrowing

### Task 8 — Enums 🟢

- ```typescript
  enum Role {
    Admin = 'ADMIN',
    User = 'USER',
    Moderator = 'MOD',
  }
  // vs const object (preferred in many cases)
  const ROLE = {
    Admin: 'ADMIN',
    User: 'USER',
  } as const;
  type Role = (typeof ROLE)[keyof typeof ROLE];
  ```
- Numeric vs String enums, `const enum`, `as const` alternative
- **Exercise:** Enum vs `as const` comparison — pros/cons

### Task 9 — Functions with TypeScript 🟡

- ```typescript
  // Parameter types + return type
  function add(a: number, b: number): number {
    return a + b;
  }
  // Optional + default parameters
  function greet(name: string, greeting?: string): string {
    return `${greeting || 'Hello'}, ${name}`;
  }
  // Function type
  type MathFn = (a: number, b: number) => number;
  const multiply: MathFn = (a, b) => a * b;

  // Overloads
  function parse(input: string): number;
  function parse(input: number): string;
  function parse(input: string | number) {
    return typeof input === 'string' ? parseInt(input) : input.toString();
  }
  ```

- **Exercise:** Type-safe utility functions: map, filter, reduce with proper types

### Task 10 — Generics Basics 🟡

- ```typescript
  // Without generics: loses type info
  function first(arr: any[]): any {
    return arr[0];
  }

  // With generics: preserves type
  function first<T>(arr: T[]): T | undefined {
    return arr[0];
  }

  const num = first([1, 2, 3]); // TypeScript knows: number
  const str = first(['a', 'b']); // TypeScript knows: string

  // Generic interface
  interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
  }
  ```

- **Exercise:** Generic Stack, Queue, LinkedList classes

### Task 11 — Classes with TypeScript 🟡

- ```typescript
  class User {
    // Access modifiers
    constructor(
      public id: number,
      public name: string,
      private password: string,
      protected email: string,
      readonly createdAt: Date = new Date()
    ) {}

    // Abstract class
  }
  abstract class Shape {
    abstract area(): number;
    perimeter?(): number;
  }
  ```
- public, private, protected, readonly
- Abstract classes, implements interface
- **Exercise:** Class hierarchy: Shape → Circle, Rectangle, Triangle

### Task 12 — Type Assertions ও Type Guards 🟡

- ```typescript
  // Type assertion (তুমি TypeScript কে বলছো "আমি জানি এটা কী")
  const input = document.getElementById('name') as HTMLInputElement;

  // Custom type guard
  function isUser(obj: unknown): obj is User {
    return typeof obj === 'object' && obj !== null && 'id' in obj && 'name' in obj;
  }

  // Discriminated union
  type Shape = { kind: 'circle'; radius: number } | { kind: 'square'; side: number };

  function area(shape: Shape) {
    switch (shape.kind) {
      case 'circle':
        return Math.PI * shape.radius ** 2;
      case 'square':
        return shape.side ** 2;
    }
  }
  ```

- **Exercise:** Type guards for API response validation

### Task 13 — Modules ও Namespaces 🟢

- ES Modules with TypeScript: import/export types
- `import type { User } from './types'` — type-only imports
- Declaration merging
- **Exercise:** Organize a project with proper module structure

### Task 14 — tsconfig.json Deep Dive 🟡

- `target`, `module`, `moduleResolution`, `strict`, `paths`, `baseUrl`
- `strict` mode options: `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`
- Project references for monorepo
- **Exercise:** tsconfig for Node.js backend vs React frontend vs Library

### Task 15 — Error Handling in TypeScript 🟡

- ```typescript
  // Result type pattern (no more try-catch everywhere)
  type Result<T, E = Error> = { success: true; data: T } | { success: false; error: E };

  async function fetchUser(id: number): Promise<Result<User>> {
    try {
      const user = await db.findUser(id);
      return { success: true, data: user };
    } catch (e) {
      return { success: false, error: e as Error };
    }
  }
  ```

- Custom error classes, error types
- **Exercise:** Type-safe error handling system

---

## Section B: Advanced TypeScript (Tasks 16-35)

### Task 16 — Advanced Generics 🔴

- ```typescript
  // Constraints
  function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
  }

  // Default generic types
  interface PaginatedResult<T, M = { total: number }> {
    data: T[];
    meta: M;
  }

  // Generic with multiple type params
  function merge<T extends object, U extends object>(a: T, b: U): T & U {
    return { ...a, ...b };
  }
  ```

- **Exercise:** Generic Repository class with CRUD operations

### Task 17 — Conditional Types 🔴

- ```typescript
  // T extends U ? X : Y
  type IsString<T> = T extends string ? true : false;

  type A = IsString<'hello'>; // true
  type B = IsString<42>; // false

  // Extract / Exclude
  type NonNullable<T> = T extends null | undefined ? never : T;

  // infer keyword
  type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
  type UnpackPromise<T> = T extends Promise<infer U> ? U : T;
  ```

- **Exercise:** Build your own conditional utility types

### Task 18 — Mapped Types 🔴

- ```typescript
  // Transform all properties
  type Readonly<T> = { readonly [K in keyof T]: T[K] };
  type Optional<T> = { [K in keyof T]?: T[K] };
  type Nullable<T> = { [K in keyof T]: T[K] | null };

  // Key remapping
  type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
  };
  // { getName: () => string, getAge: () => number }
  ```

- **Exercise:** Build custom mapped types for API layer

### Task 19 — Template Literal Types 🔴

- ```typescript
  type EventName = `on${Capitalize<string>}`;
  // "onClick", "onHover", "onSubmit", etc.

  type CSSProperty = `${string}-${string}`;

  // Combine with mapped types
  type EventHandlers<T> = {
    [K in keyof T as `on${Capitalize<string & K>}Change`]: (value: T[K]) => void;
  };
  ```

- **Exercise:** Type-safe event system with template literals

### Task 20 — Utility Types Deep Dive 🟡

- ```typescript
  // Built-in utility types
  Partial<T>; // All props optional
  Required<T>; // All props required
  Readonly<T>; // All props readonly
  Pick<T, K>; // Select specific props
  Omit<T, K>; // Remove specific props
  Record<K, V>; // Object with key K and value V
  Exclude<T, U>; // Remove types from union
  Extract<T, U>; // Extract types from union
  NonNullable<T>; // Remove null/undefined
  Parameters<T>; // Function parameter types
  ReturnType<T>; // Function return type
  Awaited<T>; // Unwrap Promise
  ```
- **Exercise:** Use every utility type in real scenarios

### Task 21 — Discriminated Unions (Advanced) 🟡

- ```typescript
  // API Response pattern
  type ApiResponse<T> =
    | { status: 'loading' }
    | { status: 'success'; data: T }
    | { status: 'error'; error: string; code: number };

  function handleResponse<T>(response: ApiResponse<T>) {
    switch (response.status) {
      case 'loading':
        return 'Loading...';
      case 'success':
        return response.data; // TypeScript knows data exists
      case 'error':
        return response.error; // TypeScript knows error exists
    }
  }
  ```

- **Exercise:** State machine with discriminated unions

### Task 22 — Index Signatures ও Record Types 🟡

- ```typescript
  // Index signature
  interface StringMap {
    [key: string]: string;
  }

  // Record (better)
  type Config = Record<string, string | number | boolean>;

  // Constrained index
  interface UserRoles {
    [userId: string]: 'admin' | 'user' | 'moderator';
  }
  ```

- **Exercise:** Type-safe configuration system

### Task 23 — Declaration Files (.d.ts) 🟡

- Type declarations for JavaScript libraries
- `@types/` packages, DefinitelyTyped
- Writing custom `.d.ts` files
- **Exercise:** Write declaration file for a JS library without types

### Task 24 — Module Augmentation ও Declaration Merging 🔴

- ```typescript
  // Extend Express Request
  declare global {
    namespace Express {
      interface Request {
        user?: User;
        requestId: string;
      }
    }
  }

  // Extend existing module
  declare module 'express-session' {
    interface SessionData {
      userId: string;
    }
  }
  ```

- **Exercise:** Augment Express, React, ও Mongoose types

### Task 25 — Branded/Nominal Types 🔴

- ```typescript
  // Prevent mixing up similar types
  type UserId = string & { __brand: "UserId" };
  type OrderId = string & { __brand: "OrderId" };

  function getUser(id: UserId) { ... }
  function getOrder(id: OrderId) { ... }

  const userId = "abc" as UserId;
  const orderId = "xyz" as OrderId;

  getUser(userId);    // ✅
  getUser(orderId);   // ❌ Type error! Can't use OrderId as UserId
  ```

- **Exercise:** Branded types for ID, Email, URL, Money

### Task 26 — Variance ও Covariance 🔴

- Covariant (read), Contravariant (write), Invariant (both)
- `in`, `out` keywords in TypeScript 5+
- **Exercise:** Understand variance in function parameters ও return types

### Task 27 — Recursive Types 🔴

- ```typescript
  // JSON type
  type JSON = string | number | boolean | null | JSON[] | { [key: string]: JSON };

  // Deep partial
  type DeepPartial<T> = {
    [K in keyof T]?: T[K] extends object ? DeepPartial<T[K]> : T[K];
  };

  // Nested path type
  type Path<T> = T extends object
    ? { [K in keyof T]: K extends string ? K | `${K}.${Path<T[K]>}` : never }[keyof T]
    : never;
  ```

- **Exercise:** Build DeepReadonly, DeepRequired, FlattenObject types

### Task 28 — Type-Level Programming 🔴

- ```typescript
  // Type-level arithmetic (advanced)
  type Length<T extends any[]> = T['length'];
  type Push<T extends any[], V> = [...T, V];
  type Pop<T extends any[]> = T extends [...infer R, any] ? R : never;

  // String manipulation at type level
  type Split<S extends string, D extends string> = S extends `${infer T}${D}${infer U}`
    ? [T, ...Split<U, D>]
    : [S];
  ```

- **Exercise:** Build type-level utilities: Trim, Replace, Split

### Task 29 — Decorators (TypeScript 5+) 🟡

- Class decorators, Method decorators, Property decorators
- Stage 3 decorators vs Legacy decorators
- **Exercise:** Build decorators: @log, @validate, @cache, @retry

### Task 30 — TypeScript Compiler API 🔴

- Programmatic access to TypeScript compiler
- AST manipulation, custom transformers
- **Exercise:** Build a simple TypeScript linting rule

### Task 31 — Performance ও Best Practices 🟡

- Avoid deep type instantiation
- Use `interface extends` over `type &` for performance
- Lazy type evaluation
- **Exercise:** Profile ও optimize slow TypeScript compilation

### Task 32 — Strict Mode Complete 🟡

- Enable ALL strict flags, understand what each one does
- `strictNullChecks`, `strictFunctionTypes`, `strictBindCallApply`, `noUncheckedIndexedAccess`
- **Exercise:** Enable strict mode on an existing project, fix all errors

### Task 33 — TypeScript with Zod 🟡

- Runtime validation + TypeScript type inference
- ```typescript
  const UserSchema = z.object({
    name: z.string().min(2),
    email: z.string().email(),
    age: z.number().min(18),
  });
  type User = z.infer<typeof UserSchema>; // Auto-generated type!
  ```
- **Exercise:** Zod schemas for complete API validation

### Task 34 — TypeScript Patterns Collection 🟡

- Builder pattern, Factory with generics, Repository pattern
- Type-safe event emitter, Type-safe router
- **Exercise:** Implement 10 TypeScript design patterns

### Task 35 — Build: Type-Safe Utility Library ⚫

- Build a utility library (like Lodash) fully typed:
  - Array utilities: chunk, flatten, unique, groupBy
  - Object utilities: pick, omit, deepMerge, deepClone
  - String utilities: capitalize, camelCase, kebabCase
  - Function utilities: debounce, throttle, memoize, pipe
  - All with 100% type safety, generics, overloads
  - Published to npm with declaration files

---

## Section C: TypeScript in Real Projects (Tasks 36-50)

### Task 36 — TypeScript with Node.js/Express 🟡

- Express app fully typed: routes, middleware, request/response
- **Exercise:** Express API with full TypeScript (no `any` anywhere)

### Task 37 — TypeScript with React 🟡

- Component props, State types, Event handlers, Hooks
- ```typescript
  interface Props {
    user: User;
    onUpdate: (user: User) => void;
    children: React.ReactNode;
  }
  const UserCard: React.FC<Props> = ({ user, onUpdate, children }) => { ... }
  ```
- **Exercise:** React app with 100% TypeScript (no `any`)

### Task 38 — TypeScript with Prisma/ORM 🟡

- Auto-generated types from database schema
- Type-safe queries, relations, transactions
- **Exercise:** Prisma + TypeScript: type-safe database layer

### Task 39 — TypeScript with Testing 🟡

- Jest/Vitest with TypeScript, type-safe mocks
- **Exercise:** Test suite fully typed

### Task 40 — Type-Safe API Layer 🔴

- ```typescript
  // Shared types between frontend and backend
  interface ApiEndpoints {
    'GET /users': { response: User[]; query: { page: number } };
    'POST /users': { body: CreateUser; response: User };
    'GET /users/:id': { params: { id: string }; response: User };
  }
  ```
- **Exercise:** Shared type definitions for full-stack app

### Task 41 — TypeScript Monorepo Setup 🔴

- Turborepo/Nx with TypeScript
- Shared packages: `@app/types`, `@app/utils`, `@app/config`
- **Exercise:** Monorepo: shared types between frontend ও backend

### Task 42 — TypeScript Migration Strategy 🟡

- Gradually migrate JS → TS: `allowJs`, `checkJs`, strict incrementally
- **Exercise:** Migrate a JavaScript project to TypeScript step-by-step

### Task 43 — Advanced tsconfig for Different Environments 🟡

- Base config + extends for: server, client, tests, scripts
- **Exercise:** Multi-tsconfig setup for full-stack project

### Task 44 — TypeScript with GraphQL 🟡

- Code generation: GraphQL schema → TypeScript types
- graphql-codegen, type-safe resolvers
- **Exercise:** GraphQL API with generated TypeScript types

### Task 45 — TypeScript with WebSocket 🟡

- Type-safe WebSocket events
- **Exercise:** Type-safe real-time communication layer

### Task 46 — TypeScript with Queue/Workers 🟡

- Type-safe job definitions, payloads, results
- **Exercise:** BullMQ with typed jobs

### Task 47 — TypeScript Custom ESLint Rules 🟡

- @typescript-eslint/eslint-plugin advanced rules
- Custom rules for your project
- **Exercise:** ESLint config for strict TypeScript project

### Task 48 — TypeScript Performance Optimization 🟡

- Incremental builds, project references, `tsc --watch`
- SWC, esbuild for faster compilation
- **Exercise:** Optimize TypeScript build time by 5x

### Task 49 — TypeScript 5+ Latest Features 🟡

- `satisfies` operator, const type parameters, decorator metadata
- **Exercise:** Use all TypeScript 5+ features in a project

### Task 50 — Build: Full-Stack TypeScript Application ⚫

- **Phase 1 Final Project:**
  - Monorepo (Turborepo)
  - Backend: Express + Prisma + PostgreSQL (100% TypeScript)
  - Frontend: React/Next.js (100% TypeScript)
  - Shared: Types package, Utils package
  - Validation: Zod (shared between frontend ও backend)
  - Testing: Vitest with typed tests
  - CI/CD: Type checking in pipeline
  - **ZERO `any` in entire codebase**

---

## ✅ Phase 1 Completion Checklist

- [ ] সব 50 tasks complete
- [ ] Basic → Advanced TypeScript types confidently use করতে পারো
- [ ] Generics, Conditional Types, Mapped Types লিখতে পারো
- [ ] TypeScript দিয়ে Node.js + React project build করতে পারো
- [ ] Zero `any` codebase maintain করতে পারো
- [ ] Utility types নিজে build করতে পারো

> _"TypeScript = তোমার code এর bodyguard। এটা master করলে bugs তোমাকে ছুঁতে পারবে না।"_
