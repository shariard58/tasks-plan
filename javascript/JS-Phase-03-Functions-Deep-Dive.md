# JS Phase 03 — Functions Deep Dive & Functional Programming (Tasks 61-90)

> JavaScript এ function হলো first-class citizen। Function বুঝলে JS বোঝা হয়ে যায়।

---

## Section A: Function Internals (Tasks 61-70)

### Task 61 — Function Creation — All 7 Ways 🟡

- 7 ways to create functions:
  1. Function Declaration: `function greet() {}`
  2. Function Expression: `const greet = function() {}`
  3. Arrow Function: `const greet = () => {}`
  4. Method Shorthand: `{ greet() {} }`
  5. Constructor: `new Function('a', 'b', 'return a + b')`
  6. Generator: `function* gen() {}`
  7. Async: `async function fetch() {}`
  - প্রতিটার difference: hoisting, `this`, `arguments`, `new`, `prototype`
  - **Output:** Comparison table ও interactive demo

### Task 62 — arguments Object Deep Dive 🟡

- arguments object:
  - Array-like (has length, indexed) but NOT an array
  - No array methods (map, filter, etc.)
  - Convert to array: `Array.from(arguments)`, `[...arguments]`
  - `arguments.callee` (deprecated, references the function itself)
  - Relation with named parameters (non-strict: linked, strict: independent)
  - Arrow functions: NO arguments object (uses parent's)
  - Rest parameters (`...args`) — modern alternative
  - **Build:** Function that handles any number of arguments (variadic)

### Task 63 — Default Parameters Internals 🟢

- Default params deep dive:
  - `function fn(a = 1, b = a * 2)` — can reference previous params
  - Default creates its own scope (TDZ applies)
  - Default can be function call: `fn(a = getDefault())`
  - undefined triggers default, null does NOT
  - Default ও destructuring combined: `fn({name = 'anon'} = {})`
  - **Challenge:** 10টা edge case problems with defaults

### Task 64 — IIFE (Immediately Invoked Function Expression) 🟡

- IIFE patterns:
  - Classic: `(function() { ... })()`
  - Arrow: `(() => { ... })()`
  - With params: `(function(global) { ... })(window)`
  - Named IIFE: `(function init() { ... })()`
  - IIFE for module pattern (pre-ES modules)
  - IIFE for avoiding global pollution
  - IIFE for creating private scope
  - **Build:** Module system using IIFE (revealing module pattern)

### Task 65 — Higher-Order Functions 🟡

- Functions that take or return functions:
  - `map`, `filter`, `reduce` — are HOFs
  - Build custom HOFs:
    1. `once(fn)` — function that runs only once
    2. `times(n, fn)` — run fn n times
    3. `after(n, fn)` — fn runs only after n calls
    4. `before(n, fn)` — fn runs only first n-1 calls
    5. `negate(predicateFn)` — inverts boolean result
    6. `complement(fn)` — same as negate
    7. `flip(fn)` — reverses argument order
    8. `spread(fn)` — `fn([a,b])` → `fn(a, b)`
    9. `unspread(fn)` — `fn(a, b)` → `fn([a,b])`
  - **Output:** Utility library of HOFs with tests

### Task 66 — Function Composition & Pipe 🔴

- Composition:
  - `compose(f, g)(x)` = `f(g(x))` (right to left)
  - `pipe(f, g)(x)` = `g(f(x))` (left to right)
  - **Build:**
    ```js
    const compose =
      (...fns) =>
      (x) =>
        fns.reduceRight((acc, fn) => fn(acc), x);
    const pipe =
      (...fns) =>
      (x) =>
        fns.reduce((acc, fn) => fn(acc), x);
    ```
  - Async compose: `composeAsync(...fns)` — handles Promise returns
  - **Real example:**
    ```js
    const processUser = pipe(validateInput, normalizeEmail, hashPassword, saveToDatabase);
    ```
  - **Challenge:** Data transformation pipeline — CSV string → parsed → filtered → sorted → formatted

### Task 67 — Currying & Partial Application 🔴

- Currying:
  - `curry(fn)` — transforms `fn(a, b, c)` → `fn(a)(b)(c)`
  - **Build:** Auto-curry function:
    ```js
    function curry(fn) {
      return function curried(...args) {
        if (args.length >= fn.length) return fn(...args);
        return (...more) => curried(...args, ...more);
      };
    }
    ```
  - Partial Application:
  - `partial(fn, ...args)` — pre-fill some arguments
  - Placeholder support: `partial(fn, _, 2, _)(1, 3)` → `fn(1, 2, 3)`
  - **Real examples:**

    ```js
    const add = curry((a, b) => a + b);
    const add5 = add(5);
    add5(3); // 8

    const formatCurrency = curry((symbol, amount) => `${symbol}${amount}`);
    const formatBDT = formatCurrency('৳');
    formatBDT(500); // "৳500"
    ```

### Task 68 — Memoization 🟡

- Cache function results:
  - **Build:** Basic memoize:
    ```js
    function memoize(fn) {
      const cache = new Map();
      return function (...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
      };
    }
    ```
  - Advanced features:
    - Custom key generator
    - Max cache size (LRU eviction)
    - TTL (time to live) — auto-expire
    - Async memoize (cache Promise)
    - WeakMap-based memoize (for object args)
  - **Build:** Full-featured memoize library

### Task 69 — Recursion Mastery 🔴

- Recursive patterns:
  - Base case + recursive case
  - **Problems:**
    1. Factorial
    2. Fibonacci (naive → memoized → iterative)
    3. Deep flatten array
    4. Deep clone object (handle circular refs)
    5. DOM tree traversal
    6. File system traversal
    7. Permutations ও combinations
    8. Tower of Hanoi
    9. Binary search (recursive)
    10. Merge sort (recursive)
  - Tail call optimization (TCO) — concept ও current browser support
  - Trampoline function — avoid stack overflow:
    ```js
    function trampoline(fn) {
      return function (...args) {
        let result = fn(...args);
        while (typeof result === 'function') result = result();
        return result;
      };
    }
    ```

### Task 70 — Debounce & Throttle From Scratch 🟡

- **Debounce:** Wait until user stops doing something
  ```js
  function debounce(fn, delay, { leading = false } = {}) {
    let timerId;
    return function (...args) {
      const callNow = leading && !timerId;
      clearTimeout(timerId);
      timerId = setTimeout(() => {
        timerId = null;
        if (!leading) fn.apply(this, args);
      }, delay);
      if (callNow) fn.apply(this, args);
    };
  }
  ```
- **Throttle:** Run at most once per interval
  ```js
  function throttle(fn, interval, { leading = true, trailing = true } = {}) {
    // implement with leading/trailing edge options
  }
  ```
- Both with: cancel(), flush(), pending check
- **Build:** Interactive demo page — type in input, see debounce vs throttle difference visually

---

## Section B: Functional Programming Patterns (Tasks 71-80)

### Task 71 — Pure Functions & Side Effects 🟡

- Pure function rules:
  - Same input → always same output
  - No side effects (no mutation, no I/O, no random)
  - **Identify:** 20টা function — pure or impure?
  - **Refactor:** 10টা impure functions → pure
  - Side effects containment: push side effects to the edges
  - **Output:** Pure function checklist ও examples

### Task 72 — Immutable Data Manipulation 🟡

- Immutable patterns:
  - Array: filter, map, reduce, slice, concat, spread
  - Object: Object.assign, spread, computed properties
  - Nested updates (the hard part)
  - **Build:** Immutable update utility library:

    ```js
    const state = { users: [{ id: 1, name: 'John', address: { city: 'Dhaka' } }] };

    // Update nested: state.users[0].address.city = 'Chittagong'
    // Without mutation:
    updateIn(state, ['users', 0, 'address', 'city'], 'Chittagong');
    ```

  - Performance: structural sharing concept (persistent data structures)

### Task 73 — Functor & Monad Concepts (Simplified) 🔴

- Functional concepts in JS:
  - **Functor:** Container with `.map()` → Array is a Functor!
  - **Build:** Maybe monad (handle null/undefined safely):

    ```js
    class Maybe {
      constructor(value) {
        this._value = value;
      }
      static of(value) {
        return new Maybe(value);
      }
      isNothing() {
        return this._value == null;
      }
      map(fn) {
        return this.isNothing() ? this : Maybe.of(fn(this._value));
      }
      getOrElse(defaultVal) {
        return this.isNothing() ? defaultVal : this._value;
      }
    }

    Maybe.of(user)
      .map((u) => u.address)
      .map((a) => a.city)
      .getOrElse('Unknown'); // Safe null chaining!
    ```

  - **Build:** Either monad (handle errors):
    ```js
    // Right = success, Left = error
    // Like try/catch but functional
    ```
  - **Build:** IO monad (defer side effects)
  - **Focus:** Don't memorize theory — understand the practical pattern

### Task 74 — Transducers 🔴

- Efficient data transformation:
  - Problem: `arr.map(fn1).filter(fn2).map(fn3)` — 3 passes over array
  - Transducers: compose transformations, single pass
  - **Build:** Transducer library:
    ```js
    const xform = compose(
      map((x) => x * 2),
      filter((x) => x > 5),
      take(3)
    );
    transduce(xform, append, [], [1, 2, 3, 4, 5]); // [6, 8, 10]
    ```
  - **Benchmark:** transducer vs chained methods on large arrays

### Task 75 — Lens Pattern 🔴

- Focus on nested data:
  - **Build:** Lens for immutable nested updates:

    ```js
    const nameLens = lens(
      (obj) => obj.name, // getter
      (obj, val) => ({ ...obj, name: val }) // setter
    );

    view(nameLens, user); // "John"
    set(nameLens, user, 'Jane'); // { ...user, name: "Jane" }
    over(nameLens, user, (s) => s.toUpperCase()); // { ...user, name: "JOHN" }
    ```

  - Compose lenses for deep access
  - **Output:** Working lens library

### Task 76 — Point-Free Style 🟡

- Tacit programming:
  - Don't mention the data:

    ```js
    // Not point-free:
    const getNames = (users) => users.map((u) => u.name);

    // Point-free:
    const prop = (key) => (obj) => obj[key];
    const getNames = map(prop('name'));
    ```

  - Helper functions: `prop`, `path`, `identity`, `always`, `tap`
  - **Challenge:** Refactor 10 functions to point-free style
  - When point-free is readable vs when it's not

### Task 77 — Functional Error Handling 🟡

- No try/catch, no exceptions:
  - Result type: `Ok(value)` or `Err(error)`
  - **Build:** Result type:
    ```js
    class Result {
      static ok(value) {
        return new Ok(value);
      }
      static err(error) {
        return new Err(error);
      }
    }
    class Ok extends Result {
      map(fn) {
        return Result.ok(fn(this.value));
      }
    }
    class Err extends Result {
      map(fn) {
        return this;
      }
    } // skip transformation
    ```
  - Chain results: `.map()`, `.flatMap()`, `.getOrElse()`, `.match({ok, err})`
  - **Real example:** Validation pipeline using Result

### Task 78 — Observable Pattern (RxJS-lite) 🔴

- Reactive programming:
  - **Build:** Simple Observable:
    ```js
    class Observable {
      constructor(subscribeFn) {
        this._subscribe = subscribeFn;
      }
      subscribe(observer) {
        return this._subscribe(observer);
      }
      map(fn) {
        /* return new Observable that maps values */
      }
      filter(fn) {
        /* return new Observable that filters */
      }
      static fromEvent(element, eventName) {
        /* ... */
      }
      static interval(ms) {
        /* ... */
      }
    }
    ```
  - Operators: map, filter, debounce, throttle, merge, switchMap
  - **Build:** Typeahead search using custom Observable

### Task 79 — Functional Utilities Library 🔴

- Build a comprehensive FP utility library (Ramda-lite):
  - **Data:** `identity`, `always`, `clone`, `equals`
  - **Function:** `curry`, `compose`, `pipe`, `memoize`, `once`, `flip`
  - **Array:** `map`, `filter`, `reduce`, `find`, `head`, `tail`, `last`, `init`, `take`, `drop`, `zip`, `unzip`, `chunk`, `flatten`, `unique`, `groupBy`, `sortBy`, `partition`
  - **Object:** `pick`, `omit`, `merge`, `path`, `pathOr`, `assocPath`, `dissocPath`, `keys`, `values`, `entries`, `fromEntries`, `mapValues`
  - **String:** `capitalize`, `camelCase`, `snakeCase`, `kebabCase`, `truncate`, `slugify`
  - **Logic:** `ifElse`, `cond`, `when`, `unless`, `both`, `either`, `complement`
  - All functions curried, all pure
  - **Output:** Complete FP library with JSDoc + tests

### Task 80 — Functional Programming Challenges 🔴

- 15 FP challenges:
  1. Transform CSV to JSON (pure functions, pipe)
  2. Validate form data (compose validators)
  3. Tree structure → flat list (recursive reduce)
  4. Build query string from object
  5. Parse URL into components
  6. Group transactions by date ও category
  7. Calculate statistics (mean, median, mode) — functional
  8. Text analytics (word count, char count, sentence count)
  9. Shopping cart totals (discounts, tax) — pure functions
  10. Config merger (deep merge with override rules)
  11. Event log analyzer (filter, group, aggregate)
  12. Data pipeline: API response → normalize → filter → sort → format
  13. Matrix operations (add, multiply, transpose) — functional
  14. Schedule optimizer (time slots → best arrangement)
  15. Build a calculator using function composition

---

## Section C: Advanced Function Patterns (Tasks 81-90)

### Task 81 — Function Overloading Patterns 🟡

- JS doesn't have native overloading, but:
  - Argument length check
  - Argument type check
  - Options object pattern
  - **Build:** `overload()` helper:
    ```js
    const process = overload(
      [String, (s) => console.log('String:', s)],
      [Number, (n) => console.log('Number:', n)],
      [Array, (a) => console.log('Array:', a.length)],
      [Object, (o) => console.log('Object:', Object.keys(o))]
    );
    ```

### Task 82 — Method Chaining (Fluent Interface) 🟡

- Chainable API:
  - **Build:** Query builder:
    ```js
    const results = query(users)
      .where('age', '>', 18)
      .where('city', '=', 'Dhaka')
      .select('name', 'email')
      .orderBy('name')
      .limit(10)
      .execute();
    ```
  - **Build:** DOM manipulation library (jQuery-like chaining)
  - **Build:** Animation builder with chaining

### Task 83 — Command Pattern 🟡

- Encapsulate actions as objects:
  - **Build:** Undo/Redo system:
    ```js
    class Command {
      execute() {}
      undo() {}
    }
    class AddTextCommand extends Command { ... }
    class DeleteTextCommand extends Command { ... }
    class CommandManager {
      execute(cmd) { cmd.execute(); this.history.push(cmd); }
      undo() { const cmd = this.history.pop(); cmd.undo(); }
    }
    ```
  - **Apply to:** Text editor with undo/redo

### Task 84 — Middleware Pattern 🔴

- Express.js-like middleware:
  - **Build:** Middleware engine:
    ```js
    class MiddlewareEngine {
      use(fn) {
        /* add middleware */
      }
      execute(context) {
        /* run all middleware in order */
      }
    }
    ```
  - Middleware: `(context, next) => { /* do stuff */ next(); }`
  - **Build:** HTTP request processor with middleware:
    - Logger middleware
    - Auth middleware
    - Validation middleware
    - Rate limit middleware
    - Error handling middleware

### Task 85 — Plugin System 🔴

- Extensible architecture:
  - **Build:** Plugin manager:

    ```js
    class App {
      use(plugin) {
        plugin.install(this);
      }
      // Hooks: onInit, onRequest, onError, onDestroy
    }

    const loggerPlugin = {
      install(app) {
        app.on('request', (req) => console.log(req));
      },
    };
    ```

  - Plugin lifecycle: install, activate, deactivate, uninstall
  - Plugin dependencies
  - Plugin configuration

### Task 86 — Dependency Injection in JS 🟡

- DI without frameworks:
  - **Build:** Simple DI container:

    ```js
    class Container {
      register(name, factory) {
        /* ... */
      }
      resolve(name) {
        /* ... */
      }
    }

    container.register('db', () => new Database());
    container.register('userService', (c) => new UserService(c.resolve('db')));
    ```

  - Benefits: testability, loose coupling
  - **Output:** DI container with singleton/transient support

### Task 87 — Event Emitter Complete Implementation 🟡

- Full-featured event system:
  - `on(event, handler)` — listen
  - `off(event, handler)` — remove
  - `once(event, handler)` — listen once
  - `emit(event, ...args)` — fire event
  - `removeAllListeners(event?)` — clear
  - `listeners(event)` — get handlers
  - `listenerCount(event)` — count
  - Wildcard support: `on('user.*', handler)`
  - Max listeners warning (memory leak prevention)
  - **Output:** Production-quality EventEmitter

### Task 88 — State Machine Implementation 🔴

- Finite state machine:
  - **Build:**

    ```js
    const lightMachine = createMachine({
      initial: 'red',
      states: {
        red: { on: { NEXT: 'green' } },
        green: { on: { NEXT: 'yellow' } },
        yellow: { on: { NEXT: 'red' } },
      },
    });

    lightMachine.transition('red', 'NEXT'); // 'green'
    ```

  - Features: guards, actions, context, nested states
  - **Build:** Checkout flow state machine (cart → shipping → payment → confirm)
  - **Output:** State machine library with visualizer

### Task 89 — Pub/Sub System 🟡

- Publish/Subscribe:
  - **Build:** Message bus:
    ```js
    const bus = createPubSub();
    const unsubscribe = bus.subscribe('user:created', (user) => { ... });
    bus.publish('user:created', { name: 'John' });
    unsubscribe();
    ```
  - Topic-based routing
  - Wildcard subscriptions
  - Message history
  - Dead letter queue (unhandled messages)
  - **Output:** PubSub library with tests

### Task 90 — Build: Lodash-Lite Library ⚫

- Phase 03 graduation project:
  - Implement 50+ utility functions (like Lodash but simpler)
  - Categories: Array, Object, String, Function, Collection, Lang, Math
  - All functions pure ও well-tested
  - Documentation page (each function with example)
  - Published to npm (optional)
  - Bundle size optimized (tree-shakeable ES modules)
  - **Output:** Complete utility library

---

## ✅ Phase 03 Completion Checklist

- [ ] Function creation 7 ways জানো ও difference বোঝো
- [ ] Higher-order functions confidently ব্যবহার করতে পারো
- [ ] compose/pipe implement ও ব্যবহার করতে পারো
- [ ] Currying ও partial application পারো
- [ ] Memoization implement করতে পারো (with advanced features)
- [ ] Recursion confidently handle করতে পারো
- [ ] Debounce ও Throttle নিজে implement করতে পারো
- [ ] FP concepts বোঝো (pure functions, immutability, functors)
- [ ] Method chaining, middleware, plugin patterns পারো
- [ ] Event emitter, state machine, pub/sub build করতে পারো
- [ ] Utility library build ও publish করতে পারো

> ✅ Ready? → **JS Phase 04: OOP & Prototypes**
