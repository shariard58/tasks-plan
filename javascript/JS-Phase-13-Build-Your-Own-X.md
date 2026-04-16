# JS Phase 13 — Build Your Own X: Framework Author Level (Tasks 386-430)

> সবাই framework USE করে। তুমি framework BUILD করবে। এই phase এর পর তুমি React, Express, Webpack, Redux — সব internally বুঝবে।
> কারণ তুমি এদের clone নিজে build করেছো।

---

## Section A: Build Your Own Core Libraries (Tasks 386-398)

### Task 386 — Build Your Own: Promise (A+ Spec Compliant) ⚫

- Promise A+ specification implement করো:

  ```js
  const p = new MyPromise((resolve, reject) => {
    setTimeout(() => resolve(42), 1000);
  });

  p.then((val) => val * 2)
    .then((val) => console.log(val)) // 84
    .catch((err) => console.error(err));
  ```

  - **Requirements:**
    - `new MyPromise(executor)` — executor receives resolve, reject
    - `.then(onFulfilled, onRejected)` — chainable, returns new Promise
    - `.catch(onRejected)` — sugar for `.then(null, onRejected)`
    - `.finally(callback)` — runs regardless of outcome
    - States: pending → fulfilled/rejected (irreversible)
    - Async resolution: `.then` callbacks run asynchronously (microtask)
    - Thenable assimilation: if `.then` returns a thenable, wait for it
  - **Static methods:**
    - `MyPromise.resolve(value)` / `MyPromise.reject(reason)`
    - `MyPromise.all(promises)` — all must resolve
    - `MyPromise.allSettled(promises)` — wait for all, regardless of outcome
    - `MyPromise.race(promises)` — first to settle wins
    - `MyPromise.any(promises)` — first to resolve wins
  - **Test:** Run against Promise A+ test suite (promises-aplus-tests)
  - **Why:** Async programming এর foundation — Promise internally বুঝলে async code trivial হয়ে যায়

### Task 387 — Build Your Own: EventEmitter 🟡

- Node.js EventEmitter clone:

  ```js
  const emitter = new MyEventEmitter();

  emitter.on('data', (msg) => console.log(msg));
  emitter.once('connect', () => console.log('Connected!'));
  emitter.emit('data', 'Hello');
  emitter.emit('connect');
  emitter.emit('connect'); // Won't fire (once)
  emitter.off('data', handler);
  ```

  - **Features:**
    - `on(event, handler)` — add listener
    - `once(event, handler)` — one-time listener
    - `off(event, handler)` — remove specific listener
    - `emit(event, ...args)` — trigger event
    - `removeAllListeners(event?)` — remove all
    - `listeners(event)` — get listener array
    - `listenerCount(event)` — count
    - `eventNames()` — all registered events
    - `setMaxListeners(n)` — warn on too many
    - Error event handling (throw if no error handler)
    - Wildcard events: `emitter.on('*', handler)`

### Task 388 — Build Your Own: Observable (RxJS-like) 🔴

- Reactive streams:

  ```js
  const obs = new MyObservable((subscriber) => {
    subscriber.next(1);
    subscriber.next(2);
    subscriber.next(3);
    subscriber.complete();
  });

  obs
    .pipe(
      filter((x) => x > 1),
      map((x) => x * 10)
    )
    .subscribe({
      next: (val) => console.log(val), // 20, 30
      complete: () => console.log('Done'),
    });
  ```

  - **Core:**
    - `Observable`, `Subscriber`, `Subscription`
    - `subscribe({ next, error, complete })`
    - Lazy execution (nothing happens until subscribe)
    - Unsubscription (teardown)
  - **Operators (build 15+):**
    - `map`, `filter`, `reduce`, `take`, `skip`
    - `debounceTime`, `throttleTime`
    - `mergeMap`, `switchMap`, `concatMap`
    - `catchError`, `retry`
    - `distinctUntilChanged`
    - `combineLatest`, `merge`, `zip`
  - **Creation:** `from(array)`, `fromEvent(el, type)`, `interval(ms)`, `of(...values)`

### Task 389 — Build Your Own: Lodash Utility Library 🟡

- 50+ utility functions:
  - **Array:** `chunk`, `compact`, `difference`, `flatten`, `flattenDeep`, `intersection`, `union`, `uniq`, `uniqBy`, `zip`, `range`, `shuffle`, `sample`, `sortBy`, `groupBy`, `partition`
  - **Object:** `pick`, `omit`, `merge`, `deepMerge`, `cloneDeep`, `get` (path), `set` (path), `has`, `keys`, `values`, `entries`, `invert`, `mapKeys`, `mapValues`
  - **Function:** `debounce`, `throttle`, `memoize`, `once`, `curry`, `partial`, `compose`, `pipe`, `negate`, `flip`
  - **String:** `camelCase`, `kebabCase`, `snakeCase`, `capitalize`, `truncate`, `pad`, `escape`, `unescape`, `template`
  - **Lang:** `isArray`, `isObject`, `isString`, `isNumber`, `isNaN`, `isEmpty`, `isEqual`, `isNil`, `clone`, `cloneDeep`
  - **Each function must:**
    - Handle edge cases (null, undefined, empty)
    - Be well-tested (use your test framework from Phase 12)
    - Match Lodash behavior

### Task 390 — Build Your Own: State Manager (Redux-like) 🔴

- Predictable state management:

  ```js
  // Create store
  const store = createStore(reducer, initialState);

  // Reducer
  function reducer(state, action) {
    switch (action.type) {
      case 'INCREMENT':
        return { ...state, count: state.count + 1 };
      case 'DECREMENT':
        return { ...state, count: state.count - 1 };
      case 'ADD_TODO':
        return { ...state, todos: [...state.todos, action.payload] };
      default:
        return state;
    }
  }

  // Subscribe to changes
  store.subscribe((state) => console.log('State:', state));

  // Dispatch actions
  store.dispatch({ type: 'INCREMENT' });
  store.getState(); // { count: 1, todos: [] }
  ```

  - **Features:**
    - `createStore(reducer, initialState, enhancer)`
    - `store.dispatch(action)` — update state
    - `store.getState()` — get current state
    - `store.subscribe(listener)` — listen to changes
    - **Middleware system:** `applyMiddleware(logger, thunk)`
    - **Thunk middleware:** dispatch async actions
    - **Logger middleware:** log actions ও state changes
    - **combineReducers:** merge multiple reducers
    - **Time-travel debugging:** undo/redo actions
    - DevTools: action history, state snapshots

### Task 391 — Build Your Own: Router (SPA) 🔴

- Client-side routing:

  ```js
  const router = new MyRouter();

  router.route('/', () => renderHome());
  router.route('/about', () => renderAbout());
  router.route('/users/:id', (params) => renderUser(params.id));
  router.route('/posts/:id/comments/:commentId', (params) => renderComment(params));
  router.route('*', () => render404());

  router.start(); // Listen for URL changes
  router.navigate('/users/42'); // Programmatic navigation
  ```

  - **Features:**
    - Hash-based routing (`#/path`) ও History API (`/path`)
    - Dynamic segments (`:id`, `:slug`)
    - Query parameters (`?page=2&sort=name`)
    - Wildcard routes (`*`)
    - Nested routes / route groups
    - Route guards (beforeEnter, beforeLeave)
    - Redirect
    - Link component: `<a data-route="/about">About</a>`
    - Active link highlighting
    - Route transitions (optional CSS animation hooks)
    - Scroll restoration

### Task 392 — Build Your Own: HTTP Client (Axios-like) 🟡

- Full-featured HTTP client:

  ```js
  const client = createHTTPClient({
    baseURL: 'https://api.example.com',
    headers: { Authorization: 'Bearer token' },
    timeout: 5000,
  });

  const { data, status, headers } = await client.get('/users', { params: { page: 1 } });
  await client.post('/users', { name: 'John' });
  await client.put('/users/1', { name: 'Jane' });
  await client.delete('/users/1');
  ```

  - **Features:**
    - Methods: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS
    - Request/response interceptors:
      ```js
      client.interceptors.request.use((config) => {
        config.headers['X-Request-ID'] = generateId();
        return config;
      });
      client.interceptors.response.use(
        (response) => response,
        (error) => {
          if (error.status === 401) refreshToken();
        }
      );
      ```
    - Automatic JSON parsing
    - Timeout handling
    - Request cancellation (AbortController)
    - Retry with exponential backoff
    - Progress tracking (upload/download)
    - Instance creation with defaults

### Task 393 — Build Your Own: Template Engine 🟡

- String template rendering:

  ```js
  const template = compile(`
    <h1>{{title}}</h1>
    <ul>
      {{#each items}}
        <li class="{{#if active}}active{{/if}}">
          {{name}} - {{price | currency}}
        </li>
      {{/each}}
    </ul>
    {{#if showTotal}}
      <p>Total: {{total | currency}}</p>
    {{/if}}
  `);

  const html = template({
    title: 'Products',
    items: [{ name: 'Book', price: 20, active: true }],
    showTotal: true,
    total: 20,
  });
  ```

  - **Features:**
    - Variable interpolation: `{{name}}`
    - Conditionals: `{{#if}}...{{else}}...{{/if}}`
    - Loops: `{{#each items}}...{{/each}}` (with `{{@index}}`, `{{@key}}`)
    - Filters/Pipes: `{{value | uppercase}}`, `{{price | currency}}`
    - Partials: `{{> header}}`
    - Escaping: `{{name}}` (escaped) vs `{{{html}}}` (raw)
    - Nested paths: `{{user.address.city}}`
    - Custom helpers: `{{formatDate date "YYYY-MM-DD"}}`
    - Precompilation (compile to function for performance)

### Task 394 — Build Your Own: Form Validator 🟡

- Declarative form validation:

  ```js
  const schema = createSchema({
    username: v.string().required().min(3).max(20).alphanumeric(),
    email: v.string().required().email(),
    password: v
      .string()
      .required()
      .min(8)
      .matches(/[A-Z]/, 'Must contain uppercase')
      .matches(/[0-9]/, 'Must contain number'),
    confirmPassword: v.string().required().sameAs('password'),
    age: v.number().min(13).max(120).integer(),
    website: v.string().url().optional(),
    tags: v.array().of(v.string().max(20)).min(1).max(5),
    address: v.object().shape({
      street: v.string().required(),
      city: v.string().required(),
      zip: v.string().matches(/^\d{5}$/),
    }),
  });

  const result = schema.validate(formData);
  // { valid: false, errors: { password: ['Must contain uppercase'] } }
  ```

  - **Features:**
    - Chainable API
    - Async validation (check server, e.g., username available)
    - Conditional validation
    - Custom validators
    - Error messages (customizable, i18n ready)
    - Transform values (trim, lowercase)
    - Validate on change / on blur / on submit

### Task 395 — Build Your Own: Dependency Injection Container 🔴

- IoC Container:

  ```js
  const container = new Container();

  // Register
  container.register('config', () => ({ dbHost: 'localhost', dbPort: 5432 }));
  container.register('database', (c) => new Database(c.resolve('config')));
  container.register('userRepo', (c) => new UserRepository(c.resolve('database')));
  container.register('userService', (c) => new UserService(c.resolve('userRepo')));

  // Resolve (auto-resolves dependencies)
  const userService = container.resolve('userService');

  // Singleton vs Transient
  container.singleton('database', ...); // Same instance every time
  container.transient('request', ...);  // New instance every time

  // Scoped (per request)
  const scope = container.createScope();
  scope.resolve('userService'); // New scope, new instances
  ```

  - **Features:**
    - Lazy resolution
    - Circular dependency detection
    - Lifetime management (singleton, transient, scoped)
    - Auto-wiring (resolve dependencies from constructor)
    - Decorator support
    - Module system (group registrations)

### Task 396 — Build Your Own: ORM (Object-Relational Mapper) 🔴

- Database abstraction:

  ```js
  // Define model
  class User extends Model {
    static table = 'users';
    static schema = {
      id: { type: 'integer', primaryKey: true, autoIncrement: true },
      name: { type: 'string', required: true },
      email: { type: 'string', unique: true },
      age: { type: 'integer' },
      createdAt: { type: 'datetime', default: () => new Date() },
    };

    static relations = {
      posts: { type: 'hasMany', model: 'Post', foreignKey: 'userId' },
      profile: { type: 'hasOne', model: 'Profile', foreignKey: 'userId' },
    };
  }

  // Query builder
  const users = await User.where({ age: { $gt: 18 } })
    .select('name', 'email')
    .orderBy('name', 'asc')
    .limit(10)
    .offset(20)
    .include('posts')
    .execute();

  // CRUD
  const user = await User.create({ name: 'John', email: 'john@test.com' });
  await User.update(user.id, { name: 'Jane' });
  await User.delete(user.id);
  ```

  - **Features:**
    - Schema definition ও validation
    - Query builder (chainable)
    - Relations (hasOne, hasMany, belongsTo, manyToMany)
    - Migrations (create/alter tables)
    - Seeding (populate with test data)
    - SQLite backend (pure JS or better-sqlite3)

### Task 397 — Build Your Own: Task Queue / Job Scheduler 🟡

- Background job processing:

  ```js
  const queue = new TaskQueue({ concurrency: 3 });

  // Add jobs
  queue.add('sendEmail', { to: 'user@test.com', subject: 'Hello' });
  queue.add('processImage', { path: '/uploads/photo.jpg' }, { priority: 'high' });
  queue.add('generateReport', { userId: 123 }, { delay: 60000 }); // Delay 1 min

  // Process jobs
  queue.process('sendEmail', async (job) => {
    await sendEmail(job.data.to, job.data.subject);
  });

  // Events
  queue.on('completed', (job) => console.log(`Job ${job.id} done`));
  queue.on('failed', (job, err) => console.error(`Job ${job.id} failed:`, err));
  ```

  - **Features:**
    - Priority queues (high, normal, low)
    - Delayed jobs
    - Retry with backoff
    - Concurrency control
    - Job persistence (file-based)
    - Dead letter queue (failed jobs)
    - Rate limiting
    - Cron-like scheduling (`schedule('*/5 * * * *', task)`)
    - Dashboard (web UI showing queue status)

### Task 398 — Build Your Own: Logger 🟡

- Production-grade logging:

  ```js
  const log = createLogger({
    level: 'info',
    format: combine(timestamp(), json()),
    transports: [
      new ConsoleTransport({ level: 'debug', colorize: true }),
      new FileTransport({ filename: 'app.log', level: 'info' }),
      new FileTransport({ filename: 'error.log', level: 'error' }),
    ],
  });

  log.info('Server started', { port: 3000 });
  log.warn('Slow query', { query: 'SELECT...', duration: 500 });
  log.error('Database connection failed', { host: 'db.example.com', error: err });

  // Child logger (inherit config, add context)
  const reqLog = log.child({ requestId: 'abc123', userId: 42 });
  reqLog.info('Processing request'); // Includes requestId and userId
  ```

  - **Features:**
    - Log levels: trace, debug, info, warn, error, fatal
    - Transports: console, file, HTTP, custom
    - Formatters: JSON, text, pretty-print
    - Structured logging (key-value pairs)
    - Child loggers (inherited context)
    - Log rotation (by size or date)
    - Async logging (don't block main thread)
    - Sampling (log 1% of debug messages in production)

---

## Section B: Build Your Own Frontend Tools (Tasks 399-410)

### Task 399 — Build Your Own: Virtual DOM + Component Framework 🔴

- Build React-like framework:

  ```js
  // User writes components like this:
  function Counter() {
    const [count, setCount] = useState(0);

    return h(
      'div',
      {},
      h('h1', {}, `Count: ${count}`),
      h('button', { onClick: () => setCount(count + 1) }, '+'),
      h('button', { onClick: () => setCount(count - 1) }, '-')
    );
  }

  // Or with JSX-like template string:
  function Counter() {
    const [count, setCount] = useState(0);
    return html`
      <div>
        <h1>Count: ${count}</h1>
        <button onclick=${() => setCount(count + 1)}>+</button>
      </div>
    `;
  }

  mount(Counter, document.getElementById('app'));
  ```

  - **Features:**
    - `h(tag, props, ...children)` — createElement
    - Virtual DOM diffing ও patching
    - `useState` hook
    - `useEffect` hook (with cleanup)
    - `useMemo` / `useCallback`
    - Component lifecycle (mount, update, unmount)
    - Props passing
    - Conditional rendering
    - List rendering (with keys)
    - Event delegation
    - Batched state updates

### Task 400 — Build Your Own: CSS-in-JS Library 🟡

- Runtime CSS generation:

  ```js
  const styles = css({
    container: {
      display: 'flex',
      padding: '16px',
      '&:hover': {
        backgroundColor: '#f0f0f0',
      },
      '@media (max-width: 768px)': {
        flexDirection: 'column',
      },
    },
    title: {
      fontSize: '24px',
      fontWeight: 'bold',
      color: (props) => (props.primary ? 'blue' : 'black'),
    },
  });

  element.className = styles.container; // "css-abc123"
  ```

  - **Features:**
    - Generate unique class names (hash-based)
    - Inject `<style>` tags into `<head>`
    - Nested selectors (`&:hover`, `& > div`)
    - Media queries
    - Pseudo-classes / pseudo-elements
    - Dynamic styles (based on props)
    - Keyframe animations
    - Global styles
    - SSR support (extract CSS string)
    - Deduplication (same styles = same class)

### Task 401 — Build Your Own: Module Bundler (Advanced) ⚫

- Webpack-like bundler with advanced features:
  ```bash
  mybundler build --entry src/index.js --output dist/
  mybundler dev --port 3000  # Dev server with HMR
  ```

  - **Features:**
    - **Dependency graph:** parse imports → build graph → topological sort
    - **Module wrapping:** each module in closure with own scope
    - **Code splitting:** dynamic `import()` → separate chunks
    - **Tree shaking:** remove unused exports (ESM only)
    - **Loaders:** transform files before bundling
      - `.css` → inject as `<style>` tag
      - `.json` → JS object
      - `.png` → base64 data URL or file copy
    - **Plugins:** hook into build lifecycle
    - **Source maps:** map bundled code back to source
    - **Dev server:** HTTP server with live reload
    - **HMR (Hot Module Replacement):** update modules without full reload
    - **Minification:** remove whitespace, shorten variables (basic)
    - **Caching:** content hash in filenames for cache busting

### Task 402 — Build Your Own: Linter 🔴

- Static code analysis tool:
  ```bash
  mylinter --config .mylintrc.json src/
  ```
  ```
  src/utils.js:15:3 error  Unexpected var, use let or const  (no-var)
  src/utils.js:23:1 warn   Function has too many parameters   (max-params)
  src/app.js:8:5   error  Missing return type                 (require-return)
  ```

  - **Architecture:**
    1. Parse source → AST
    2. Walk AST → apply rules
    3. Report violations
  - **Rules (build 15+):**
    - `no-var` — disallow `var`
    - `no-unused-vars` — detect unused variables
    - `no-console` — disallow `console.log`
    - `eqeqeq` — require `===` instead of `==`
    - `no-eval` — disallow `eval()`
    - `max-params` — max function parameters
    - `max-depth` — max nesting depth
    - `no-shadow` — disallow variable shadowing
    - `no-implicit-globals` — no global leaks
    - `camelcase` — enforce naming convention
    - `no-duplicate-keys` — no duplicate object keys
    - `no-unreachable` — code after return/throw
    - `prefer-const` — use const when never reassigned
    - `no-empty-function` — disallow empty functions
    - `consistent-return` — ensure consistent return types
  - **Features:**
    - Config file (`.mylintrc.json`)
    - Rule severity: off, warn, error
    - Auto-fix (for fixable rules)
    - Plugin system (custom rules)
    - Ignore patterns (`.mylintignore`)

### Task 403 — Build Your Own: Formatter (Prettier-like) 🔴

- Automatic code formatting:
  ```bash
  myformatter --write src/
  myformatter --check src/  # Check without modifying
  ```

  - **Architecture:**
    1. Parse source → AST
    2. Generate "intermediate representation" (IR) — abstract layout
    3. Print IR → formatted string (with line width constraints)
  - **Formatting rules:**
    - Consistent indentation (spaces/tabs, configurable)
    - Line width limit (80/100/120)
    - Semicolons (add/remove)
    - Quotes (single/double)
    - Trailing commas
    - Bracket spacing
    - Arrow function parens
    - Object/array line breaking (single line if short, multi-line if long)
  - **Config file:** `.myformatterrc.json`
  - **Editor integration:** format on save (VS Code extension idea)

### Task 404 — Build Your Own: Dev Server with HMR 🔴

- Development server with Hot Module Replacement:
  - **HTTP server:** serve static files
  - **File watcher:** `fs.watch()` for changes
  - **WebSocket:** push updates to browser
  - **HMR protocol:**
    1. File changes detected
    2. Rebuild changed module
    3. Send update via WebSocket
    4. Client-side: replace module without full reload
  - **Features:**
    - Static file serving
    - Auto-reload on file change
    - HMR for JS and CSS
    - Error overlay (show compile errors in browser)
    - Proxy (forward API requests to backend)
    - HTTPS support
    - Open browser on start

### Task 405 — Build Your Own: Reactive UI Library (Svelte-like) ⚫

- Compile-time reactivity:

  ```js
  // Component definition
  const Counter = component({
    state: { count: 0 },
    template: `
      <div>
        <h1>Count: {count}</h1>
        <button @click="count++">+</button>
        <button @click="count--">-</button>
      </div>
    `,
  });

  // OR: compile-time approach
  // Compiler transforms assignments into DOM updates:
  // count = count + 1  →  count = count + 1; $$update('count', count);
  ```

  - **Features:**
    - Template parser (parse custom syntax → AST)
    - Compiler: template → optimized DOM operations
    - Reactive assignments (assignment triggers update)
    - No Virtual DOM — direct DOM manipulation (like Svelte)
    - Each blocks: `{#each items as item}...{/each}`
    - If blocks: `{#if condition}...{:else}...{/if}`
    - Two-way binding: `bind:value={name}`
    - Event handlers: `@click`, `@input`
    - Lifecycle: `onMount`, `onDestroy`
    - Scoped CSS

### Task 406 — Build Your Own: SSR (Server-Side Rendering) Engine 🔴

- Render components on server:

  ```js
  // Server
  const app = createServer();

  app.get('*', (req, res) => {
    const component = matchRoute(req.url);
    const data = await fetchData(req.url);
    const html = renderToString(component, data);

    res.send(`
      <!DOCTYPE html>
      <html>
        <body>
          <div id="app">${html}</div>
          <script>window.__DATA__ = ${JSON.stringify(data)}</script>
          <script src="/bundle.js"></script>
        </body>
      </html>
    `);
  });
  ```

  - **Features:**
    - `renderToString(component, props)` — component → HTML string
    - Hydration: client takes over server-rendered HTML
    - Data fetching on server
    - Streaming: `renderToStream()` for large pages
    - Route-based code splitting
    - Meta tags / SEO handling

### Task 407 — Build Your Own: Static Type Checker (Basic) ⚫

- Type check JavaScript with annotations:

  ```js
  // @type {string}
  let name = 'John';
  name = 42; // TypeError: Cannot assign number to string

  // @param {number} a
  // @param {number} b
  // @returns {number}
  function add(a, b) {
    return a + b;
  }

  add('hello', 'world'); // TypeError: Expected number, got string
  ```

  - **Architecture:**
    1. Parse JS → AST
    2. Extract type annotations from comments
    3. Infer types where not annotated
    4. Check assignments, function calls, returns
    5. Report type errors
  - **Type system:**
    - Primitives: string, number, boolean, null, undefined
    - Objects: `{ name: string, age: number }`
    - Arrays: `Array<number>`, `string[]`
    - Functions: `(a: number, b: number) => number`
    - Union: `string | number`
    - Optional: `name?: string`
  - **This is a simplified TypeScript!**

### Task 408 — Build Your Own: Package Registry & CLI 🟡

- npm registry clone:
  ```bash
  mypkg init                    # Create package.json
  mypkg publish                 # Publish to registry
  mypkg install <package>       # Install package
  mypkg install                 # Install all from package.json
  mypkg uninstall <package>     # Remove package
  mypkg search <query>          # Search registry
  mypkg info <package>          # Package details
  ```

  - **Registry server:**
    - Store packages (tarballs)
    - Package metadata API
    - Search API
    - Auth (publish requires login)
    - Scoped packages (`@scope/package`)
  - **CLI:**
    - Dependency resolution (handle conflicts)
    - Lockfile generation
    - `node_modules` installation
    - Scripts execution
    - Version management (semver)

### Task 409 — Build Your Own: WebSocket Library 🟡

- WebSocket abstraction:

  ```js
  // Server
  const wss = new MyWebSocketServer({ port: 8080 });
  wss.on('connection', (socket) => {
    socket.on('message', (data) => {
      wss.broadcast(data); // Send to all
    });
    socket.send('Welcome!');
  });

  // Client
  const ws = new MyWebSocket('ws://localhost:8080');
  ws.on('open', () => ws.send('Hello'));
  ws.on('message', (data) => console.log(data));
  ws.on('close', () => console.log('Disconnected'));

  // Features
  ws.on('reconnect', () => {}); // Auto-reconnect
  ```

  - **Features:**
    - Auto-reconnect with exponential backoff
    - Heartbeat (ping/pong)
    - Rooms/channels
    - Binary message support
    - Message compression
    - Authentication (token on connect)
    - Rate limiting
    - Graceful shutdown

### Task 410 — Build Your Own: GraphQL Server 🔴

- GraphQL from scratch:

  ```js
  const schema = buildSchema(`
    type User {
      id: ID!
      name: String!
      email: String!
      posts: [Post!]!
    }
  
    type Post {
      id: ID!
      title: String!
      author: User!
    }
  
    type Query {
      user(id: ID!): User
      users: [User!]!
    }
  
    type Mutation {
      createUser(name: String!, email: String!): User!
    }
  `);

  const resolvers = {
    Query: {
      user: (_, { id }) => db.getUser(id),
      users: () => db.getAllUsers(),
    },
    User: {
      posts: (user) => db.getPostsByUser(user.id),
    },
    Mutation: {
      createUser: (_, { name, email }) => db.createUser(name, email),
    },
  };

  const server = createGraphQLServer({ schema, resolvers });
  ```

  - **Features:**
    - Schema parser (GraphQL SDL → AST)
    - Query parser (parse GraphQL queries → AST)
    - Query executor (resolve fields recursively)
    - Type system: Scalar, Object, List, NonNull, Enum, Input
    - Mutations
    - Variables
    - Fragments
    - Introspection (query schema itself)
    - Validation (check query against schema)
    - N+1 problem: DataLoader-like batching

---

## Section C: Build Your Own Backend Infrastructure (Tasks 411-430)

### Task 411 — Build Your Own: Web Framework (Express-like) ⚫

- HTTP framework from scratch:

  ```js
  const app = new MyFramework();

  // Middleware
  app.use(cors());
  app.use(bodyParser());
  app.use(logger());

  // Routes
  app.get('/api/users', (req, res) => {
    res.json(users);
  });

  app.post('/api/users', validate(userSchema), (req, res) => {
    const user = createUser(req.body);
    res.status(201).json(user);
  });

  app.get('/api/users/:id', (req, res) => {
    const user = getUser(req.params.id);
    if (!user) return res.status(404).json({ error: 'Not found' });
    res.json(user);
  });

  // Route groups
  const api = app.group('/api/v2');
  api.use(authMiddleware);
  api.get('/users', listUsers);

  app.listen(3000, () => console.log('Server on :3000'));
  ```

  - **Features:**
    - Routing: GET, POST, PUT, PATCH, DELETE
    - Route parameters (`:id`)
    - Query parameters
    - Middleware chain (`app.use()`)
    - Route-specific middleware
    - Route groups (prefix)
    - Request parsing (JSON, form data, multipart)
    - Response helpers (`.json()`, `.send()`, `.status()`, `.redirect()`)
    - Static file serving
    - Error handling middleware
    - Cookie parsing & setting
    - CORS middleware
    - Body parser middleware

### Task 412 — Build Your Own: Database Migration System 🟡

- Schema versioning:

  ```bash
  mydb migrate:create add_users_table
  mydb migrate:up
  mydb migrate:down
  mydb migrate:status
  ```

  ```js
  // migrations/001_add_users_table.js
  exports.up = async (db) => {
    await db.execute(`
      CREATE TABLE users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        email TEXT UNIQUE NOT NULL,
        created_at DATETIME DEFAULT CURRENT_TIMESTAMP
      )
    `);
  };

  exports.down = async (db) => {
    await db.execute('DROP TABLE users');
  };
  ```

  - **Features:**
    - Migration file generation (timestamped)
    - Up/down migrations
    - Migration tracking (which have run)
    - Rollback (single, all)
    - Status report
    - Schema seeding

### Task 413 — Build Your Own: Caching Layer 🟡

- Multi-strategy cache:

  ```js
  const cache = createCache({
    strategy: 'lru', // LRU, LFU, FIFO, TTL
    maxSize: 1000,
    ttl: 60000, // 60 seconds default
    onEvict: (key, val) => console.log(`Evicted: ${key}`),
  });

  cache.set('user:1', { name: 'John' });
  cache.get('user:1'); // { name: 'John' }
  cache.has('user:1'); // true
  cache.delete('user:1');
  cache.clear();

  // Wrap function with cache
  const getUser = cache.wrap(
    'user',
    async (id) => {
      return await db.getUser(id);
    },
    { ttl: 300000 }
  );
  ```

  - **Strategies:**
    - **LRU (Least Recently Used):** evict least recently accessed
    - **LFU (Least Frequently Used):** evict least frequently accessed
    - **FIFO:** evict oldest
    - **TTL:** evict after time-to-live
  - **Features:** stats, events, serialization, namespace

### Task 414 — Build Your Own: Rate Limiter (Advanced) 🟡

- Multiple algorithms:

  ```js
  const limiter = createRateLimiter({
    algorithm: 'sliding-window', // fixed-window, sliding-window, token-bucket, leaky-bucket
    limit: 100,
    window: 60000, // 1 minute
    keyGenerator: (req) => req.ip,
  });

  // Middleware
  app.use('/api/', limiter.middleware());

  // Programmatic
  const { allowed, remaining, resetAt } = limiter.check('user:123');
  ```

  - **Build all 4 algorithms** with correct behavior
  - **Storage:** in-memory, Redis-compatible interface
  - **Per-route limits** (different limits for different endpoints)
  - **Distributed rate limiting** (multiple Node.js instances)

### Task 415 — Build Your Own: Pub/Sub Message Broker 🔴

- Message queue system:

  ```js
  const broker = new MessageBroker();

  // Subscribe
  broker.subscribe('user.created', async (message) => {
    await sendWelcomeEmail(message.data);
  });

  broker.subscribe('user.created', async (message) => {
    await createDefaultProfile(message.data);
  });

  // Publish
  broker.publish('user.created', { id: 1, name: 'John', email: 'john@test.com' });

  // Pattern matching
  broker.subscribe('user.*', handler); // Any user event
  broker.subscribe('*.created', handler); // Any creation event
  ```

  - **Features:**
    - Topic-based routing
    - Pattern matching (wildcards)
    - Message persistence (file-based)
    - At-least-once delivery
    - Dead letter queue
    - Message acknowledgment
    - Consumer groups
    - Message replay (from offset)

### Task 416 — Build Your Own: API Gateway 🔴

- Microservice gateway:

  ```js
  const gateway = createGateway({
    routes: {
      '/api/users/*': { target: 'http://user-service:3001', auth: true },
      '/api/posts/*': { target: 'http://post-service:3002', auth: true },
      '/api/auth/*': { target: 'http://auth-service:3003', auth: false },
    },
  });

  gateway.use(rateLimiter());
  gateway.use(authentication());
  gateway.use(requestLogger());
  gateway.use(circuitBreaker());

  gateway.listen(8080);
  ```

  - **Features:**
    - Reverse proxy (forward requests to backends)
    - Load balancing (round-robin, least connections)
    - Circuit breaker (stop calling failing services)
    - Health checks (periodic ping)
    - Request/response transformation
    - API key management
    - Rate limiting per service
    - Response caching
    - Request aggregation (combine multiple service calls)

### Task 417 — Build Your Own: CRON Scheduler 🟡

- Task scheduling:

  ```js
  const scheduler = new CronScheduler();

  // Standard cron syntax
  scheduler.schedule('*/5 * * * *', () => {
    console.log('Every 5 minutes');
  });

  scheduler.schedule('0 9 * * 1-5', () => {
    sendDailyReport(); // 9 AM weekdays
  });

  scheduler.schedule('0 0 1 * *', () => {
    generateMonthlyInvoices(); // 1st of every month
  });
  ```

  - **Build:**
    - Cron expression parser (minute, hour, day, month, weekday)
    - Cron expression validator
    - Next run time calculator
    - Job execution (with error handling)
    - Job history (last run, next run, success/failure)
    - Overlap protection (don't start if previous still running)
    - Timezone support

### Task 418 — Build Your Own: Feature Flag System 🟡

- Feature toggles:

  ```js
  const flags = new FeatureFlags({
    'new-dashboard': {
      enabled: true,
      rollout: 50, // 50% of users
      rules: [
        { attribute: 'role', operator: 'in', value: ['admin', 'beta'] },
        { attribute: 'country', operator: 'eq', value: 'BD' },
      ],
    },
    'dark-mode': { enabled: true }, // Everyone
    'experimental-api': { enabled: false }, // Nobody
  });

  if (flags.isEnabled('new-dashboard', user)) {
    renderNewDashboard();
  } else {
    renderOldDashboard();
  }
  ```

  - **Features:**
    - Boolean flags (on/off)
    - Percentage rollout
    - User targeting (rules-based)
    - A/B testing support
    - Flag management API
    - Real-time flag updates (WebSocket)
    - Dashboard UI

### Task 419 — Build Your Own: CLI Framework 🟡

- Build CLI tools easily:

  ```js
  const cli = new MyCLI('mytool', 'A cool CLI tool');

  cli
    .command('init')
    .description('Initialize a new project')
    .option('-n, --name <name>', 'Project name', 'my-project')
    .option('-t, --template <template>', 'Template to use', 'default')
    .option('--no-git', 'Skip git initialization')
    .action(async (options) => {
      console.log(`Creating ${options.name} with ${options.template} template`);
    });

  cli
    .command('build')
    .description('Build the project')
    .option('--watch', 'Watch for changes')
    .option('--minify', 'Minify output')
    .action(buildCommand);

  cli.parse(process.argv);
  ```

  - **Features:**
    - Command registration with options ও arguments
    - Auto-generated help (`--help`)
    - Version flag (`--version`)
    - Option parsing (boolean, string, array)
    - Required vs optional arguments
    - Default values
    - Interactive prompts (question, confirm, select, multiselect)
    - Progress bar ও spinner
    - Colors (ANSI escape codes)
    - Tab completion

### Task 420 — Build Your Own: Configuration System 🟡

- Unified config from multiple sources:

  ```js
  const config = createConfig({
    sources: [
      defaults({ port: 3000, host: 'localhost' }),
      envFile('.env'),
      envVars('APP_'), // APP_PORT → port
      jsonFile('config.json'),
      args(process.argv), // --port=8080
    ],
    schema: {
      port: { type: 'number', required: true },
      host: { type: 'string', required: true },
      dbUrl: { type: 'string', required: true, secret: true },
      debug: { type: 'boolean', default: false },
    },
  });

  config.get('port'); // 8080 (from CLI args, highest priority)
  config.get('dbUrl'); // From .env file
  config.getAll(); // Complete config object
  ```

  - **Priority:** CLI args > env vars > env file > config file > defaults
  - **Features:**
    - Type coercion (string "3000" → number 3000)
    - Validation (required, type, range)
    - Secret masking (don't log sensitive values)
    - Nested keys (`database.host`)
    - Watch for changes (hot reload config)

### Task 421 — Build Your Own: Workflow Engine 🔴

- Define ও execute workflows:

  ```js
  const workflow = defineWorkflow('user-onboarding', {
    steps: [
      {
        name: 'validate-email',
        handler: async (ctx) => {
          const isValid = await verifyEmail(ctx.data.email);
          if (!isValid) throw new Error('Invalid email');
          return { ...ctx.data, emailVerified: true };
        },
      },
      {
        name: 'create-account',
        handler: async (ctx) => {
          const user = await createUser(ctx.data);
          return { ...ctx.data, userId: user.id };
        },
        onError: 'cleanup', // Go to cleanup step on error
      },
      {
        name: 'send-welcome',
        handler: async (ctx) => {
          await sendEmail(ctx.data.email, 'Welcome!');
        },
        retries: 3,
      },
    ],
    onComplete: (result) => console.log('Onboarding complete:', result),
    onError: (error, step) => console.error(`Failed at ${step}:`, error),
  });

  await workflow.execute({ email: 'john@test.com', name: 'John' });
  ```

  - **Features:**
    - Sequential ও parallel steps
    - Conditional branching
    - Error handling ও retry
    - Step timeout
    - Workflow state persistence
    - Resume from failure point
    - Workflow versioning
    - Dashboard (visualize workflow execution)

### Task 422-430 — Build Your Own: Complete Application Framework ⚫

- **The ultimate capstone of Phase 13** — combine everything into one cohesive framework:

### Task 422 — Framework Core & Plugin Architecture ⚫

- Plugin system where features are pluggable:
  ```js
  const app = new MyFramework();
  app.plugin(routerPlugin());
  app.plugin(databasePlugin({ url: 'sqlite://app.db' }));
  app.plugin(authPlugin({ secret: 'jwt-secret' }));
  app.plugin(validationPlugin());
  ```

### Task 423 — Framework: Database Layer ⚫

- ORM + migrations + seeding integrated into framework

### Task 424 — Framework: Authentication Module ⚫

- JWT + session + OAuth built-in

### Task 425 — Framework: Validation Module ⚫

- Request validation with schema definition

### Task 426 — Framework: Testing Module ⚫

- Built-in test runner for the framework

### Task 427 — Framework: CLI Module ⚫

- `myframework new project-name`, `myframework generate model User`, `myframework serve`

### Task 428 — Framework: Error Handling & Logging ⚫

- Structured error handling, logging, monitoring

### Task 429 — Framework: Documentation Generator ⚫

- Auto-generate API docs from route definitions

### Task 430 — Framework: Production Deployment ⚫

- Build, optimize, deploy pipeline
- **Final output: A complete, documented, tested framework — publishable to npm**

---

## ✅ Phase 13 Completion Checklist

- [ ] All 45 tasks complete
- [ ] Built Promise A+ compliant from scratch
- [ ] Built Redux-like state manager
- [ ] Built React-like component framework
- [ ] Built Express-like HTTP framework
- [ ] Built Webpack-like module bundler
- [ ] Built ESLint-like linter
- [ ] Built GraphQL server
- [ ] Built complete application framework
- [ ] At least 5 projects published to npm
- [ ] Can explain any framework's internals because you've built similar tools

> **তুমি এখন framework author — কোনো library তোমাকে ভয় দেখাতে পারবে না কারণ তুমি নিজে build করতে পারো।** 🏗️
