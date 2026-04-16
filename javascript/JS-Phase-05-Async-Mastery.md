# JS Phase 05 — Async JavaScript Mastery (Tasks 121-150)

> Async হলো JavaScript এর superpower। Browser-ও block হয় না, server-ও handle করে হাজার request। কিন্তু internally কি হচ্ছে — সেটা বুঝতে হবে।

---

## Section A: Callback Pattern (Tasks 121-126)

### Task 121 — Callback Fundamentals 🟢

- Callback pattern:
  - Function passed as argument, called later
  - Synchronous callbacks: `[1,2,3].forEach(cb)`, `Array.sort(cb)`
  - Asynchronous callbacks: `setTimeout(cb)`, `fs.readFile(path, cb)`
  - Error-first callback (Node.js convention):
    ```js
    function readFile(path, callback) {
      // callback(error, data)
      // Success: callback(null, data)
      // Error: callback(error, null)
    }
    ```
  - **Build:** 5 async operations using callbacks

### Task 122 — Callback Hell & Inversion of Control 🟡

- Problems with callbacks:
  - Callback hell (pyramid of doom):
    ```js
    getUser(id, (err, user) => {
      getOrders(user.id, (err, orders) => {
        getItems(orders[0].id, (err, items) => {
          getPrice(items[0].id, (err, price) => {
            // 4 levels deep...
          });
        });
      });
    });
    ```
  - Inversion of control: we give control to external function
  - Trust issues: callback called twice? Never? Too early? Too late?
  - **Challenge:** Flatten callback hell using named functions

### Task 123 — Build Custom Callback Utilities 🟡

- **Build:**
  1. `parallel(tasks, callback)` — run all tasks concurrently, callback when all done
  2. `series(tasks, callback)` — run tasks one by one
  3. `waterfall(tasks, callback)` — pass result of each to next
  4. `race(tasks, callback)` — callback with first to finish
  5. `retry(task, times, callback)` — retry on failure
  - Basically async.js library functionality!

---

## Section B: Promise Deep Dive (Tasks 124-135)

### Task 124 — Promise Fundamentals 🟡

- Promise states:
  - **Pending** → **Fulfilled** (resolved with value)
  - **Pending** → **Rejected** (rejected with reason)
  - Once settled, never changes (immutable state)
  - ```js
    const promise = new Promise((resolve, reject) => {
      // async operation
      resolve(value); // or
      reject(error);
    });

    promise
      .then((value) => {
        /* success */
      })
      .catch((error) => {
        /* failure */
      })
      .finally(() => {
        /* always runs */
      });
    ```

  - **Build:** 10 async operations converted from callback to Promise

### Task 125 — Promise Chaining Deep Dive 🟡

- Chaining rules:
  - `.then()` returns a NEW Promise
  - Return value becomes next `.then()`'s argument
  - Return Promise → next `.then()` waits for it
  - Throw error → skips to next `.catch()`
  - `.catch()` returns a Promise too (recovery)
  - ```js
    fetch('/api/user')
      .then((res) => res.json()) // Returns Promise
      .then((user) => fetch(`/api/orders/${user.id}`)) // Returns Promise
      .then((res) => res.json())
      .then((orders) => console.log(orders))
      .catch((err) => console.error(err)); // Catches ANY error above
    ```
  - **Challenge:** 10 Promise chaining puzzles — predict the output

### Task 126 — Promise Static Methods 🟡

- All static methods:
  - `Promise.all([p1, p2, p3])` — wait for ALL, fail if ANY fails
  - `Promise.allSettled([p1, p2, p3])` — wait for ALL regardless of success/failure
  - `Promise.race([p1, p2, p3])` — first to settle (success or failure)
  - `Promise.any([p1, p2, p3])` — first to SUCCEED (ES2021)
  - `Promise.resolve(value)` — wrap value in resolved Promise
  - `Promise.reject(reason)` — wrap in rejected Promise
  - `Promise.withResolvers()` — returns {promise, resolve, reject} (ES2024)
  - **Build:** Real examples for each:
    1. `Promise.all` — fetch user profile + posts + comments simultaneously
    2. `Promise.allSettled` — send notifications to multiple channels (some may fail)
    3. `Promise.race` — timeout pattern (race against setTimeout)
    4. `Promise.any` — try multiple API servers, use fastest response

### Task 127 — Build Promise From Scratch 🔴

- Implement entire Promise/A+ spec:
  ```js
  class MyPromise {
    #state = 'pending';
    #value = undefined;
    #handlers = [];

    constructor(executor) {
      const resolve = (value) => {
        /* ... */
      };
      const reject = (reason) => {
        /* ... */
      };
      try {
        executor(resolve, reject);
      } catch (err) {
        reject(err);
      }
    }

    then(onFulfilled, onRejected) {
      /* returns new MyPromise */
    }
    catch(onRejected) {
      return this.then(null, onRejected);
    }
    finally(onFinally) {
      /* ... */
    }

    static all(promises) {
      /* ... */
    }
    static allSettled(promises) {
      /* ... */
    }
    static race(promises) {
      /* ... */
    }
    static any(promises) {
      /* ... */
    }
    static resolve(value) {
      /* ... */
    }
    static reject(reason) {
      /* ... */
    }
  }
  ```

  - Must handle:
    - Microtask scheduling (`queueMicrotask`)
    - Thenable objects (objects with `.then()` method)
    - Promise resolution procedure (recursive unwrapping)
    - Multiple `.then()` on same promise
    - Chain of promises
  - **Test:** Run Promises/A+ test suite against your implementation

### Task 128 — Microtasks vs Macrotasks (Promise Edition) 🔴

- Execution order:
  ```js
  console.log('1');
  setTimeout(() => console.log('2'), 0);
  Promise.resolve().then(() => console.log('3'));
  Promise.resolve().then(() => setTimeout(() => console.log('4'), 0));
  Promise.resolve().then(() => console.log('5'));
  console.log('6');
  // Output: 1, 6, 3, 5, 2, 4
  ```

  - Microtask queue: Promise callbacks, queueMicrotask, MutationObserver
  - Macrotask queue: setTimeout, setInterval, I/O, UI rendering
  - Microtask queue drains completely before next macrotask
  - **Challenge:** 20 execution order puzzles (mixed sync/Promise/setTimeout)

### Task 129 — Common Promise Anti-Patterns 🟡

- Mistakes to avoid:
  1. **Promise constructor anti-pattern:** unnecessary `new Promise` wrapping
  2. **Broken chain:** forgetting to return Promise in `.then()`
  3. **Catch placement:** `.catch()` at wrong position
  4. **Swallowed errors:** missing `.catch()` entirely
  5. **Sequential when could be parallel:** `await a; await b;` vs `Promise.all`
  6. **forEach with async:** `array.forEach(async fn)` doesn't wait
  7. **Racing condition:** unchecked Promise race conditions
  - **Challenge:** Fix 10 buggy Promise code snippets

### Task 130 — Promise Utility Functions 🟡

- **Build:**
  1. `promisify(fn)` — convert callback function to Promise:
     ```js
     const readFile = promisify(fs.readFile);
     const data = await readFile('file.txt');
     ```
  2. `delay(ms)` — Promise that resolves after ms
  3. `timeout(promise, ms)` — reject if too slow
  4. `retry(fn, times, delay)` — retry Promise-based function
  5. `throttlePromises(fns, limit)` — run max `limit` Promises concurrently
  6. `deferred()` — externally controllable Promise
  7. `cancellable(promise)` — AbortController-style cancellation

---

## Section C: Async/Await Mastery (Tasks 131-140)

### Task 131 — Async/Await Fundamentals 🟡

- Syntactic sugar over Promises:
  - `async function` always returns a Promise
  - `await` pauses execution until Promise settles
  - `await` unwraps Promise value
  - Error handling: `try/catch` instead of `.catch()`
  - ```js
    async function getUser(id) {
      try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const user = await response.json();
        return user;
      } catch (error) {
        console.error('Failed:', error);
        throw error; // re-throw for caller
      }
    }
    ```
  - Top-level await (ES2022 — in modules)

### Task 132 — Async Patterns & Best Practices 🟡

- Common patterns:
  1. **Sequential:** `const a = await getA(); const b = await getB(a);`
  2. **Parallel:** `const [a, b] = await Promise.all([getA(), getB()]);`
  3. **Parallel with error handling:** `Promise.allSettled()`
  4. **First success:** `await Promise.any([api1(), api2()])`
  5. **Loop sequential:** `for (const item of items) { await process(item); }`
  6. **Loop parallel:** `await Promise.all(items.map(process))`
  7. **Conditional async:** `const data = cache ?? await fetchData();`
  - **Challenge:** Refactor 10 Promise chains to async/await

### Task 133 — Error Handling in Async 🟡

- Error strategies:
  1. **try/catch per function:**
     ```js
     async function save(data) {
       try {
         return await db.save(data);
       } catch (err) {
         throw new SaveError(err);
       }
     }
     ```
  2. **Go-style tuple:** `const [err, data] = await to(fetchData());`
     ```js
     function to(promise) {
       return promise.then((data) => [null, data]).catch((err) => [err, null]);
     }
     ```
  3. **Higher-order function:** `const safeFetch = withErrorHandler(fetch);`
  4. **Global unhandled rejection:** `process.on('unhandledRejection', handler)`
  - **Build:** Error handling utility library (all 4 patterns)

### Task 134 — Async Iteration 🔴

- `for await...of`:

  ```js
  async function* fetchPages(url) {
    let page = 1;
    while (true) {
      const response = await fetch(`${url}?page=${page}`);
      const data = await response.json();
      if (data.length === 0) return;
      yield data;
      page++;
    }
  }

  for await (const page of fetchPages('/api/users')) {
    console.log('Page:', page);
  }
  ```

  - Async generators: `async function*`
  - Async iterators: `[Symbol.asyncIterator]()`
  - **Build:** Streaming data processor (read large file chunk by chunk)
  - **Build:** Real-time feed reader (SSE/WebSocket data stream)

### Task 135 — AbortController & Cancellation 🟡

- Cancel async operations:

  ```js
  const controller = new AbortController();

  fetch('/api/data', { signal: controller.signal })
    .then((res) => res.json())
    .catch((err) => {
      if (err.name === 'AbortError') console.log('Cancelled');
      else throw err;
    });

  // Cancel after 5 seconds
  setTimeout(() => controller.abort(), 5000);
  ```

  - Cancellable operations: fetch, addEventListener, streams
  - `AbortSignal.timeout(ms)` — auto-cancel after timeout
  - `AbortSignal.any([signal1, signal2])` — cancel if any signal fires
  - **Build:** Search with auto-cancel (new search cancels previous)
  - **Build:** File upload with cancel button

### Task 136 — Concurrent Async Patterns 🔴

- Advanced concurrency:
  1. **Pool/Semaphore:** Limit concurrent operations
     ```js
     class AsyncPool {
       #running = 0;
       #queue = [];
       constructor(limit) {
         this.limit = limit;
       }
       async add(fn) {
         if (this.#running >= this.limit) {
           await new Promise((r) => this.#queue.push(r));
         }
         this.#running++;
         try {
           return await fn();
         } finally {
           this.#running--;
           if (this.#queue.length > 0) this.#queue.shift()();
         }
       }
     }
     ```
  2. **Mutex:** Only one operation at a time on shared resource
  3. **Rate limiter:** Max N operations per second
  4. **Circuit breaker:** Stop calling failing service
  - **Build:** All 4 patterns with tests

### Task 137 — Web Workers & Async Offloading 🟡

- Run CPU-intensive tasks off main thread:

  ```js
  // main.js
  const worker = new Worker('worker.js');
  worker.postMessage({ type: 'SORT', data: largeArray });
  worker.onmessage = (e) => console.log('Sorted:', e.data);

  // worker.js
  self.onmessage = (e) => {
    if (e.data.type === 'SORT') {
      const sorted = e.data.data.sort((a, b) => a - b);
      self.postMessage(sorted);
    }
  };
  ```

  - Transferable objects (zero-copy transfer)
  - SharedArrayBuffer ও Atomics
  - Worker pools
  - **Build:** Image processing in Web Worker (grayscale, blur, resize)

### Task 138 — Streams API 🔴

- Process data in chunks:
  - ReadableStream: `new ReadableStream({ start(controller) { ... } })`
  - WritableStream: `new WritableStream({ write(chunk) { ... } })`
  - TransformStream: pipe readable → writable with transformation
  - `fetch` response as stream:
    ```js
    const response = await fetch('/large-file');
    const reader = response.body.getReader();
    while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      processChunk(value);
    }
    ```
  - **Build:** Progress indicator for large file download
  - **Build:** Streaming JSON parser

### Task 139 — Server-Sent Events & WebSocket 🟡

- Real-time communication:
  - **SSE (one-way server → client):**
    ```js
    const eventSource = new EventSource('/api/stream');
    eventSource.onmessage = (e) => console.log(e.data);
    eventSource.addEventListener('custom-event', handler);
    eventSource.close();
    ```
  - **WebSocket (two-way):**
    ```js
    const ws = new WebSocket('ws://localhost:8080');
    ws.onopen = () => ws.send('Hello');
    ws.onmessage = (e) => console.log(e.data);
    ws.onclose = () => console.log('Disconnected');
    ws.onerror = (err) => console.error(err);
    ```
  - **Build:** Real-time chat using WebSocket
  - **Build:** Live notification system using SSE
  - Auto-reconnect ও heartbeat implementation

### Task 140 — Async Testing 🟡

- Testing async code:
  - Testing Promises: `return promise` or `async/await`
  - Fake timers: `jest.useFakeTimers()`
  - Mock fetch: `jest.mock()` or MSW (Mock Service Worker)
  - Testing race conditions
  - Testing error cases
  - **Build:** Complete test suite for:
    1. Promise-based API client
    2. Async data processing pipeline
    3. WebSocket connection handler
    4. Retry ও circuit breaker logic

---

## Section D: Advanced Async Concepts (Tasks 141-150)

### Task 141 — Scheduler & Priority Queue for Async Tasks 🔴

- **Build:** Task scheduler:
  ```js
  class TaskScheduler {
    addTask(task, priority) {
      /* ... */
    }
    async run() {
      // Execute tasks by priority
      // Respect concurrency limit
      // Handle failures ও retries
    }
  }
  ```

  - Features: priority levels, concurrency limit, retry, timeout, dependencies

### Task 142 — Async State Machine 🔴

- State machine with async transitions:
  ```js
  const machine = createAsyncMachine({
    initial: 'idle',
    states: {
      idle: { on: { FETCH: { target: 'loading', action: startFetch } } },
      loading: {
        on: {
          SUCCESS: { target: 'success', action: setData },
          ERROR: { target: 'error', action: setError },
        },
      },
      success: { on: { REFRESH: { target: 'loading', action: startFetch } } },
      error: { on: { RETRY: { target: 'loading', action: startFetch } } },
    },
  });
  ```

  - **Build:** Data fetching state machine (idle → loading → success/error)
  - **Build:** Multi-step form with async validation at each step

### Task 143 — Event Loop Visualizer 🔴

- **Build:** Interactive event loop visualizer:
  - Show call stack
  - Show Web APIs
  - Show callback queue (macrotask)
  - Show microtask queue
  - Step-by-step execution
  - User inputs code → see how it executes
  - Like Loupe (latentflip.com/loupe) but your own
  - **Output:** Educational tool for understanding event loop

### Task 144 — Concurrent Data Structures 🔴

- Thread-safe data in JS:
  - SharedArrayBuffer + Atomics for multi-threaded access
  - `Atomics.load()`, `Atomics.store()`, `Atomics.add()`, `Atomics.compareExchange()`
  - `Atomics.wait()` ও `Atomics.notify()` — inter-worker synchronization
  - **Build:** Shared counter between main thread ও workers
  - **Build:** Producer-consumer queue using SharedArrayBuffer

### Task 145 — Promise Pool Pattern 🟡

- Control concurrency:
  ```js
  async function promisePool(tasks, poolLimit) {
    const results = [];
    const executing = new Set();

    for (const [i, task] of tasks.entries()) {
      const p = Promise.resolve().then(() => task());
      results[i] = p;
      executing.add(p);

      const clean = () => executing.delete(p);
      p.then(clean, clean);

      if (executing.size >= poolLimit) {
        await Promise.race(executing);
      }
    }

    return Promise.all(results);
  }
  ```

  - **Build:** Image gallery loader (max 3 concurrent downloads)
  - **Build:** API batch processor (100 requests, max 5 concurrent)

### Task 146 — Async Context (AsyncLocalStorage) 🔴

- Track context across async operations:
  - Node.js `AsyncLocalStorage`:

    ```js
    const { AsyncLocalStorage } = require('async_hooks');
    const storage = new AsyncLocalStorage();

    storage.run({ requestId: '123' }, async () => {
      // Any async code here can access the context
      console.log(storage.getStore().requestId); // '123'
      await someAsyncOperation(); // still accessible inside
    });
    ```

  - Use cases: request tracing, logging, user context
  - TC39 `AsyncContext` proposal
  - **Build:** Request context tracker (attach requestId to all logs)

### Task 147 — Build: Async Task Runner ⚫

- Full-featured task runner:
  - Define tasks with dependencies
  - Parallel execution where possible
  - Sequential where required (dependencies)
  - Retry failed tasks
  - Timeout per task
  - Progress reporting
  - Cancel all tasks
  - **Example:**

    ```js
    const runner = new TaskRunner();
    runner.add('fetchUser', () => fetchUser(1));
    runner.add('fetchPosts', () => fetchPosts(1), { depends: ['fetchUser'] });
    runner.add('fetchComments', () => fetchComments(), { parallel: true });
    runner.add('combine', (results) => combine(results), {
      depends: ['fetchPosts', 'fetchComments'],
    });

    const results = await runner.run();
    ```

### Task 148 — Build: Fetch Wrapper Library ⚫

- Production-grade HTTP client:
  - Base URL configuration
  - Request/response interceptors
  - Automatic JSON parsing
  - Timeout (AbortController-based)
  - Retry with exponential backoff
  - Request deduplication
  - Cache (configurable TTL)
  - Error handling ও custom error types
  - TypeScript-friendly (JSDoc types)
  - **Output:** Complete fetch wrapper (like axios but lighter)

### Task 149 — Build: Async Queue System ⚫

- Job queue:
  - Add jobs to queue
  - Process jobs sequentially or with concurrency limit
  - Priority levels
  - Dead letter queue (failed jobs after max retries)
  - Job status tracking (pending, running, completed, failed)
  - Event hooks: onJobStart, onJobComplete, onJobFail
  - Persistent storage (localStorage/IndexedDB)
  - **Output:** Production job queue system

### Task 150 — Async Mastery Final Challenge ⚫

- Phase 05 graduation project:
  - **Build:** Real-time data dashboard:
    - WebSocket connection for live data
    - SSE for notifications
    - Parallel API calls for initial data
    - AbortController for cleanup
    - Web Worker for data processing
    - Stream API for large data
    - Promise pool for batch operations
    - Retry ও circuit breaker for resilience
    - All with proper error handling
    - **Must use:** Every async concept from this phase
  - **Output:** Full working dashboard

---

## ✅ Phase 05 Completion Checklist

- [ ] Callback pattern ও callback hell বোঝো
- [ ] Promise internally কিভাবে কাজ করে বলতে পারো
- [ ] Promise নিজে from scratch build করতে পারো
- [ ] Microtask vs Macrotask execution order predict করতে পারো
- [ ] async/await confidently ব্যবহার করতে পারো
- [ ] Error handling strategies জানো (try/catch, Go-style, HOF)
- [ ] Async iteration ও generators পারো
- [ ] AbortController ব্যবহার করতে পারো
- [ ] Concurrent patterns (pool, mutex, rate limiter, circuit breaker) পারো
- [ ] Web Workers, Streams, SSE, WebSocket ব্যবহার করতে পারো
- [ ] Async testing confidently করতে পারো

> ✅ Ready? → **JS Phase 06: Browser & Web APIs**
