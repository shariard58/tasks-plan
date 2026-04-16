# JS Phase 07 — Node.js & Server-Side JavaScript (Tasks 186-220)

> Node.js হলো JS এর server-side runtime। Same language, different environment। V8 engine + libuv = non-blocking I/O।

---

## Section A: Node.js Core Concepts (Tasks 186-195)

### Task 186 — Node.js Architecture 🟡

- How Node.js works:
  - V8 Engine: compiles JS to machine code
  - libuv: event loop, async I/O, thread pool
  - Node.js event loop phases: timers → pending callbacks → idle → poll → check → close
  - Single-threaded but non-blocking
  - Thread pool for: file I/O, DNS, crypto, zlib
  - **Diagram:** Draw the complete Node.js architecture
  - **Build:** Script that demonstrates each event loop phase

### Task 187 — Module System (CJS vs ESM) 🟡

- CommonJS (CJS):

  ```js
  // Export
  module.exports = { add, subtract };
  exports.multiply = (a, b) => a * b;

  // Import
  const { add } = require('./math');
  ```

- ES Modules (ESM):

  ```js
  // Export
  export const add = (a, b) => a + b;
  export default function subtract(a, b) {
    return a - b;
  }

  // Import
  import subtract, { add } from './math.js';
  ```

  - `require()` — synchronous, dynamic
  - `import` — async, static (can tree-shake)
  - Module caching: `require.cache`
  - `package.json` `"type": "module"` for ESM
  - Interop: `import()` dynamic import in both
  - **Build:** Package that supports both CJS ও ESM

### Task 188 — File System (fs) Module 🟡

- File operations:
  - Sync: `fs.readFileSync()`, `fs.writeFileSync()` (blocks event loop!)
  - Async callback: `fs.readFile()`, `fs.writeFile()`
  - Promise: `fs.promises.readFile()`, `fs.promises.writeFile()`
  - All operations: read, write, append, copy, rename, delete, mkdir, rmdir, stat, watch
  - `fs.createReadStream()` / `fs.createWriteStream()` — streaming
  - **Build:**
    1. File manager CLI (ls, cat, cp, mv, rm, mkdir)
    2. Log file analyzer (read large log, parse, filter, stats)
    3. File watcher (watch directory for changes)

### Task 189 — Path Module 🟢

- Path utilities:

  ```js
  const path = require('path');

  path.join('/user', 'docs', 'file.txt'); // '/user/docs/file.txt'
  path.resolve('src', 'utils'); // absolute path
  path.dirname('/user/docs/file.txt'); // '/user/docs'
  path.basename('/user/docs/file.txt'); // 'file.txt'
  path.basename('/user/docs/file.txt', '.txt'); // 'file'
  path.extname('file.txt'); // '.txt'
  path.parse('/user/docs/file.txt'); // { root, dir, base, ext, name }
  path.format(pathObject); // back to string
  path.isAbsolute('/user'); // true
  path.relative('/user/docs', '/user/photos'); // '../photos'
  ```

  - Cross-platform: `path.sep`, `path.delimiter`
  - **Always use `path.join`** — never concatenate with `/`

### Task 190 — Streams Deep Dive 🔴

- Node.js streams:
  - **Readable:** data source (file read, HTTP request body)
  - **Writable:** data destination (file write, HTTP response)
  - **Duplex:** both read ও write (TCP socket)
  - **Transform:** modify data passing through (gzip, encryption)
  - Stream modes: flowing (push) vs paused (pull)
  - Events: `data`, `end`, `error`, `close`, `drain`, `pipe`
  - Pipe: `readable.pipe(transform).pipe(writable)`
  - `pipeline()` (with error handling):
    ```js
    const { pipeline } = require('stream/promises');
    await pipeline(
      fs.createReadStream('input.txt'),
      zlib.createGzip(),
      fs.createWriteStream('output.txt.gz')
    );
    ```
  - **Build:**
    1. File copy using streams (handle large files)
    2. CSV parser transform stream
    3. Encryption transform stream
    4. Progress indicator for file operations

### Task 191 — Buffer & Binary Data 🟡

- Buffer:

  ```js
  const buf = Buffer.from('Hello', 'utf8');
  buf.toString('hex'); // '48656c6c6f'
  buf.toString('base64'); // 'SGVsbG8='

  Buffer.alloc(10); // zero-filled
  Buffer.allocUnsafe(10); // faster, uninitialized

  Buffer.concat([buf1, buf2]);
  buf.slice(0, 3);
  buf.copy(target);
  ```

  - Buffer vs ArrayBuffer vs TypedArray
  - Encoding: utf8, ascii, base64, hex, binary
  - **Build:** Binary protocol parser (parse custom packet format)

### Task 192 — Events Module (EventEmitter) 🟡

- Node.js event system:

  ```js
  const EventEmitter = require('events');

  class Server extends EventEmitter {
    start() {
      // ... setup
      this.emit('ready', { port: 3000 });
    }
  }

  const server = new Server();
  server.on('ready', (info) => console.log(`Ready on port ${info.port}`));
  server.once('error', (err) => console.error(err));
  server.start();
  ```

  - Methods: `on`, `once`, `off`, `emit`, `removeAllListeners`, `listenerCount`, `eventNames`
  - `setMaxListeners(n)` — prevent memory leak warnings
  - Error event: MUST have handler or process crashes
  - **Build:** Custom event-driven logger (emits events for each log level)

### Task 193 — HTTP Module (No Framework) 🟡

- Raw HTTP server:

  ```js
  const http = require('http');

  const server = http.createServer((req, res) => {
    const { method, url, headers } = req;

    if (method === 'GET' && url === '/api/users') {
      res.writeHead(200, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify([{ id: 1, name: 'John' }]));
    } else if (method === 'POST' && url === '/api/users') {
      let body = '';
      req.on('data', (chunk) => (body += chunk));
      req.on('end', () => {
        const user = JSON.parse(body);
        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(user));
      });
    } else {
      res.writeHead(404);
      res.end('Not Found');
    }
  });

  server.listen(3000);
  ```

  - **Build:** Complete REST API (no Express):
    - CRUD for users
    - JSON body parsing
    - URL routing
    - Query parameter parsing
    - Error handling
    - CORS headers
    - Static file serving

### Task 194 — Child Processes 🟡

- Run external commands:

  ```js
  const { exec, execFile, spawn, fork } = require('child_process');

  // exec — shell command, buffers output
  exec('ls -la', (err, stdout, stderr) => { ... });

  // spawn — stream output (large data)
  const ls = spawn('ls', ['-la']);
  ls.stdout.on('data', (data) => console.log(data.toString()));

  // fork — Node.js child process with IPC
  const child = fork('worker.js');
  child.send({ type: 'PROCESS', data: largeArray });
  child.on('message', (result) => console.log(result));
  ```

  - `exec` vs `execFile` vs `spawn` vs `fork` — when to use which
  - **Build:** Task runner that spawns child processes for parallel work

### Task 195 — Worker Threads 🔴

- True multi-threading in Node.js:

  ```js
  const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

  if (isMainThread) {
    const worker = new Worker(__filename, { workerData: { num: 42 } });
    worker.on('message', (result) => console.log(result));
    worker.on('error', (err) => console.error(err));
  } else {
    const result = heavyComputation(workerData.num);
    parentPort.postMessage(result);
  }
  ```

  - SharedArrayBuffer between threads
  - Worker pool for CPU-intensive tasks
  - **Build:** Worker pool:
    ```js
    const pool = new WorkerPool('worker.js', { size: 4 });
    const result = await pool.exec({ type: 'HASH', data: password });
    ```

---

## Section B: Building with Node.js (Tasks 196-210)

### Task 196 — Process & Environment 🟡

- Process object:
  - `process.env` — environment variables
  - `process.argv` — command line arguments
  - `process.cwd()` — current working directory
  - `process.exit(code)` — exit (0 = success, 1 = error)
  - `process.pid`, `process.platform`, `process.version`
  - `process.memoryUsage()` — heap, RSS, external
  - `process.cpuUsage()`
  - Signal handling: `process.on('SIGINT', handler)`, `process.on('SIGTERM', handler)`
  - **Build:** .env file parser (like dotenv but simple)

### Task 197 — npm & Package Management 🟡

- npm deep dive:
  - `package.json`: dependencies, devDependencies, peerDependencies, scripts, main, module, exports, type, files, bin
  - Semantic versioning: `^1.2.3` (minor), `~1.2.3` (patch), `1.2.3` (exact)
  - `package-lock.json` — why it matters (deterministic installs)
  - `npx` — run packages without installing
  - Creating packages:
    - `npm init`
    - `npm publish`
    - Scoped packages: `@myorg/package`
  - **Build:** Create ও publish an npm package:
    1. A utility library
    2. Proper package.json (main, module, exports, types)
    3. README with docs
    4. CI/CD for automated publishing

### Task 198 — CLI Tool Development 🟡

- Command line tools:
  ```js
  #!/usr/bin/env node
  const args = process.argv.slice(2);
  ```

  - Argument parsing (manually ও with `commander` or `yargs`)
  - Interactive prompts
  - Colors ও formatting (ANSI codes)
  - Progress bars
  - Spinners
  - **Build:** CLI tools:
    1. `todo-cli` — add, list, complete, delete todos
    2. `file-organizer` — organize files by type/date
    3. `project-scaffolder` — create project templates

### Task 199 — Testing in Node.js 🟡

- Built-in test runner (Node 18+):

  ```js
  const { describe, it } = require('node:test');
  const assert = require('node:assert');

  describe('Calculator', () => {
    it('should add numbers', () => {
      assert.strictEqual(add(2, 3), 5);
    });
  });
  ```

  - `node:assert` — assertion library
  - `node:test` — test runner
  - Mocking: `mock.method()`, `mock.fn()`
  - **Build:** Test suite for all previous Node.js builds

### Task 200 — Error Handling in Node.js 🟡

- Node-specific error handling:
  - Synchronous: `try/catch`
  - Async callback: error-first `callback(err, data)`
  - Promises: `.catch()` ও `try/catch` with `await`
  - Streams: `.on('error', handler)`
  - Process level:

    ```js
    process.on('uncaughtException', (err) => {
      console.error('Uncaught:', err);
      process.exit(1); // Must exit!
    });

    process.on('unhandledRejection', (reason) => {
      console.error('Unhandled rejection:', reason);
    });
    ```

  - **Build:** Global error handler with logging ও graceful shutdown

### Task 201 — Build: Static File Server 🟡

- HTTP server for files:
  - Serve files from a directory
  - MIME type detection
  - Directory listing
  - 404 for missing files
  - Range requests (partial content — video streaming)
  - Gzip compression
  - Cache headers (ETag, Last-Modified)
  - Security: prevent path traversal (`../../../etc/passwd`)
  - **Output:** Production-quality static file server

### Task 202 — Build: REST API Framework (Mini Express) 🔴

- Build your own Express.js:

  ```js
  const app = createApp();

  app.use(logger);
  app.use(jsonParser);

  app.get('/api/users', (req, res) => {
    res.json(users);
  });

  app.post('/api/users', (req, res) => {
    const user = req.body;
    users.push(user);
    res.status(201).json(user);
  });

  app.get('/api/users/:id', (req, res) => {
    res.json(users.find((u) => u.id === req.params.id));
  });

  app.listen(3000);
  ```

  - Features:
    - Route matching (GET, POST, PUT, DELETE)
    - Path parameters (`:id`)
    - Query parameters
    - Middleware system (use, per-route)
    - JSON body parser
    - Static file serving
    - Error middleware
    - Response helpers (json, send, status, redirect)
  - **Output:** Working framework (~300 lines)

### Task 203 — Build: Template Engine 🟡

- Server-side HTML rendering:

  ```js
  const template = `
    <h1>{{ title }}</h1>
    {% for user in users %}
      <div class="user">
        <p>{{ user.name }}</p>
        {% if user.isAdmin %}
          <span class="badge">Admin</span>
        {% endif %}
      </div>
    {% endfor %}
  `;

  const html = render(template, { title: 'Users', users: [...] });
  ```

  - Features: variable interpolation, loops, conditionals, includes, escaping
  - **Output:** Template engine with caching

### Task 204 — Database Interaction (No ORM) 🟡

- Raw database access:
  - SQLite with `better-sqlite3`:
    ```js
    const db = new Database('app.db');
    db.exec('CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT)');
    db.prepare('INSERT INTO users (name, email) VALUES (?, ?)').run('John', 'john@email.com');
    const users = db.prepare('SELECT * FROM users WHERE name = ?').all('John');
    ```
  - Parameterized queries (prevent SQL injection!)
  - Transactions
  - Migrations
  - **Build:** Complete database layer:
    - CRUD operations
    - Query builder (programmatic SQL)
    - Migration system
    - Seeding

### Task 205 — Authentication & Security 🔴

- Auth from scratch:
  - Password hashing: `crypto.scrypt()` or `bcrypt`
  - JWT (JSON Web Tokens):

    ```js
    const crypto = require('crypto');

    function createJWT(payload, secret) {
      const header = base64url({ alg: 'HS256', typ: 'JWT' });
      const body = base64url(payload);
      const signature = crypto
        .createHmac('sha256', secret)
        .update(`${header}.${body}`)
        .digest('base64url');
      return `${header}.${body}.${signature}`;
    }
    ```

  - Session-based auth
  - CSRF protection
  - Rate limiting
  - Input sanitization
  - **Build:** Auth system:
    - Register (hash password, store)
    - Login (verify password, issue JWT)
    - Protected routes (verify JWT middleware)
    - Refresh tokens
    - Logout (token blacklist)

### Task 206 — WebSocket Server 🟡

- Real-time server:

  ```js
  const { WebSocketServer } = require('ws');

  const wss = new WebSocketServer({ port: 8080 });

  wss.on('connection', (ws) => {
    ws.on('message', (data) => {
      // Broadcast to all clients
      wss.clients.forEach((client) => {
        if (client !== ws && client.readyState === 1) {
          client.send(data.toString());
        }
      });
    });
  });
  ```

  - **Build:** Real-time chat server:
    - Rooms/channels
    - Private messages
    - Online user list
    - Message history
    - Typing indicator

### Task 207 — Cron Jobs & Scheduling 🟡

- Scheduled tasks:
  - `setInterval` — simple recurring
  - Cron expression: `'0 */5 * * *'` (every 5 hours)
  - **Build:** Task scheduler:
    ```js
    const scheduler = new Scheduler();
    scheduler.every('5 minutes', cleanupTempFiles);
    scheduler.daily('09:00', sendDailyReport);
    scheduler.cron('0 0 * * MON', weeklyBackup);
    scheduler.start();
    ```
  - **Build:** Automated backup system

### Task 208 — Logging System 🟡

- Production logging:
  - Log levels: error, warn, info, debug, trace
  - **Build:** Logger:

    ```js
    const logger = createLogger({
      level: 'info',
      transports: [
        new ConsoleTransport({ colorize: true }),
        new FileTransport({ filename: 'app.log', maxSize: '10mb', maxFiles: 5 }),
      ],
      format: (level, message, meta) => {
        return `[${new Date().toISOString()}] [${level}] ${message} ${JSON.stringify(meta)}`;
      },
    });

    logger.info('User logged in', { userId: 123 });
    logger.error('Database connection failed', { error });
    ```

  - Features: levels, transports, formatting, rotation, structured logging

### Task 209 — Build: Full REST API Project 🔴

- Complete backend (using your mini-Express from Task 202):
  - **Blog API:**
    - Users: register, login, profile
    - Posts: CRUD, pagination, search
    - Comments: CRUD, nested
    - Tags: CRUD, filter posts by tag
    - Auth: JWT, protected routes
    - Database: SQLite
    - Validation: input sanitization
    - Error handling: global error handler
    - Logging: structured logs
    - **Output:** Production-quality REST API

### Task 210 — Build: Real-Time Dashboard Backend ⚫

- WebSocket + REST API:
  - REST endpoints for data CRUD
  - WebSocket for live updates
  - Server-Sent Events for notifications
  - Background jobs for data processing
  - Database for persistence
  - Auth for all endpoints
  - Rate limiting
  - **Output:** Backend for a real-time analytics dashboard

---

## Section C: Node.js Advanced (Tasks 211-220)

### Task 211 — Cluster Module 🟡

- Multi-process Node.js:

  ```js
  const cluster = require('cluster');
  const os = require('os');

  if (cluster.isPrimary) {
    const cpuCount = os.cpus().length;
    for (let i = 0; i < cpuCount; i++) cluster.fork();
    cluster.on('exit', (worker) => cluster.fork()); // restart on crash
  } else {
    // Each worker runs the server
    http.createServer(handler).listen(3000);
  }
  ```

  - Sticky sessions for WebSocket
  - **Build:** Clustered HTTP server with load testing

### Task 212 — Debugging & Profiling Node.js 🟡

- Debug tools:
  - `node --inspect` + Chrome DevTools
  - `console.time()` / `console.timeEnd()`
  - `process.memoryUsage()`
  - Heap snapshot analysis
  - CPU profiling
  - Memory leak detection
  - **Build:** Find ও fix 5 intentionally leaky Node.js programs

### Task 213 — Environment Configuration 🟢

- Config management:
  - Environment variables: `process.env.NODE_ENV`
  - `.env` files (dotenv pattern)
  - Config per environment (dev, staging, production)
  - Secrets management (never commit secrets!)
  - **Build:** Config system:
    ```js
    const config = loadConfig({
      port: { env: 'PORT', default: 3000, type: 'number' },
      dbUrl: { env: 'DATABASE_URL', required: true },
      debug: { env: 'DEBUG', default: false, type: 'boolean' },
    });
    ```

### Task 214 — Graceful Shutdown 🟡

- Clean server stop:

  ```js
  const server = http.createServer(handler).listen(3000);

  async function shutdown(signal) {
    console.log(`${signal} received. Shutting down gracefully...`);
    server.close(() => {
      // Close database connections
      // Finish pending requests
      // Flush logs
      process.exit(0);
    });

    // Force exit after timeout
    setTimeout(() => process.exit(1), 10000);
  }

  process.on('SIGTERM', () => shutdown('SIGTERM'));
  process.on('SIGINT', () => shutdown('SIGINT'));
  ```

  - **Build:** Server with complete graceful shutdown

### Task 215 — Node.js Security Best Practices 🔴

- Security checklist:
  - Input validation ও sanitization
  - SQL injection prevention (parameterized queries)
  - XSS prevention (escape output)
  - CSRF tokens
  - Rate limiting
  - Helmet-like headers (security headers)
  - CORS configuration
  - Path traversal prevention
  - Dependency vulnerability scanning (`npm audit`)
  - Secret management
  - **Build:** Security middleware package (like Helmet)

### Task 216 — Build: Proxy Server 🟡

- HTTP proxy:
  ```js
  const proxy = createProxy({
    '/api': { target: 'http://localhost:4000', rewrite: { '/api': '' } },
    '/auth': { target: 'http://localhost:5000' },
  });
  proxy.listen(3000);
  ```

  - Features: URL rewriting, header modification, load balancing, caching
  - **Output:** Reverse proxy server

### Task 217 — Build: File Upload Service 🟡

- Handle file uploads:
  - Multipart form data parsing (from scratch!)
  - Stream-based upload (no memory overflow)
  - File validation (type, size)
  - Unique filename generation
  - Storage: local filesystem
  - Image resize (using sharp or canvas)
  - **Output:** File upload microservice

### Task 218 — Build: Task Queue with Redis-like In-Memory Store 🔴

- Job processing:
  - In-memory job queue
  - Workers process jobs
  - Priority queues
  - Delayed jobs
  - Retry with exponential backoff
  - Job status tracking
  - Pub/Sub for notifications
  - Persistence to disk
  - **Output:** Redis-lite + Bull-lite

### Task 219 — Build: CLI Framework ⚫

- Build your own Commander.js:

  ```js
  const cli = createCLI('myapp', '1.0.0');

  cli
    .command('init [name]')
    .description('Initialize project')
    .option('-t, --template <type>', 'Template type', 'default')
    .action((name, options) => {
      /* ... */
    });

  cli
    .command('serve')
    .option('-p, --port <number>', 'Port', 3000)
    .action((options) => {
      /* ... */
    });

  cli.parse(process.argv);
  ```

  - Features: commands, arguments, options, help text, validation, autocomplete
  - **Output:** CLI framework library

### Task 220 — Build: Complete Node.js Toolkit ⚫

- Phase 07 graduation — combine everything:
  - Mini Express (from Task 202)
  - Template Engine (Task 203)
  - Database Layer (Task 204)
  - Auth System (Task 205)
  - Logger (Task 208)
  - CLI (Task 219)
  - **Combined into:** Full-stack toolkit
  - **Output:** Create a blog with your own framework — zero external dependencies for core functionality

---

## ✅ Phase 07 Completion Checklist

- [ ] Node.js architecture ও event loop phases বোঝো
- [ ] File system operations confidently পারো
- [ ] Streams ও Buffers পারো
- [ ] HTTP server without framework বানাতে পারো
- [ ] Child processes ও Worker threads ব্যবহার করতে পারো
- [ ] npm package create ও publish করতে পারো
- [ ] CLI tools build করতে পারো
- [ ] Database interaction (SQL) পারো
- [ ] Auth (JWT, sessions, hashing) implement করতে পারো
- [ ] WebSocket server build করতে পারো
- [ ] Mini Express-like framework build করতে পারো
- [ ] Node.js security best practices জানো

> ✅ Ready? → **JS Phase 08: Metaprogramming**
