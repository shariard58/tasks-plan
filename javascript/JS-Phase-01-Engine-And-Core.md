# JS Phase 01 — JavaScript Engine & Core Internals (Tasks 1-30)

> JavaScript কিভাবে internally কাজ করে — এটা না বুঝলে তুমি চিরকাল surface-level developer থাকবে।
> এই Phase শেষে তুমি JS engine এর ভিতরে কী হচ্ছে সব বলতে পারবে।

---

## Section A: How JavaScript Runs (Tasks 1-8)

### Task 1 — JavaScript Engine Exploration 🟢

- Research ও document করো:
  - V8 (Chrome, Node.js), SpiderMonkey (Firefox), JavaScriptCore (Safari)
  - Engine এর 3টা main step: **Parsing → Compilation → Execution**
  - JS কি interpreted না compiled? (Answer: JIT — Just In Time)
  - Ignition (V8 interpreter) ও TurboFan (V8 optimizer) কি করে
  - **Output:** একটা diagram বানাও যেটা দেখায় code → engine → output flow

### Task 2 — Parsing & AST (Abstract Syntax Tree) 🟡

- JS code কিভাবে parse হয়:
  - Tokenization/Lexing: code → tokens
  - Parsing: tokens → AST
  - https://astexplorer.net এ যাও → নিজের code এর AST দেখো
  - Simple function এর AST manually draw করো
  - AST কোথায় ব্যবহার হয়: ESLint, Babel, Prettier — সব AST based
  - **Output:** 5টা different code snippet এর AST compare করো

### Task 3 — Execution Context Deep Dive 🟡

- Execution Context কী:
  - **Global Execution Context** — code load হলে প্রথম যেটা তৈরি হয়
  - **Function Execution Context** — প্রতিটা function call এ নতুন context
  - **Eval Execution Context** — (rare, avoid eval)
  - প্রতিটা context এর 2 phase: **Creation Phase** ও **Execution Phase**
  - Creation Phase এ কি হয়: Variable Object, Scope Chain, `this` binding
  - **Output:** 5টা code snippet diagram আঁকো — context কিভাবে create হয়

### Task 4 — Call Stack Visualizer 🟡

- Call Stack বোঝো ও visualize করো:
  - একটা web page বানাও যেটা call stack দেখায়
  - Function call → push to stack, Function return → pop from stack
  - Recursive function এর stack trace দেখাও
  - Stack Overflow কিভাবে হয় — demonstrate করো
  - Maximum call stack size বের করো (browser-specific)
  - Chrome DevTools এ Call Stack panel ব্যবহার করো
  - **Output:** Interactive call stack visualizer (HTML + JS)

### Task 5 — Memory Heap Visualization 🟡

- Memory model:
  - Stack Memory: primitives, references
  - Heap Memory: objects, arrays, functions
  - Stack vs Heap — diagram আঁকো
  - Primitive: value stored in stack
  - Object: reference in stack, value in heap
  - **Code demo:** prove that primitives are copied, objects are referenced

  ```js
  let a = 10;
  let b = a; // b gets copy of value
  b = 20; // a is still 10

  let obj1 = { x: 1 };
  let obj2 = obj1; // obj2 gets copy of REFERENCE
  obj2.x = 99; // obj1.x is also 99!
  ```

  - **Output:** Interactive memory visualizer page

### Task 6 — Garbage Collection 🟡

- How JS cleans memory:
  - Reference counting (old method, problems with circular references)
  - Mark-and-Sweep algorithm (modern — V8 uses this)
  - Generational collection: Young Gen (Scavenger) ও Old Gen (Mark-Compact)
  - কখন object garbage collected হয়: no more references
  - Memory leak কিভাবে হয়: forgotten references
  - **Code demo:** 5টা memory leak pattern create করো ও fix করো:
    1. Forgotten timer (setInterval without clear)
    2. Closures holding references
    3. Detached DOM nodes
    4. Global variables
    5. Event listeners not removed

### Task 7 — Variable Environments & Hoisting 🟡

- Deep hoisting:
  - `var` — hoisted, initialized as `undefined`
  - `let`/`const` — hoisted, but in **Temporal Dead Zone (TDZ)**
  - Function declarations — fully hoisted (can call before declaration)
  - Function expressions — variable hoisted, not the function
  - Class declarations — hoisted but in TDZ
  - **Challenge:** 20টা hoisting puzzle তৈরি করো → output predict করো → verify করো
  - **Output:** Hoisting quiz page (show code → guess output → reveal answer)

### Task 8 — Strict Mode Deep Dive 🟢

- `"use strict"` কী change করে:
  - Accidental globals prevent করে
  - Silent errors → throw errors
  - `this` in function = `undefined` (not `window`)
  - `delete` on variables/functions → error
  - Duplicate parameter names → error
  - Octal syntax → error
  - `with` statement → error
  - Reserved words as variable names → error
  - **Challenge:** 15টা code snippet — strict mode vs non-strict output compare করো

---

## Section B: Scope, this & Context (Tasks 9-18)

### Task 9 — Scope Chain Visualization 🟡

- Scope types:
  - Global scope
  - Function scope
  - Block scope (`let`, `const` in `{}`)
  - Module scope
  - Lexical scope (static — determined at write time, not runtime)
  - Scope chain: inner → outer → ... → global
  - **Output:** Interactive scope visualizer — code input করলে scope chain দেখায়

### Task 10 — Lexical Environment Internals 🔴

- Internal structure:
  - Every execution context has a **Lexical Environment**
  - Lexical Environment = **Environment Record** + **Outer Reference**
  - Environment Record: variable storage
  - Outer Reference: link to parent scope
  - How scope chain is formed via outer references
  - **Output:** 10টা nested function example → lexical environment chain diagram draw করো

### Task 11 — The `this` Keyword — All 7 Rules 🔴

- `this` binding rules (priority order):
  1. `new` binding → `this` = newly created object
  2. Explicit binding → `call()`, `apply()`, `bind()` → `this` = specified object
  3. Implicit binding → `obj.method()` → `this` = obj
  4. Default binding → standalone function → `this` = window (or undefined in strict)
  5. Arrow function → `this` = lexical (parent's this)
  6. Event handler → `this` = element (unless arrow function)
  7. Class method → `this` = instance
  - **Challenge:** 30টা `this` puzzle তৈরি করো → output predict করো → verify করো
  - **Output:** Interactive `this` quiz page

### Task 12 — call(), apply(), bind() From Scratch 🟡

- নিজে implement করো:
  - `Function.prototype.myCall = function(context, ...args) {...}`
  - `Function.prototype.myApply = function(context, args) {...}`
  - `Function.prototype.myBind = function(context, ...args) {...}`
  - Edge cases: context null/undefined → global, primitive context → wrap
  - **Test:** নিজের implementation vs native — same output verify করো
  - **Output:** Working polyfills with test cases

### Task 13 — Arrow Functions vs Regular Functions 🟡

- Every difference:
  - `this` binding (lexical vs dynamic)
  - `arguments` object (doesn't exist in arrow)
  - `new` keyword (can't use with arrow)
  - `prototype` property (arrow doesn't have)
  - Method definition (don't use arrow for methods)
  - Generator function (can't be arrow)
  - **Challenge:** 15টা scenario — arrow vs regular output compare করো

### Task 14 — new Keyword Internals 🟡

- `new` keyword কী করে (step by step):
  1. Empty object create করে `{}`
  2. Object এর `__proto__` = Constructor.prototype সেট করে
  3. Constructor function call করে `this` = new object
  4. If constructor returns object → use that, else → return new object
  - **নিজে implement করো:**
  ```js
  function myNew(Constructor, ...args) {
    // Step 1-4 implement করো
  }
  ```

  - **Output:** Working `myNew` function with test cases

### Task 15 — Closure — The Heart of JavaScript 🔴

- Closure deep dive:
  - Closure = function + its lexical environment
  - Private state with closures
  - Counter factory
  - Module pattern
  - Memoization with closure
  - Event handler closures
  - Loop + closure trap (var vs let)
  - IIFE (Immediately Invoked Function Expression)
  - Closure memory implications
  - **Challenge:** 25টা closure puzzle solve করো
  - **Output:** Closure playground page — interactive demos of all patterns

### Task 16 — Scope & Closure Interview Challenges 🔴

- 20টা tricky problems:

  ```js
  // Example 1:
  for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000);
  }
  // Output? Fix it 3 ways (let, IIFE, bind)

  // Example 2:
  function createCounter() {
    let count = 0;
    return {
      increment: () => ++count,
      getCount: () => count,
    };
  }
  // Can we access count directly? Why not?
  ```

  - 20টা এরকম tricky problems তৈরি করো
  - প্রতিটার explanation লেখো
  - **Output:** Interview prep quiz page

### Task 17 — eval() ও with() — কেন Avoid করবে 🟢

- eval():
  - কি করে (string → code execution)
  - Security risk (code injection)
  - Performance cost (can't optimize)
  - Alternatives: `Function()` constructor, `JSON.parse()`
  - **When eval is acceptable:** JSON parsing (legacy), dynamic code (sandboxed)
- with():
  - কি করে (extends scope chain)
  - Ambiguity problem
  - Performance cost
  - Removed in strict mode

### Task 18 — Variable Declaration Deep Dive 🟡

- `var` vs `let` vs `const` — internal differences:
  - var: function-scoped, hoisted, re-declarable
  - let: block-scoped, TDZ, not re-declarable
  - const: block-scoped, TDZ, not re-assignable (but mutable if object)
  - Internal: var → Variable Environment, let/const → Lexical Environment
  - **Edge cases:**
    - `const obj = {}; obj.x = 1;` → OK (mutation, not reassignment)
    - `const arr = []; arr.push(1);` → OK
    - for loop: `for (const x of [1,2,3])` → OK (new binding each iteration)
  - **Challenge:** 15টা edge case problems

---

## Section C: Event Loop & Concurrency (Tasks 19-26)

### Task 19 — Event Loop Complete Visualization 🔴

- Event Loop architecture:
  - Call Stack
  - Web APIs / Node APIs (setTimeout, fetch, DOM events)
  - Callback Queue (Task Queue / Macrotask Queue)
  - Microtask Queue (Promise callbacks, queueMicrotask, MutationObserver)
  - **Priority:** Microtask > Macrotask
  - **Flow:** Stack empty → drain ALL microtasks → ONE macrotask → repeat
  - **Output:** Animated event loop visualizer (HTML + CSS + JS)

### Task 20 — Microtasks vs Macrotasks 🔴

- Macrotasks: setTimeout, setInterval, setImmediate (Node), I/O, UI rendering
- Microtasks: Promise.then/catch/finally, queueMicrotask, MutationObserver, process.nextTick (Node)
- **Challenge:** 25টা output prediction puzzle:
  ```js
  console.log('1');
  setTimeout(() => console.log('2'), 0);
  Promise.resolve().then(() => console.log('3'));
  console.log('4');
  // Output: 1, 4, 3, 2 — কেন?
  ```

  - 25টা increasingly complex puzzles solve করো
  - **Output:** Event loop quiz page

### Task 21 — setTimeout & setInterval Deep Dive 🟡

- Internal behavior:
  - `setTimeout(fn, 0)` — actually minimum ~4ms (browser), ~1ms (Node)
  - `setTimeout` guarantee: "at LEAST after X ms", not "exactly after X ms"
  - `setInterval` drift problem ও fix (self-adjusting timer)
  - Recursive setTimeout vs setInterval — কোনটা better
  - Timer coalescing in background tabs
  - `clearTimeout`, `clearInterval`
  - **Challenge:** Build:
    1. Accurate stopwatch (handles drift)
    2. Countdown timer
    3. Debounce function
    4. Throttle function

### Task 22 — requestAnimationFrame Mastery 🟡

- Why rAF > setTimeout for animation:
  - Syncs with display refresh rate (~60fps)
  - Pauses in background tabs (saves battery)
  - Browser optimizes rendering
  - **Build:**
    1. Smooth animation (moving ball)
    2. FPS counter
    3. Animation with easing functions
    4. Cancelable animation with cancelAnimationFrame

### Task 23 — requestIdleCallback 🟡

- Background work:
  - কখন use করবে: non-urgent work
  - `deadline.timeRemaining()` check করো
  - **Build:** Background task scheduler — large data processing without blocking UI
  - Polyfill for unsupported browsers

### Task 24 — queueMicrotask & MutationObserver 🟡

- queueMicrotask:
  - Schedule microtask manually
  - Use case: ensure code runs after current task but before next macrotask
  - Compare with Promise.resolve().then()
- MutationObserver:
  - Watch DOM changes
  - **Build:** DOM change logger — observe ও log all DOM mutations
  - Configure: childList, attributes, characterData, subtree

### Task 25 — Node.js Event Loop Differences 🔴

- Node.js event loop phases:
  1. **timers** — setTimeout, setInterval callbacks
  2. **pending callbacks** — I/O callbacks
  3. **idle, prepare** — internal
  4. **poll** — I/O events
  5. **check** — setImmediate callbacks
  6. **close callbacks** — socket.close()
  - `process.nextTick` — runs between phases (before any microtask)
  - **Challenge:** 15টা Node.js event loop puzzles (setTimeout vs setImmediate vs nextTick)

### Task 26 — Web Workers & Multithreading 🟡

- True parallel execution:
  - Main thread vs Worker thread
  - `new Worker('worker.js')`
  - `postMessage` ও `onmessage` communication
  - Transferable objects (ArrayBuffer)
  - SharedArrayBuffer ও Atomics
  - **Build:**
    1. Heavy computation in worker (fibonacci, prime numbers)
    2. Image processing worker (grayscale converter)
    3. Worker pool (manage multiple workers)
  - Limitations: no DOM access, no `window`

---

## Section D: Type System & Coercion (Tasks 27-30)

### Task 27 — Type Coercion Complete Guide 🔴

- Implicit coercion rules:
  - String coercion: `value + ""`, `String(value)`, template literals
  - Number coercion: `+value`, `Number(value)`, math operators
  - Boolean coercion: `!!value`, `Boolean(value)`, conditionals
  - **Coercion table:** document every combination:
    - `"" + 1`, `"5" - 3`, `true + true`, `null + 1`, `undefined + 1`
    - `[] + []`, `[] + {}`, `{} + []`, `{} + {}`
    - `null == undefined`, `NaN == NaN`, `0 == false`, `"" == false`
  - **Challenge:** 30টা coercion output prediction
  - **Output:** Complete coercion cheat sheet page

### Task 28 — == vs === Deep Dive 🟡

- Abstract Equality (==) algorithm:
  - Step by step spec reading
  - When types are same → same as ===
  - null == undefined → true
  - Number vs String → String to Number
  - Boolean vs anything → Boolean to Number first
  - Object vs Primitive → ToPrimitive
  - **Challenge:** Implement `abstractEquality(a, b)` that follows spec exactly

### Task 29 — typeof, instanceof, Object.prototype.toString 🟡

- Type checking methods:
  - `typeof` quirks: `typeof null === "object"` (bug since 1995)
  - `typeof NaN === "number"` (technically correct)
  - `typeof function() {} === "function"` (not a separate type)
  - `instanceof` — checks prototype chain
  - `Object.prototype.toString.call(value)` — most reliable
  - **Build:** Universal type checker function:
  ```js
  function getType(value) {
    // Returns: "string", "number", "boolean", "null", "undefined",
    // "array", "object", "function", "date", "regexp", "map", "set", etc.
  }
  ```

### Task 30 — Symbol.toPrimitive & Type Conversion Hooks 🟡

- Object to Primitive conversion:
  - `[Symbol.toPrimitive](hint)` — hint is "number", "string", or "default"
  - `valueOf()` — for numeric context
  - `toString()` — for string context
  - Conversion order: toPrimitive → valueOf → toString
  - **Build:** Custom objects with different conversion behaviors:
  ```js
  const money = {
    amount: 100,
    currency: 'BDT',
    [Symbol.toPrimitive](hint) {
      if (hint === 'number') return this.amount;
      if (hint === 'string') return `${this.amount} ${this.currency}`;
      return this.amount;
    },
  };
  // +money → 100
  // `${money}` → "100 BDT"
  // money + 50 → 150
  ```

---

## ✅ Phase 01 Completion Checklist

- [ ] JS engine কিভাবে code execute করে diagram আঁকতে পারো
- [ ] Execution Context, Call Stack clearly explain করতে পারো
- [ ] Memory model (Stack vs Heap) বোঝো
- [ ] Garbage collection কিভাবে কাজ করে বোঝো
- [ ] Hoisting সব cases (var, let, const, function, class) জানো
- [ ] Scope chain ও Lexical Environment বোঝো
- [ ] `this` keyword 7টা rule জানো
- [ ] call/apply/bind নিজে implement করতে পারো
- [ ] Closure confidently explain ও ব্যবহার করতে পারো
- [ ] Event Loop (Browser + Node.js) fully বোঝো
- [ ] Microtask vs Macrotask priority জানো
- [ ] Type coercion rules জানো

> ✅ Ready? → **JS Phase 02: Data Types, Structures & Algorithms**
