# JS Phase 08 — Metaprogramming & Advanced Patterns (Tasks 221-245)

> Metaprogramming মানে code যে code কে manipulate করে। Proxy, Reflect, Symbol — এগুলো JS এর most powerful features।

---

## Section A: Proxy & Reflect (Tasks 221-230)

### Task 221 — Proxy Fundamentals 🔴

- Intercept object operations:

  ```js
  const handler = {
    get(target, prop, receiver) {
      console.log(`Getting ${prop}`);
      return Reflect.get(target, prop, receiver);
    },
    set(target, prop, value, receiver) {
      console.log(`Setting ${prop} = ${value}`);
      return Reflect.set(target, prop, value, receiver);
    },
  };

  const proxy = new Proxy({ name: 'John' }, handler);
  proxy.name; // logs "Getting name" → "John"
  proxy.age = 25; // logs "Setting age = 25"
  ```

  - All 13 traps:
    - `get`, `set`, `has` (in operator), `deleteProperty`
    - `ownKeys`, `getOwnPropertyDescriptor`
    - `defineProperty`, `preventExtensions`, `isExtensible`
    - `getPrototypeOf`, `setPrototypeOf`
    - `apply` (function call), `construct` (new operator)
  - **Build:** Demo for each of the 13 traps

### Task 222 — Reflect API 🟡

- Mirror of Proxy traps:
  - `Reflect.get(target, prop)` — same as `target[prop]`
  - `Reflect.set(target, prop, value)` — same as `target[prop] = value`
  - `Reflect.has(target, prop)` — same as `prop in target`
  - `Reflect.deleteProperty(target, prop)` — same as `delete target[prop]`
  - `Reflect.ownKeys(target)` — all own keys (including Symbol)
  - `Reflect.apply(fn, thisArg, args)` — same as `fn.apply(thisArg, args)`
  - `Reflect.construct(Target, args)` — same as `new Target(...args)`
  - Why use Reflect inside Proxy traps: consistent return values, proper `this` handling
  - **Challenge:** Rewrite 10 operations using Reflect instead of direct syntax

### Task 223 — Build: Reactive System (Vue-like) 🔴

- Proxy-based reactivity:

  ```js
  function reactive(obj) {
    const subscribers = new Map();

    return new Proxy(obj, {
      get(target, prop) {
        // Track: record current effect as subscriber
        track(target, prop);
        return target[prop];
      },
      set(target, prop, value) {
        target[prop] = value;
        // Trigger: notify all subscribers
        trigger(target, prop);
        return true;
      },
    });
  }

  function effect(fn) {
    // Run fn, and re-run whenever its dependencies change
    activeEffect = fn;
    fn();
    activeEffect = null;
  }

  const state = reactive({ count: 0 });
  effect(() => console.log('Count:', state.count));
  state.count++; // Auto-logs "Count: 1"
  ```

  - **Build:** Complete reactive system with computed values ও watchers

### Task 224 — Build: Validation Proxy 🟡

- Type-safe objects:

  ```js
  const schema = {
    name: { type: 'string', required: true, minLength: 2 },
    age: { type: 'number', min: 0, max: 150 },
    email: { type: 'string', pattern: /^[^@]+@[^@]+\.[^@]+$/ },
  };

  const user = createValidated(schema);
  user.name = 'J'; // Error: minLength is 2
  user.age = -1; // Error: min is 0
  user.email = 'bad'; // Error: doesn't match pattern
  user.name = 'John'; // OK
  ```

  - **Build:** Full validation proxy with: type check, range, pattern, required, custom validators

### Task 225 — Build: Observable Object (Change Detection) 🟡

- Track all changes:

  ```js
  const obj = observable({ name: 'John', scores: [1, 2, 3] });

  obj.onChange((path, oldVal, newVal) => {
    console.log(`${path}: ${oldVal} → ${newVal}`);
  });

  obj.name = 'Jane'; // "name: John → Jane"
  obj.scores.push(4); // "scores.3: undefined → 4"
  obj.scores[0] = 10; // "scores.0: 1 → 10"
  ```

  - Deep proxy (nested objects ও arrays too)
  - **Build:** State management with change history (undo/redo)

### Task 226 — Build: API Mocking Proxy 🟡

- Intercept API calls:

  ```js
  const api = createMockAPI({
    'GET /users': [{ id: 1, name: 'John' }],
    'POST /users': (body) => ({ id: 2, ...body }),
    'GET /users/:id': (params) => ({ id: params.id, name: 'User' }),
  });

  const users = await api.get('/users');
  const newUser = await api.post('/users', { name: 'Jane' });
  ```

  - Delay simulation, error simulation, logging
  - **Output:** API mock for testing

### Task 227 — Build: Immutable Proxy 🟡

- Enforce immutability:

  ```js
  const config = immutable({
    db: { host: 'localhost', port: 5432 },
    api: { key: 'abc123' },
  });

  config.db.host; // "localhost" — OK
  config.db.host = 'x'; // Error: Cannot modify immutable object
  config.newProp = 'x'; // Error: Cannot add to immutable object
  delete config.api; // Error: Cannot delete from immutable object
  ```

  - Deep immutability (nested objects too)
  - Frozen vs Proxy-based: pros/cons

### Task 228 — Build: Access Control Proxy 🟡

- Role-based property access:

  ```js
  const user = secured(userData, {
    public: ['name', 'avatar'],
    private: ['email', 'phone'],
    admin: ['password', 'ssn'],
  });

  const publicView = user.as('public');
  publicView.name; // "John"
  publicView.email; // Error: Access denied

  const adminView = user.as('admin');
  adminView.password; // "hashed..." — OK
  ```

  - **Build:** RBAC (Role-Based Access Control) using Proxy

### Task 229 — Revocable Proxy 🟡

- Temporary access:

  ```js
  const { proxy, revoke } = Proxy.revocable(target, handler);

  proxy.name; // Works
  revoke(); // Cut off access
  proxy.name; // TypeError: Cannot perform 'get' on a proxy that has been revoked
  ```

  - Use cases: temporary API access, session-based data, resource cleanup
  - **Build:** Temporary data access system (auto-revoke after timeout)

### Task 230 — Build: ORM-like Query Builder with Proxy 🔴

- Magic property access:

  ```js
  const db = createQueryProxy();

  // These auto-generate SQL:
  db.users
    .where({ age: { gt: 18 } })
    .select('name', 'email')
    .limit(10);
  // → SELECT name, email FROM users WHERE age > 18 LIMIT 10

  db.posts.join('users').where({ published: true }).orderBy('created_at', 'DESC');
  // → SELECT * FROM posts JOIN users ON ... WHERE published = true ORDER BY created_at DESC
  ```

  - Property access creates query chain via Proxy
  - **Output:** SQL query builder using Proxy magic

---

## Section B: Symbols & Well-Known Symbols (Tasks 231-236)

### Task 231 — Symbol Deep Dive (Revisited) 🟡

- Advanced Symbol usage:
  - Symbol description: `Symbol('desc').description`
  - Global symbol registry: `Symbol.for()`, `Symbol.keyFor()`
  - Hidden properties: not in `Object.keys`, `for...in`, `JSON.stringify`
  - Visible in: `Object.getOwnPropertySymbols()`, `Reflect.ownKeys()`
  - **Build:** Private-like properties using Symbols (not truly private, but hidden)

### Task 232 — Symbol.iterator — Custom Iterables 🔴

- Make any object iterable:

  ```js
  class Range {
    constructor(start, end) {
      this.start = start;
      this.end = end;
    }

    [Symbol.iterator]() {
      let current = this.start;
      const end = this.end;
      return {
        next() {
          return current <= end ? { value: current++, done: false } : { done: true };
        },
      };
    }
  }

  for (const n of new Range(1, 5)) console.log(n); // 1, 2, 3, 4, 5
  [...new Range(1, 5)]; // [1, 2, 3, 4, 5]
  ```

  - **Build:** 5 custom iterables:
    1. `Range(start, end, step)` — number range
    2. `InfiniteSequence(fn)` — fibonacci, primes, etc.
    3. `LinkedList` — iterable linked list
    4. `BinaryTree` — iterable tree (inOrder, preOrder, postOrder)
    5. `Paginator(fetchFn)` — paginated API data

### Task 233 — Symbol.toPrimitive — Custom Type Conversion 🟡

- Control how object converts:

  ```js
  class Money {
    constructor(amount, currency) {
      this.amount = amount;
      this.currency = currency;
    }

    [Symbol.toPrimitive](hint) {
      switch (hint) {
        case 'number':
          return this.amount;
        case 'string':
          return `${this.amount} ${this.currency}`;
        case 'default':
          return this.amount;
      }
    }
  }

  const price = new Money(100, 'BDT');
  +price; // 100 (number hint)
  `${price}`; // "100 BDT" (string hint)
  price + 50; // 150 (default hint)
  ```

  - Also: `toString()`, `valueOf()` — older approach
  - **Build:** Temperature class (auto-converts between Celsius/Fahrenheit)

### Task 234 — Symbol.species — Constructor for Derived Objects 🔴

- Control what constructor derived methods use:

  ```js
  class SpecialArray extends Array {
    static get [Symbol.species]() {
      return Array; // map, filter return plain Array, not SpecialArray
    }
  }

  const arr = new SpecialArray(1, 2, 3);
  const mapped = arr.map((x) => x * 2);
  mapped instanceof SpecialArray; // false
  mapped instanceof Array; // true
  ```

  - Use case: subclass Array/Promise/RegExp without affecting derived objects

### Task 235 — Symbol.asyncIterator — Async Iteration 🔴

- Async iterables:

  ```js
  class AsyncRange {
    constructor(start, end) {
      this.start = start;
      this.end = end;
    }

    async *[Symbol.asyncIterator]() {
      for (let i = this.start; i <= this.end; i++) {
        await new Promise((r) => setTimeout(r, 100));
        yield i;
      }
    }
  }

  for await (const n of new AsyncRange(1, 5)) console.log(n);
  ```

  - **Build:** Async data stream from API with `for await...of`

### Task 236 — All Well-Known Symbols Reference 🟡

- Complete reference:
  | Symbol | Purpose | Example |
  |--------|---------|---------|
  | `Symbol.iterator` | Make iterable | `for...of`, spread |
  | `Symbol.asyncIterator` | Async iterable | `for await...of` |
  | `Symbol.toPrimitive` | Type conversion | `+obj`, `${obj}` |
  | `Symbol.toStringTag` | Custom toString | `[object MyClass]` |
  | `Symbol.hasInstance` | Custom instanceof | `x instanceof MyClass` |
  | `Symbol.species` | Derived constructor | `arr.map()` return type |
  | `Symbol.match` | Custom regex match | `str.match(obj)` |
  | `Symbol.replace` | Custom replace | `str.replace(obj, x)` |
  | `Symbol.search` | Custom search | `str.search(obj)` |
  | `Symbol.split` | Custom split | `str.split(obj)` |
  | `Symbol.isConcatSpreadable` | Spread in concat | `arr.concat(obj)` |
  | `Symbol.unscopables` | with statement filter | — |
  - **Build:** Custom class using at least 7 well-known symbols

---

## Section C: Advanced Metaprogramming (Tasks 237-245)

### Task 237 — eval() ও Function Constructor 🔴

- Dynamic code execution:
  - `eval('1 + 2')` — executes string as code
  - `new Function('a', 'b', 'return a + b')` — creates function from string
  - **DANGER:** Security risks (code injection)
  - **When legitimate:** template engines, calculators, code playgrounds
  - Safer alternative: `new Function` (doesn't access local scope)
  - **Build:** Safe expression evaluator (math only, no arbitrary code)

### Task 238 — Tagged Templates Advanced 🟡

- Template tag functions:

  ```js
  function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
      const value = values[i] ? `<mark>${values[i]}</mark>` : '';
      return result + string + value;
    }, '');
  }

  const name = 'John';
  highlight`Hello ${name}, welcome!`;
  // "Hello <mark>John</mark>, welcome!"
  ```

  - **Build:**
    1. `html` tag — HTML template with XSS protection
    2. `css` tag — CSS-in-JS with scoping
    3. `sql` tag — parameterized SQL queries
    4. `gql` tag — GraphQL query builder
    5. `i18n` tag — internationalized strings

### Task 239 — WeakRef ও FinalizationRegistry Advanced 🔴

- Weak references advanced:

  ```js
  const registry = new FinalizationRegistry((heldValue) => {
    console.log(`${heldValue} was garbage collected`);
  });

  let obj = { name: 'heavy' };
  const ref = new WeakRef(obj);
  registry.register(obj, 'heavy object');

  obj = null; // Now eligible for GC
  // Eventually: "heavy object was garbage collected"
  ```

  - **Build:** Smart cache that auto-evicts when memory is low
  - **Build:** Object lifecycle tracker (count live instances)

### Task 240 — AST (Abstract Syntax Tree) 🔴

- Parse ও manipulate JS code:
  - What is AST: code → structured tree representation
  - Use acorn/esprima to parse JS → AST
  - Use escodegen to AST → JS
  - **Build:**
    1. Code complexity analyzer (count nesting depth, function count)
    2. Variable renamer (find ও rename all occurrences)
    3. Dead code detector (find unused variables/functions)
    4. Simple code formatter

### Task 241 — Build: Dependency Injection Framework 🔴

- DI with decorators ও metadata:

  ```js
  const container = new DIContainer();

  container.register('logger', () => new Logger());
  container.register('db', () => new Database(), { singleton: true });
  container.register('userService', (c) => new UserService(c.resolve('db'), c.resolve('logger')));

  const userService = container.resolve('userService');
  ```

  - Features: singleton/transient, lazy initialization, circular dependency detection, auto-wiring
  - **Output:** Full DI container

### Task 242 — Build: Schema Validation Library 🟡

- Runtime type checking:

  ```js
  const userSchema = schema({
    name: string().min(2).max(50).required(),
    email: string().email().required(),
    age: number().min(0).max(150).optional(),
    role: oneOf(['admin', 'user', 'guest']).default('user'),
    address: object({
      city: string().required(),
      zip: string().pattern(/^\d{4}$/),
    }).optional(),
    tags: array(string()).min(1).max(10),
  });

  const result = userSchema.validate(data);
  // { success: boolean, data: cleanData, errors: [...] }
  ```

  - **Output:** Zod-like validation library (pure JS)

### Task 243 — Build: Reactive Store (Redux-lite) 🔴

- State management:

  ```js
  const store = createStore(
    // Reducer
    (state, action) => {
      switch (action.type) {
        case 'ADD_TODO':
          return { ...state, todos: [...state.todos, action.payload] };
        case 'TOGGLE_TODO':
          return {
            ...state,
            todos: state.todos.map((t) => (t.id === action.payload ? { ...t, done: !t.done } : t)),
          };
        default:
          return state;
      }
    },
    // Initial state
    { todos: [] }
  );

  store.subscribe((state) => console.log('State:', state));
  store.dispatch({ type: 'ADD_TODO', payload: { id: 1, text: 'Learn JS', done: false } });
  ```

  - Features: middleware, devtools (time travel), selectors, combine reducers
  - **Output:** Complete state management library

### Task 244 — Build: Template Compiler 🔴

- Compile templates to functions:

  ```js
  const template = compile(`
    <h1>{{title}}</h1>
    {{#each items}}
      <p>{{name}} - {{price}}</p>
    {{/each}}
    {{#if showTotal}}
      <p>Total: {{total}}</p>
    {{/if}}
  `);

  // template is now a function:
  const html = template({ title: 'Products', items: [...], showTotal: true, total: 500 });
  ```

  - Lexer → Parser → Compiler → Runtime
  - **Output:** Handlebars-lite template compiler

### Task 245 — Build: Mini Programming Language ⚫

- Phase 08 graduation — build a language:
  1. **Lexer:** Source code → tokens
  2. **Parser:** Tokens → AST
  3. **Interpreter:** Execute AST
  - Language features:
    - Variables: `let x = 5`
    - Math: `+`, `-`, `*`, `/`
    - Comparison: `==`, `!=`, `<`, `>`
    - If/else: `if (x > 5) { ... } else { ... }`
    - While loop: `while (x < 10) { ... }`
    - Functions: `fn add(a, b) { return a + b }`
    - Print: `print("Hello")`
  - **Output:** Working programming language interpreter in JS

---

## ✅ Phase 08 Completion Checklist

- [ ] Proxy ও all 13 traps বোঝো ও ব্যবহার করতে পারো
- [ ] Reflect API ব্যবহার করতে পারো
- [ ] Proxy দিয়ে reactive system build করতে পারো
- [ ] Validation, immutability, access control — Proxy দিয়ে পারো
- [ ] All well-known Symbols জানো ও ব্যবহার করতে পারো
- [ ] Custom iterables (sync ও async) build করতে পারো
- [ ] Tagged templates advanced use পারো
- [ ] WeakRef ও FinalizationRegistry বোঝো
- [ ] AST concept বোঝো ও basic manipulation পারো
- [ ] Schema validation library build করতে পারো
- [ ] State management (Redux-like) build করতে পারো
- [ ] Mini programming language build করতে পারো

> ✅ Ready? → **JS Phase 09: Performance & Security**
