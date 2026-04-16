# Backend Phase 03 — Node.js Deep Internals (Tasks 66-105)

> **Node.js দিয়ে code লেখো সবাই। কিন্তু Node.js কিভাবে কাজ করে — এটা জানে 1%।**
> **V8, libuv, Event Loop, Streams, Buffers, Cluster — সব internals master করবে এই phase এ।**
> **এটা শেষ করলে তুমি Node.js এর doctor — কোনো problem diagnose করতে পারবে।**

---

## Section A: Node.js Architecture (Tasks 66-75)

### Task 66 — Node.js Architecture Deep Dive 🟡

- Node.js = V8 + libuv + Core Modules:
  ```
  Your JavaScript Code
         ↓
  ┌─────────────────────────────┐
  │      Node.js Bindings       │  ← C++ bridge between JS and C
  ├─────────────┬───────────────┤
  │    V8       │    libuv      │
  │ (JS Engine) │ (Async I/O)   │
  │             │               │
  │ Parsing     │ Event Loop    │
  │ Compilation │ Thread Pool   │
  │ Execution   │ File System   │
  │ GC          │ Networking    │
  │             │ DNS           │
  │             │ Child Process │
  └─────────────┴───────────────┘
         ↓              ↓
      JavaScript     Operating System
      Execution      (Linux/macOS/Windows)
  ```

  - **V8:** JavaScript → Machine Code (JIT compilation)
  - **libuv:** Cross-platform async I/O library (C)
  - **Exercise:**
    1. Node.js source code browse করো (github.com/nodejs/node)
    2. `lib/` folder দেখো (JS modules) — `fs.js`, `http.js`, `net.js`
    3. `src/` folder দেখো (C++ bindings)
    4. `deps/` folder দেখো (V8, libuv, OpenSSL, zlib)

### Task 67 — Event Loop — The Heart of Node.js 🔴

- How Node.js handles async operations:

  ```
  ┌───────────────────────────┐
  ┌→│         Timers            │  ← setTimeout, setInterval
  │ └───────────┬───────────────┘
  │ ┌───────────┴───────────────┐
  │ │    Pending Callbacks      │  ← System-level callbacks (TCP errors)
  │ └───────────┬───────────────┘
  │ ┌───────────┴───────────────┐
  │ │     Idle, Prepare         │  ← Internal use only
  │ └───────────┬───────────────┘
  │ ┌───────────┴───────────────┐
  │ │         Poll              │  ← I/O callbacks (fs, network)
  │ └───────────┬───────────────┘     Calculates how long to block
  │ ┌───────────┴───────────────┐
  │ │         Check             │  ← setImmediate
  │ └───────────┬───────────────┘
  │ ┌───────────┴───────────────┐
  │ │     Close Callbacks       │  ← socket.on('close')
  │ └───────────┬───────────────┘
  └─────────────┘

  Between EACH phase: process ALL microtasks
  ├── process.nextTick() queue (highest priority)
  └── Promise callbacks (.then, .catch, .finally)
  ```

  - **Execution Order Quiz:**

    ```js
    console.log('1: Start');

    setTimeout(() => console.log('2: setTimeout'), 0);
    setImmediate(() => console.log('3: setImmediate'));

    Promise.resolve().then(() => console.log('4: Promise'));
    process.nextTick(() => console.log('5: nextTick'));

    console.log('6: End');

    // Output:
    // 1: Start
    // 6: End
    // 5: nextTick
    // 4: Promise
    // 2: setTimeout (OR 3: setImmediate — order not guaranteed at top level)
    // 3: setImmediate (OR 2: setTimeout)
    ```

  - **Exercise:**
    1. 10টা Event Loop ordering puzzle solve করো
    2. Event Loop visualizer build করো
    3. Understand: কেন `setTimeout(fn, 0)` actually 0ms না
    4. `process.nextTick` vs `Promise` vs `setImmediate` timing difference

### Task 68 — libuv Thread Pool 🟡

- Node.js isn't truly single-threaded:

  ```
  Main Thread (Event Loop)
  ├── JavaScript execution
  ├── Microtask processing
  └── Non-blocking I/O (network via epoll/kqueue)

  Thread Pool (default: 4 threads, max: 1024)
  ├── fs operations (read, write, stat)
  ├── DNS lookups (dns.lookup)
  ├── crypto operations (pbkdf2, randomBytes)
  ├── zlib compression
  └── Some custom C++ addons
  ```

  - **Thread pool size:**
    ```bash
    # Default: 4 threads
    UV_THREADPOOL_SIZE=8 node app.js  # Increase to 8
    # Max: 1024
    ```
  - **Thread pool starvation:**

    ```js
    const crypto = require('crypto');
    const fs = require('fs');

    // 4 crypto operations will use all 4 threads
    // 5th fs operation must WAIT for a thread!
    for (let i = 0; i < 4; i++) {
      crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', () => {
        console.log(`Crypto ${i} done at ${Date.now() - start}ms`);
      });
    }

    // This will be DELAYED because thread pool is busy!
    fs.readFile('test.txt', () => {
      console.log(`File read done at ${Date.now() - start}ms`);
    });

    const start = Date.now();
    ```

  - **Exercise:**
    1. Thread pool starvation reproduce করো
    2. `UV_THREADPOOL_SIZE` experiment করো
    3. Which operations use thread pool vs epoll — list তৈরি করো

### Task 69 — Buffers & Binary Data 🟡

- Raw binary data handling:

  ```js
  // Buffer = fixed-size chunk of memory (outside V8 heap)

  // Create
  const buf1 = Buffer.alloc(10); // 10 bytes, filled with 0
  const buf2 = Buffer.from('Hello'); // From string (UTF-8)
  const buf3 = Buffer.from([0x48, 0x65]); // From byte array

  // Read/Write
  buf1.writeUInt32BE(12345, 0); // Write 4-byte integer at offset 0
  buf1.readUInt32BE(0); // Read back: 12345

  // String conversion
  buf2.toString('utf8'); // "Hello"
  buf2.toString('hex'); // "48656c6c6f"
  buf2.toString('base64'); // "SGVsbG8="

  // Buffer concatenation
  const combined = Buffer.concat([buf1, buf2, buf3]);

  // Comparison
  Buffer.compare(buf1, buf2); // -1, 0, or 1

  // Copy
  buf2.copy(buf1, 0, 0, 5); // Copy buf2[0:5] to buf1[0:]

  // Pool allocation (performance)
  // Buffers < 8KB use a shared pool (Buffer.poolSize = 8192)
  // Buffers >= 8KB are directly allocated from OS
  ```

  - **Exercise:**
    1. Binary protocol parser build করো (read headers, payload from buffer)
    2. Image file header parser (detect PNG, JPEG, GIF from magic bytes)
    3. Buffer pool implement করো for performance

### Task 70 — Streams — Node.js Superpower 🔴

- Process data piece by piece (don't load everything in memory):

  ```js
  const { Readable, Writable, Transform, Duplex, pipeline } = require('stream');
  const fs = require('fs');

  // 4 types of streams:
  // 1. Readable — data source (fs.createReadStream, http request)
  // 2. Writable — data destination (fs.createWriteStream, http response)
  // 3. Duplex   — both (TCP socket)
  // 4. Transform — modify data passing through (zlib, crypto)

  // WRONG: Loading entire file into memory
  const data = fs.readFileSync('huge-file.csv'); // 2GB file = 2GB RAM! 💀

  // RIGHT: Stream it (constant memory)
  const readStream = fs.createReadStream('huge-file.csv');
  const writeStream = fs.createWriteStream('output.csv');

  // Custom Transform stream
  class CSVFilter extends Transform {
    _transform(chunk, encoding, callback) {
      const lines = chunk.toString().split('\n');
      const filtered = lines.filter((line) => line.includes('active'));
      callback(null, filtered.join('\n') + '\n');
    }
  }

  // Pipeline (proper error handling)
  const { pipeline: pipelineAsync } = require('stream/promises');

  async function processCSV() {
    await pipelineAsync(
      fs.createReadStream('huge-file.csv'),
      new CSVFilter(),
      fs.createWriteStream('active-users.csv')
    );
    console.log('Done! Memory used:', process.memoryUsage().heapUsed);
  }

  // Custom Readable
  class NumberStream extends Readable {
    #current = 0;
    #max;
    constructor(max) {
      super({ objectMode: true });
      this.#max = max;
    }
    _read() {
      if (this.#current >= this.#max) {
        this.push(null); // Signal end
      } else {
        this.push({ number: this.#current++ });
      }
    }
  }
  ```

  - **Backpressure:**

    ```js
    // If writable is slower than readable, data backs up in memory
    // Streams handle this automatically with backpressure
    readable.pipe(writable); // Handles backpressure

    // Manual handling:
    readable.on('data', (chunk) => {
      const canContinue = writable.write(chunk);
      if (!canContinue) {
        readable.pause(); // Stop reading!
        writable.once('drain', () => readable.resume()); // Resume when ready
      }
    });
    ```

  - **Exercise:**
    1. File copy using streams (handle backpressure)
    2. HTTP file download with progress bar
    3. CSV to JSON transformer stream
    4. Log file analyzer using streams (count errors, calculate stats)
    5. Compressed file streamer (read → gzip → write)

### Task 71 — Cluster Mode & Worker Threads 🔴

- Use all CPU cores:

  ```js
  // Cluster: multiple Node.js processes (separate memory)
  const cluster = require('cluster');
  const http = require('http');
  const os = require('os');

  if (cluster.isPrimary) {
    const numCPUs = os.cpus().length;
    console.log(`Primary ${process.pid} starting ${numCPUs} workers`);

    for (let i = 0; i < numCPUs; i++) {
      cluster.fork();
    }

    cluster.on('exit', (worker, code) => {
      console.log(`Worker ${worker.process.pid} died (code: ${code}). Restarting...`);
      cluster.fork(); // Auto-restart
    });
  } else {
    http
      .createServer((req, res) => {
        res.writeHead(200);
        res.end(`Worker ${process.pid}\n`);
      })
      .listen(3000);

    console.log(`Worker ${process.pid} started`);
  }
  ```

  - **Worker Threads: shared memory, for CPU tasks:**

    ```js
    const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

    if (isMainThread) {
      // Main thread: distribute work
      const workers = [];
      const data = Array.from({ length: 1000000 }, (_, i) => i);
      const chunkSize = Math.ceil(data.length / 4);

      for (let i = 0; i < 4; i++) {
        const chunk = data.slice(i * chunkSize, (i + 1) * chunkSize);
        const worker = new Worker(__filename, { workerData: chunk });
        workers.push(
          new Promise((resolve) => {
            worker.on('message', resolve);
          })
        );
      }

      const results = await Promise.all(workers);
      const totalSum = results.reduce((a, b) => a + b, 0);
    } else {
      // Worker: compute sum
      const sum = workerData.reduce((a, b) => a + b, 0);
      parentPort.postMessage(sum);
    }
    ```

  - **Cluster vs Worker Threads:**
    ```
    Feature          Cluster              Worker Threads
    ──────────────── ──────────────────── ─────────────────
    Process/Thread   Separate processes   Threads (1 process)
    Memory           Separate             Can share (SharedArrayBuffer)
    Use case         HTTP servers         CPU computation
    Communication    IPC (serialized)     postMessage / SharedArrayBuffer
    Crash impact     Only that worker     Can crash entire process
    ```
  - **Exercise:**
    1. Cluster mode HTTP server build করো
    2. Worker thread দিয়ে image processing করো
    3. Benchmark: single thread vs cluster vs worker threads

### Task 72 — Child Processes 🟡

- Run external commands from Node.js:

  ```js
  const { exec, execFile, spawn, fork } = require('child_process');

  // exec: run command, buffer output (small output)
  exec('ls -la', (err, stdout, stderr) => {
    console.log(stdout);
  });

  // execFile: run file directly (safer, no shell injection)
  execFile('node', ['--version'], (err, stdout) => {
    console.log(stdout);
  });

  // spawn: stream output (large output)
  const child = spawn('find', ['.', '-name', '*.js']);
  child.stdout.on('data', (data) => console.log(data.toString()));
  child.stderr.on('data', (data) => console.error(data.toString()));
  child.on('close', (code) => console.log(`Exit code: ${code}`));

  // fork: spawn Node.js process with IPC
  const worker = fork('worker.js');
  worker.send({ task: 'compute', data: [1, 2, 3] });
  worker.on('message', (result) => console.log('Result:', result));
  ```

  - **Security Warning:** Never use `exec` with user input!

    ```js
    // ❌ DANGEROUS — command injection!
    exec(`ls ${userInput}`); // userInput = "; rm -rf /"

    // ✅ Safe — use execFile or spawn
    execFile('ls', [userInput]);
    ```

  - **Exercise:**
    1. Task queue: fork worker processes, distribute work
    2. FFmpeg wrapper: video conversion using spawn
    3. Shell command runner (with timeout ও resource limits)

### Task 73 — Crypto Module 🟡

- Cryptographic operations:

  ```js
  const crypto = require('crypto');

  // Hashing (one-way)
  const hash = crypto.createHash('sha256').update('password').digest('hex');

  // HMAC (hash with secret key)
  const hmac = crypto.createHmac('sha256', 'secret-key').update('data').digest('hex');

  // Password hashing (slow, safe)
  const salt = crypto.randomBytes(16).toString('hex');
  crypto.pbkdf2('password', salt, 100000, 64, 'sha512', (err, derivedKey) => {
    const hash = derivedKey.toString('hex');
  });

  // scrypt (better than pbkdf2)
  crypto.scrypt('password', salt, 64, (err, derivedKey) => {
    const hash = derivedKey.toString('hex');
  });

  // Encryption (AES-256-GCM — authenticated encryption)
  function encrypt(text, key) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv('aes-256-gcm', key, iv);
    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    const authTag = cipher.getAuthTag().toString('hex');
    return { iv: iv.toString('hex'), encrypted, authTag };
  }

  function decrypt(encryptedData, key) {
    const decipher = crypto.createDecipheriv(
      'aes-256-gcm',
      key,
      Buffer.from(encryptedData.iv, 'hex')
    );
    decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'hex'));
    let decrypted = decipher.update(encryptedData.encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
  }

  // Random values
  const token = crypto.randomBytes(32).toString('hex');
  const uuid = crypto.randomUUID();
  ```

  - **Exercise:**
    1. Password hash/verify system build করো
    2. Encrypt/decrypt user data
    3. HMAC-based webhook signature verification
    4. Token generation (API keys, reset tokens)

### Task 74 — Error Handling Patterns 🟡

- Proper error handling in Node.js:

  ```js
  // 1. Custom Error Classes
  class AppError extends Error {
    constructor(message, statusCode, code) {
      super(message);
      this.statusCode = statusCode;
      this.code = code;
      this.isOperational = true; // vs programmer errors
      Error.captureStackTrace(this, this.constructor);
    }
  }

  class NotFoundError extends AppError {
    constructor(resource = 'Resource') {
      super(`${resource} not found`, 404, 'NOT_FOUND');
    }
  }

  class ValidationError extends AppError {
    constructor(errors) {
      super('Validation failed', 400, 'VALIDATION_ERROR');
      this.errors = errors;
    }
  }

  // 2. Async error wrapper (no try-catch everywhere)
  const asyncHandler = (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };

  app.get(
    '/api/users/:id',
    asyncHandler(async (req, res) => {
      const user = await User.findById(req.params.id);
      if (!user) throw new NotFoundError('User');
      res.json(user);
    })
  );

  // 3. Global error handler
  app.use((err, req, res, next) => {
    const statusCode = err.statusCode || 500;
    const response = {
      success: false,
      error: {
        code: err.code || 'INTERNAL_ERROR',
        message: err.isOperational ? err.message : 'Something went wrong',
      },
    };

    if (process.env.NODE_ENV === 'development') {
      response.error.stack = err.stack;
    }

    // Log error
    if (!err.isOperational) {
      logger.error('Unhandled error', { error: err.message, stack: err.stack });
    }

    res.status(statusCode).json(response);
  });

  // 4. Process-level error handling
  process.on('uncaughtException', (err) => {
    logger.error('UNCAUGHT EXCEPTION', { error: err });
    gracefulShutdown(1);
  });

  process.on('unhandledRejection', (reason) => {
    logger.error('UNHANDLED REJECTION', { reason });
    gracefulShutdown(1);
  });
  ```

  - **Operational vs Programmer Errors:**

    ```
    Operational (expected, handle gracefully):
    ├── Invalid user input
    ├── Database connection lost
    ├── External API timeout
    └── File not found

    Programmer (bugs, crash & restart):
    ├── TypeError, ReferenceError
    ├── Accessing null property
    ├── Assertion failures
    └── Out of memory
    ```

  - **Exercise:**
    1. Full error handling system build করো (custom classes, global handler)
    2. Async error wrapper implement করো
    3. Error logging with context (request ID, user ID, stack trace)

### Task 75 — Module System (CJS vs ESM) 🟡

- CommonJS vs ES Modules:

  ```js
  // CommonJS (require/module.exports)
  const fs = require('fs');
  module.exports = { myFunction };
  module.exports.myFunction = fn;
  exports.myFunction = fn; // Shorthand

  // ES Modules (import/export)
  import fs from 'fs';
  import { readFile } from 'fs/promises';
  export function myFunction() {}
  export default myFunction;

  // Differences:
  // CJS: Synchronous loading, dynamic (can require inside if)
  // ESM: Async loading, static (imports hoisted, tree-shakeable)
  // CJS: module.exports is the actual export object
  // ESM: named exports + default export

  // In package.json:
  // "type": "module"  → .js files are ESM
  // "type": "commonjs" → .js files are CJS (default)
  // .mjs → always ESM
  // .cjs → always CJS
  ```

  - **Exercise:**
    1. Same module CJS ও ESM দুইভাবে লেখো
    2. Dual-package: CJS ও ESM both support করো (package.json exports)
    3. Module resolution algorithm understand করো

---

## Section B: Node.js Core Modules Mastery (Tasks 76-90)

### Task 76 — `fs` Module Deep Dive 🟡

- File system operations (all patterns):

  ```js
  const fs = require('fs');
  const fsp = require('fs/promises');

  // 1. Callback pattern (old)
  fs.readFile('file.txt', 'utf8', (err, data) => {});

  // 2. Sync pattern (blocks event loop! Only for startup)
  const data = fs.readFileSync('file.txt', 'utf8');

  // 3. Promise pattern (preferred)
  const data2 = await fsp.readFile('file.txt', 'utf8');

  // 4. Stream pattern (for large files)
  const stream = fs.createReadStream('huge.txt');

  // Watch for changes
  const watcher = fs.watch('src/', { recursive: true }, (eventType, filename) => {
    console.log(`${eventType}: ${filename}`);
  });

  // Temporary files
  const tmpDir = await fsp.mkdtemp(path.join(os.tmpdir(), 'app-'));
  // Use tmpDir... then cleanup
  ```

  - **Exercise:**
    1. File manager: CRUD operations, directory traversal, search
    2. File watcher: detect changes, trigger rebuild (like nodemon)
    3. Large file processor: line-by-line reading using streams

### Task 77 — `path` Module & File Paths 🟢

- Cross-platform path handling:

  ```js
  const path = require('path');

  path.join('/users', 'john', 'docs', 'file.txt'); // /users/john/docs/file.txt
  path.resolve('src', 'index.js'); // /full/absolute/path/src/index.js
  path.basename('/path/to/file.txt'); // file.txt
  path.dirname('/path/to/file.txt'); // /path/to
  path.extname('file.txt'); // .txt
  path.parse('/path/to/file.txt'); // { root, dir, base, ext, name }

  // SECURITY: Prevent path traversal
  function safePath(userInput) {
    const resolved = path.resolve('/uploads', userInput);
    if (!resolved.startsWith('/uploads')) {
      throw new Error('Path traversal detected!');
    }
    return resolved;
  }
  ```

  - **Exercise:** Safe file serving function build করো (prevent path traversal)

### Task 78 — `http` & `https` Module Internals 🟡

- How Node.js HTTP server actually works:

  ```js
  const http = require('http');

  // Connection pooling (client side)
  const agent = new http.Agent({
    keepAlive: true, // Reuse connections
    maxSockets: 50, // Max connections per host
    maxTotalSockets: 100, // Max total connections
    timeout: 60000, // Socket timeout
  });

  // Server events
  const server = http.createServer();
  server.on('request', (req, res) => {}); // New request
  server.on('connection', (socket) => {}); // New TCP connection
  server.on('upgrade', (req, socket) => {}); // WebSocket upgrade
  server.on('close', () => {}); // Server closed

  // Request events
  // req is a Readable Stream
  // res is a Writable Stream

  // Keep-alive settings
  server.keepAliveTimeout = 65000; // How long to keep idle connection
  server.headersTimeout = 66000; // Must be > keepAliveTimeout
  ```

  - **Exercise:**
    1. Connection pool behavior test করো (reuse vs new connections)
    2. Keep-alive performance benchmark করো
    3. HTTP server with all events logged

### Task 79 — `net` Module — Raw TCP 🟡

- Low-level TCP programming:
  - **Build:** Chat server, echo server, connection pool
  - **Exercise:**
    1. TCP connection pool implement করো
    2. Protocol parser (custom binary protocol)
    3. TCP proxy (forward connections from one port to another)

### Task 80 — `events` Module — EventEmitter 🟡

- Event-driven architecture foundation:

  ```js
  const EventEmitter = require('events');

  class OrderService extends EventEmitter {
    async createOrder(orderData) {
      const order = await db.orders.create(orderData);

      // Emit events instead of calling other services directly
      this.emit('order:created', order);
      return order;
    }
  }

  const orderService = new OrderService();

  // Loose coupling — services listen for events
  orderService.on('order:created', async (order) => {
    await emailService.sendConfirmation(order);
  });

  orderService.on('order:created', async (order) => {
    await inventoryService.reduceStock(order.items);
  });

  orderService.on('order:created', async (order) => {
    await analyticsService.trackOrder(order);
  });

  // Error handling
  orderService.on('error', (err) => {
    logger.error('Order service error', err);
  });
  ```

  - **Exercise:**
    1. EventEmitter from scratch build করো (JS Phase 13 থেকে recap)
    2. Event-driven application architecture design করো
    3. Memory leak: too many listeners (MaxListenersExceededWarning)

### Task 81 — `os` Module & System Info 🟢

- System information for monitoring:
  - **Exercise:** System info dashboard endpoint build করো

### Task 82 — `url` & `querystring` Module 🟢

- URL parsing:

  ```js
  const url = new URL('https://api.example.com:8080/users?page=2&sort=name#section1');
  url.protocol; // 'https:'
  url.hostname; // 'api.example.com'
  url.port; // '8080'
  url.pathname; // '/users'
  url.searchParams.get('page'); // '2'
  url.hash; // '#section1'

  // Build URL safely
  const apiUrl = new URL('/api/users', 'https://example.com');
  apiUrl.searchParams.set('page', 2);
  apiUrl.searchParams.set('q', 'hello world'); // Auto-encodes
  apiUrl.toString(); // https://example.com/api/users?page=2&q=hello+world
  ```

  - **Exercise:** URL router build করো (match patterns like `/users/:id`)

### Task 83 — `zlib` Module — Compression 🟢

- Gzip, Brotli, Deflate:

  ```js
  const zlib = require('zlib');
  const { pipeline } = require('stream/promises');
  const fs = require('fs');

  // Compress file
  await pipeline(
    fs.createReadStream('access.log'),
    zlib.createGzip(),
    fs.createWriteStream('access.log.gz')
  );

  // Decompress
  await pipeline(
    fs.createReadStream('access.log.gz'),
    zlib.createGunzip(),
    fs.createWriteStream('access.log.restored')
  );

  // Brotli (better compression)
  await pipeline(
    fs.createReadStream('data.json'),
    zlib.createBrotliCompress(),
    fs.createWriteStream('data.json.br')
  );
  ```

  - **Exercise:** Compression middleware build করো (auto gzip/brotli based on Accept-Encoding)

### Task 84 — `timers` & Performance Hooks 🟡

- Accurate timing:

  ```js
  const { performance, PerformanceObserver } = require('perf_hooks');

  // High-resolution timing
  const start = performance.now();
  await someOperation();
  console.log(`Took: ${(performance.now() - start).toFixed(2)}ms`);

  // Performance marks & measures
  performance.mark('start-db-query');
  await db.query('SELECT * FROM users');
  performance.mark('end-db-query');
  performance.measure('DB Query', 'start-db-query', 'end-db-query');

  // Observer
  const obs = new PerformanceObserver((list) => {
    list.getEntries().forEach((entry) => {
      console.log(`${entry.name}: ${entry.duration.toFixed(2)}ms`);
    });
  });
  obs.observe({ entryTypes: ['measure'] });
  ```

  - **Exercise:**
    1. Request timing middleware build করো
    2. Database query profiler build করো
    3. Performance dashboard: track slow operations

### Task 85 — `vm` Module — Code Execution Sandbox 🟡

- Run untrusted code safely:

  ```js
  const vm = require('vm');

  // Create sandbox
  const sandbox = {
    console: { log: (...args) => console.log('[Sandbox]', ...args) },
    result: null,
  };

  vm.createContext(sandbox);

  // Run code in sandbox
  const code = 'result = 2 + 2; console.log("Hello from sandbox!");';
  vm.runInContext(code, sandbox, { timeout: 1000 }); // 1 second timeout

  console.log(sandbox.result); // 4
  ```

  - **Warning:** `vm` is NOT truly secure. Use Docker/ShadowRealm for real sandboxing
  - **Exercise:** Code execution service (like repl.it backend)

### Task 86 — `dns` Module — DNS Resolution 🟢

- Programmatic DNS:

  ```js
  const dns = require('dns');
  const { Resolver } = dns;

  // Custom DNS resolver
  const resolver = new Resolver();
  resolver.setServers(['8.8.8.8', '1.1.1.1']); // Google, Cloudflare

  resolver.resolve4('example.com', (err, addresses) => {});
  resolver.resolveMx('gmail.com', (err, records) => {});
  resolver.resolveTxt('example.com', (err, records) => {}); // SPF, DKIM
  ```

  - **Exercise:** DNS health checker build করো (check if domain resolves)

### Task 87 — `readline` & `tty` — CLI Tools 🟢

- Interactive CLI:

  ```js
  const readline = require('readline');

  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  function question(prompt) {
    return new Promise((resolve) => rl.question(prompt, resolve));
  }

  async function main() {
    const name = await question('What is your name? ');
    const age = await question('How old are you? ');
    console.log(`Hello ${name}, you are ${age} years old!`);
    rl.close();
  }
  ```

  - **Exercise:** Interactive database migration CLI tool build করো

### Task 88 — `async_hooks` — Async Context Tracking 🔴

- Track async operations:

  ```js
  const async_hooks = require('async_hooks');
  const { AsyncLocalStorage } = require('async_hooks');

  // AsyncLocalStorage — request context tracking
  const requestContext = new AsyncLocalStorage();

  // Middleware: set context for each request
  app.use((req, res, next) => {
    const store = {
      requestId: crypto.randomUUID(),
      userId: req.user?.id,
      startTime: Date.now(),
    };
    requestContext.run(store, next);
  });

  // Anywhere in your code (no need to pass context!)
  function getRequestId() {
    return requestContext.getStore()?.requestId;
  }

  // Logger automatically includes request context
  function log(message) {
    const store = requestContext.getStore();
    console.log(
      JSON.stringify({
        timestamp: new Date().toISOString(),
        requestId: store?.requestId,
        userId: store?.userId,
        message,
      })
    );
  }
  ```

  - **Exercise:**
    1. Request context tracking implement করো (request ID in all logs)
    2. Database query tracing (which request triggered which query)
    3. Performance tracking per request

### Task 89 — Node.js Diagnostics & Debugging 🟡

- Debug production issues:

  ```bash
  # Inspector (Chrome DevTools)
  node --inspect src/index.js
  # Open chrome://inspect in Chrome

  # CPU Profile
  node --prof src/index.js
  node --prof-process isolate-*.log > profile.txt

  # Heap Snapshot
  node --heapsnapshot-signal=SIGUSR2 src/index.js
  kill -USR2 <PID>  # Generate heap snapshot

  # Trace warnings
  node --trace-warnings src/index.js

  # Trace deprecations
  node --trace-deprecation src/index.js
  ```

  - **Programmatic:**

    ```js
    const v8 = require('v8');

    // Heap statistics
    console.log(v8.getHeapStatistics());

    // Heap snapshot
    const stream = v8.writeHeapSnapshot();
    console.log(`Heap snapshot written to: ${stream}`);

    // Memory usage over time
    setInterval(() => {
      const usage = process.memoryUsage();
      console.log({
        rss: `${(usage.rss / 1024 / 1024).toFixed(1)}MB`,
        heapUsed: `${(usage.heapUsed / 1024 / 1024).toFixed(1)}MB`,
        external: `${(usage.external / 1024 / 1024).toFixed(1)}MB`,
      });
    }, 5000);
    ```

  - **Exercise:**
    1. Memory leak detect করো (heap snapshot comparison)
    2. CPU bottleneck find করো (profiling)
    3. Slow async operation identify করো

### Task 90 — Native Addons (C++ ↔ Node.js) 🔴

- When JS isn't fast enough:

  ```js
  // N-API (Node-API) — stable ABI across Node versions
  // binding.gyp:
  {
    "targets": [{
      "target_name": "addon",
      "sources": ["addon.cc"]
    }]
  }
  ```

  ```cpp
  // addon.cc
  #include <napi.h>

  Napi::Number Add(const Napi::CallbackInfo& info) {
    double a = info[0].As<Napi::Number>().DoubleValue();
    double b = info[1].As<Napi::Number>().DoubleValue();
    return Napi::Number::New(info.Env(), a + b);
  }

  Napi::Object Init(Napi::Env env, Napi::Object exports) {
    exports.Set("add", Napi::Function::New(env, Add));
    return exports;
  }

  NODE_API_MODULE(addon, Init)
  ```

  ```js
  // Use in JS
  const addon = require('./build/Release/addon');
  console.log(addon.add(3, 4)); // 7
  ```

  - **Exercise:**
    1. Simple native addon build করো (add, multiply)
    2. Performance comparison: JS vs C++ addon
    3. Understand: when to use native addons (image processing, crypto)

---

## Section C: Advanced Node.js Patterns (Tasks 91-105)

### Task 91 — Dependency Injection in Node.js 🟡

- Loose coupling for testability:

  ```js
  // WITHOUT DI (tightly coupled)
  const db = require('./database');
  class UserService {
    async getUser(id) {
      return db.query('SELECT * FROM users WHERE id = $1', [id]);
    }
  }

  // WITH DI (testable, flexible)
  class UserService {
    #db;
    constructor(db) {
      this.#db = db;
    }
    async getUser(id) {
      return this.#db.query('SELECT * FROM users WHERE id = $1', [id]);
    }
  }

  // Production
  const userService = new UserService(realDatabase);

  // Testing
  const mockDb = { query: jest.fn().mockResolvedValue({ id: 1, name: 'Test' }) };
  const userService = new UserService(mockDb);
  ```

  - **DI Container:**

    ```js
    class Container {
      #services = new Map();
      #singletons = new Map();

      register(name, factory, singleton = false) {
        this.#services.set(name, { factory, singleton });
      }

      resolve(name) {
        const service = this.#services.get(name);
        if (!service) throw new Error(`Service ${name} not registered`);

        if (service.singleton) {
          if (!this.#singletons.has(name)) {
            this.#singletons.set(name, service.factory(this));
          }
          return this.#singletons.get(name);
        }

        return service.factory(this);
      }
    }

    const container = new Container();
    container.register('db', () => new Database(config), true);
    container.register('userService', (c) => new UserService(c.resolve('db')));
    container.register(
      'orderService',
      (c) => new OrderService(c.resolve('db'), c.resolve('userService'))
    );
    ```

  - **Exercise:** Full DI container build করো ও entire application wire করো

### Task 92 — Repository Pattern 🟡

- Database abstraction:

  ```js
  // Base Repository
  class BaseRepository {
    #db;
    #table;
    constructor(db, table) {
      this.#db = db;
      this.#table = table;
    }

    async findById(id) {
      const result = await this.#db.query(`SELECT * FROM ${this.#table} WHERE id = $1`, [id]);
      return result.rows[0];
    }

    async findAll({ page = 1, limit = 20, where = {}, orderBy = 'id' } = {}) {
      const offset = (page - 1) * limit;
      const conditions = Object.entries(where)
        .map(([key], i) => `${key} = $${i + 1}`)
        .join(' AND ');

      const query = `SELECT * FROM ${this.#table}
        ${conditions ? `WHERE ${conditions}` : ''}
        ORDER BY ${orderBy} LIMIT $${Object.keys(where).length + 1}
        OFFSET $${Object.keys(where).length + 2}`;

      return this.#db.query(query, [...Object.values(where), limit, offset]);
    }

    async create(data) {
      const keys = Object.keys(data);
      const values = Object.values(data);
      const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');
      const result = await this.#db.query(
        `INSERT INTO ${this.#table} (${keys.join(', ')}) VALUES (${placeholders}) RETURNING *`,
        values
      );
      return result.rows[0];
    }

    async update(id, data) {
      /* ... */
    }
    async delete(id) {
      /* ... */
    }
  }

  class UserRepository extends BaseRepository {
    constructor(db) {
      super(db, 'users');
    }
    async findByEmail(email) {
      /* ... */
    }
    async findActive() {
      /* ... */
    }
  }
  ```

  - **Exercise:** Complete repository pattern with pagination, filtering, sorting

### Task 93 — Service Layer Pattern 🟡

- Business logic separation:

  ```js
  class UserService {
    #userRepo;
    #emailService;
    #logger;

    constructor(userRepo, emailService, logger) {
      this.#userRepo = userRepo;
      this.#emailService = emailService;
      this.#logger = logger;
    }

    async register(userData) {
      // 1. Validate
      const existing = await this.#userRepo.findByEmail(userData.email);
      if (existing) throw new ConflictError('Email already exists');

      // 2. Business logic
      const hashedPassword = await hashPassword(userData.password);

      // 3. Create user
      const user = await this.#userRepo.create({
        ...userData,
        password: hashedPassword,
        role: 'user',
        isVerified: false,
      });

      // 4. Side effects
      await this.#emailService.sendVerification(user);
      this.#logger.info('User registered', { userId: user.id });

      // 5. Return (without password)
      const { password, ...publicUser } = user;
      return publicUser;
    }
  }
  ```

  - **Exercise:** Full service layer (UserService, OrderService, PaymentService)

### Task 94 — Middleware Pattern Deep Dive 🟡

- How middleware chains work:

  ```js
  // Build your own middleware system
  class MiddlewareChain {
    #middlewares = [];

    use(fn) {
      this.#middlewares.push(fn);
      return this;
    }

    async execute(context) {
      let index = 0;
      const next = async () => {
        if (index >= this.#middlewares.length) return;
        const middleware = this.#middlewares[index++];
        await middleware(context, next);
      };
      await next();
    }
  }

  const chain = new MiddlewareChain();
  chain.use(async (ctx, next) => {
    ctx.startTime = Date.now();
    await next();
    console.log(`Duration: ${Date.now() - ctx.startTime}ms`);
  });
  chain.use(async (ctx, next) => {
    console.log('Before handler');
    await next();
    console.log('After handler');
  });
  chain.use(async (ctx) => {
    ctx.result = 'Hello World';
  });
  ```

  - **Exercise:** Middleware engine build করো (like Express/Koa middleware)

### Task 95 — Connection Pooling 🟡

- Reuse database connections:

  ```js
  class ConnectionPool {
    #connections = [];
    #available = [];
    #waiting = [];
    #config;
    #min;
    #max;

    constructor(config, { min = 2, max = 10 } = {}) {
      this.#config = config;
      this.#min = min;
      this.#max = max;
    }

    async initialize() {
      for (let i = 0; i < this.#min; i++) {
        const conn = await this.#createConnection();
        this.#available.push(conn);
      }
    }

    async acquire() {
      if (this.#available.length > 0) {
        return this.#available.pop();
      }

      if (this.#connections.length < this.#max) {
        return this.#createConnection();
      }

      // Wait for a connection to become available
      return new Promise((resolve) => {
        this.#waiting.push(resolve);
      });
    }

    release(conn) {
      if (this.#waiting.length > 0) {
        const resolve = this.#waiting.shift();
        resolve(conn);
      } else {
        this.#available.push(conn);
      }
    }

    async #createConnection() {
      const conn = await connect(this.#config);
      this.#connections.push(conn);
      return conn;
    }
  }
  ```

  - **Exercise:** Database connection pool build ও test করো

### Task 96 — Rate Limiting Advanced 🟡

- Token Bucket ও Sliding Window:

  ```js
  class TokenBucket {
    #tokens;
    #maxTokens;
    #refillRate;
    #lastRefill;

    constructor(maxTokens, refillRate) {
      this.#maxTokens = maxTokens;
      this.#tokens = maxTokens;
      this.#refillRate = refillRate; // tokens per second
      this.#lastRefill = Date.now();
    }

    consume(tokens = 1) {
      this.#refill();
      if (this.#tokens >= tokens) {
        this.#tokens -= tokens;
        return true;
      }
      return false;
    }

    #refill() {
      const now = Date.now();
      const elapsed = (now - this.#lastRefill) / 1000;
      this.#tokens = Math.min(this.#maxTokens, this.#tokens + elapsed * this.#refillRate);
      this.#lastRefill = now;
    }
  }
  ```

  - **Exercise:** Per-user, per-endpoint rate limiting with Redis backend

### Task 97 — Job Queue & Background Processing 🔴

- Process heavy tasks in background:

  ```js
  class JobQueue {
    #jobs = [];
    #processing = false;
    #concurrency;
    #activeJobs = 0;

    constructor(concurrency = 5) {
      this.#concurrency = concurrency;
    }

    add(job, options = {}) {
      this.#jobs.push({
        fn: job,
        priority: options.priority || 0,
        retries: options.retries || 3,
        attempts: 0,
        delay: options.delay || 0,
      });
      this.#jobs.sort((a, b) => b.priority - a.priority);
      this.#process();
    }

    async #process() {
      while (this.#jobs.length > 0 && this.#activeJobs < this.#concurrency) {
        const job = this.#jobs.shift();
        this.#activeJobs++;

        if (job.delay > 0) {
          setTimeout(() => this.#execute(job), job.delay);
        } else {
          this.#execute(job);
        }
      }
    }

    async #execute(job) {
      try {
        job.attempts++;
        await job.fn();
      } catch (err) {
        if (job.attempts < job.retries) {
          job.delay = Math.pow(2, job.attempts) * 1000; // Exponential backoff
          this.#jobs.push(job);
        } else {
          console.error('Job failed after retries:', err);
        }
      } finally {
        this.#activeJobs--;
        this.#process();
      }
    }
  }

  const queue = new JobQueue(3);
  queue.add(
    async () => {
      await sendEmail(user.email, 'Welcome!');
    },
    { retries: 3, priority: 1 }
  );
  ```

  - **Exercise:**
    1. Full job queue build করো (priority, retry, delay, concurrency)
    2. Email sending queue
    3. Image processing queue

### Task 98 — Graceful Shutdown Pattern 🟡

- Clean server shutdown:
  - Detailed in Task 40 — implement for Express app with:
    - Stop accepting new connections
    - Wait for in-flight requests to complete
    - Close database connections
    - Flush logs
    - Clear timers/intervals
    - Exit cleanly

### Task 99 — Configuration Management 🟢

- Environment-based config:

  ```js
  // config/index.js
  const configs = {
    development: {
      port: 3000,
      db: { host: 'localhost', port: 5432, name: 'myapp_dev' },
      redis: { host: 'localhost', port: 6379 },
      log: { level: 'debug' },
      cors: { origin: '*' },
    },
    production: {
      port: process.env.PORT,
      db: {
        host: process.env.DB_HOST,
        port: process.env.DB_PORT,
        name: process.env.DB_NAME,
        password: process.env.DB_PASSWORD,
        ssl: true,
        pool: { min: 5, max: 20 },
      },
      redis: { host: process.env.REDIS_HOST, port: process.env.REDIS_PORT },
      log: { level: 'info' },
      cors: { origin: process.env.ALLOWED_ORIGINS?.split(',') },
    },
  };

  const env = process.env.NODE_ENV || 'development';
  module.exports = configs[env];
  ```

  - **Exercise:** Config system with validation, defaults, ও env-specific overrides

### Task 100 — TypeScript with Node.js 🟡

- Type safety for backend:

  ```typescript
  interface User {
    id: number;
    name: string;
    email: string;
    role: 'admin' | 'user' | 'moderator';
    createdAt: Date;
  }

  interface CreateUserDTO {
    name: string;
    email: string;
    password: string;
  }

  class UserService {
    private db: Database;

    constructor(db: Database) {
      this.db = db;
    }

    async getUser(id: number): Promise<User | null> {
      return this.db.query<User>('SELECT * FROM users WHERE id = $1', [id]);
    }

    async createUser(data: CreateUserDTO): Promise<User> {
      // TypeScript ensures data has required fields
      const { name, email, password } = data;
      // ...
    }
  }
  ```

  - **Exercise:**
    1. TypeScript Node.js project setup (tsconfig, build, dev)
    2. Express app with full type safety
    3. Database queries with typed results

### Task 101 — WebSocket Server Production 🟡

- Production-ready WebSocket:

  ```js
  const WebSocket = require('ws');
  const http = require('http');

  const server = http.createServer(app);
  const wss = new WebSocket.Server({ server });

  // Connection management
  const clients = new Map(); // userId → ws

  wss.on('connection', (ws, req) => {
    const userId = authenticateWS(req); // Verify token from URL/header
    clients.set(userId, ws);

    ws.isAlive = true;
    ws.on('pong', () => {
      ws.isAlive = true;
    });

    ws.on('message', (data) => {
      try {
        const message = JSON.parse(data);
        handleMessage(userId, message);
      } catch (err) {
        ws.send(JSON.stringify({ error: 'Invalid message format' }));
      }
    });

    ws.on('close', () => clients.delete(userId));
  });

  // Heartbeat (detect dead connections)
  setInterval(() => {
    wss.clients.forEach((ws) => {
      if (!ws.isAlive) return ws.terminate();
      ws.isAlive = false;
      ws.ping();
    });
  }, 30000);

  // Send to specific user
  function sendToUser(userId, data) {
    const ws = clients.get(userId);
    if (ws?.readyState === WebSocket.OPEN) {
      ws.send(JSON.stringify(data));
    }
  }

  // Broadcast to all
  function broadcast(data) {
    wss.clients.forEach((ws) => {
      if (ws.readyState === WebSocket.OPEN) {
        ws.send(JSON.stringify(data));
      }
    });
  }
  ```

  - **Exercise:** Real-time notification system (user-specific + broadcast)

### Task 102 — Server-Sent Events Production 🟢

- SSE with connection management:
  - **Exercise:** Live dashboard using SSE (server metrics, user activity)

### Task 103 — Node.js Memory Leaks 🔴

- Find ও fix memory leaks:

  ```js
  // Common leak patterns:

  // 1. Global arrays that grow forever
  const cache = []; // Never cleaned!
  app.get('/api', (req, res) => {
    cache.push(req.body); // Memory grows forever! 💀
  });

  // 2. Event listeners not cleaned up
  function handleConnection(socket) {
    emitter.on('data', (data) => socket.write(data));
    // When socket closes, listener still exists!
    // Fix: remove listener on close
    socket.on('close', () => emitter.removeAllListeners('data'));
  }

  // 3. Closures holding references
  function createHandler() {
    const largeData = Buffer.alloc(10 * 1024 * 1024); // 10MB
    return () => {
      // This closure keeps largeData alive forever!
      console.log(largeData.length);
    };
  }

  // 4. Timers not cleared
  const timers = [];
  app.post('/schedule', (req, res) => {
    timers.push(
      setInterval(() => {
        /* work */
      }, 1000)
    );
    // Timers never cleared!
  });
  ```

  - **Detection:**
    ```bash
    # Generate heap snapshot
    node --inspect app.js
    # Chrome DevTools → Memory → Take heap snapshot → Compare snapshots
    ```
  - **Exercise:**
    1. Memory leak introduce করো ও detect করো (heap snapshot)
    2. Fix all 4 leak patterns above
    3. Memory monitoring middleware build করো

### Task 104 — Performance Optimization 🔴

- Make Node.js fly:

  ```js
  // 1. Use streams (don't buffer large data)
  // ❌ Bad
  const data = await readFile('huge.csv');
  // ✅ Good
  createReadStream('huge.csv').pipe(transform).pipe(res);

  // 2. Use proper data structures
  // ❌ Array.find for lookups = O(n)
  const user = users.find((u) => u.id === id);
  // ✅ Map for lookups = O(1)
  const userMap = new Map(users.map((u) => [u.id, u]));
  const user = userMap.get(id);

  // 3. Avoid blocking the event loop
  // ❌ Sync operation
  const hash = crypto.pbkdf2Sync(password, salt, 100000, 64, 'sha512');
  // ✅ Async operation
  const hash = await util.promisify(crypto.pbkdf2)(password, salt, 100000, 64, 'sha512');

  // 4. Connection pooling (reuse connections)
  // 5. Caching (avoid repeated computation/queries)
  // 6. Compress responses (gzip/brotli)
  // 7. Pagination (don't return all records)
  ```

  - **Exercise:**
    1. Profile application (CPU, memory, event loop lag)
    2. Identify top 3 bottlenecks ও fix করো
    3. Before/after benchmark comparison

### Task 105 — Build: Production-Ready Node.js Boilerplate ⚫

- **Phase 03 Final Project:**
  - Complete project structure:
    ```
    src/
    ├── config/          — Configuration management
    ├── controllers/     — HTTP request handlers
    ├── services/        — Business logic
    ├── repositories/    — Database access
    ├── middleware/       — Express middleware
    │   ├── auth.js
    │   ├── validate.js
    │   ├── rateLimiter.js
    │   ├── errorHandler.js
    │   └── requestLogger.js
    ├── models/          — Data models
    ├── routes/          — Route definitions
    ├── utils/           — Helper functions
    │   ├── logger.js
    │   ├── errors.js
    │   └── crypto.js
    ├── jobs/            — Background jobs
    ├── events/          — Event handlers
    └── index.js         — Entry point
    ```
  - **Features:**
    - Structured logging (JSON)
    - Error handling (custom errors, global handler)
    - Request validation
    - Rate limiting
    - CORS
    - Compression
    - Graceful shutdown
    - Health check endpoint
    - Configuration management
    - Dependency injection
    - Security headers
  - **This boilerplate will be used for ALL remaining phases**

---

## ✅ Phase 03 Completion Checklist

- [ ] সব 40 tasks complete
- [ ] Event Loop fully understand করো (timing, phases, microtasks)
- [ ] Streams master করেছো (read, write, transform, pipeline, backpressure)
- [ ] Cluster mode ও Worker Threads use করতে পারো
- [ ] Memory leaks detect ও fix করতে পারো
- [ ] Performance profiling করতে পারো
- [ ] Production-ready boilerplate তৈরি আছে

> _"Node.js দিয়ে code লেখে সবাই — Node.js বোঝে 1%। তুমি সেই 1%।"_
> _"Event Loop diagram দেখে explain করতে পারো? তাহলে তুমি interview crack করবে।"_
