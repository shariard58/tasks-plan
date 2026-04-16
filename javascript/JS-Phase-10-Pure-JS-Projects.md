# JS Phase 10 — Pure JavaScript Projects: 50 Real-World Builds (Tasks 271-320)

> Framework দিয়ে সবাই বানায়। Pure JS দিয়ে বানালেই তুমি real developer। এই 50টা project complete করলে — তুমি JS master।
> 30টা original + 20টা নতুন advanced project — সব zero framework।

---

## 🎯 Rules for Phase 10

1. **ZERO frameworks** — no React, Vue, Angular, jQuery, Express
2. **ZERO libraries** — unless the project specifically says so
3. **Only built-in:** DOM API, Fetch, Canvas, Web APIs, Node.js built-in modules
4. **Each project:** must have clean code, error handling, responsive design (where applicable)
5. **Each project:** must be deployed ও in portfolio

---

## Section A: UI & DOM Projects (Tasks 271-278)

### Task 271 — Interactive To-Do App ⚫

- Features:
  - Add, edit, delete, complete tasks
  - Categories ও tags
  - Due dates with overdue highlighting
  - Priority levels (high, medium, low)
  - Filter: all, active, completed, by category, by tag
  - Sort: by date, priority, name
  - Search tasks
  - Drag & drop reorder
  - localStorage persistence
  - Undo/redo (Command pattern)
  - Keyboard shortcuts (Ctrl+N, Delete, Ctrl+Z)
  - Responsive design
  - Dark/light theme toggle
  - **Concepts used:** DOM manipulation, events, delegation, localStorage, drag & drop, keyboard events

### Task 272 — Real-Time Chat Application ⚫

- Frontend + Backend:
  - WebSocket server (Node.js, no libraries)
  - Chat rooms / channels
  - Private messaging
  - User list (online/offline)
  - Typing indicator
  - Message timestamps
  - Emoji picker (pure CSS/JS)
  - Image sharing (File API + Base64)
  - Message search
  - Notifications (Notification API)
  - Auto-reconnect on disconnect
  - Message history (persist to file)
  - **Concepts used:** WebSocket, Node.js HTTP, File API, Notification API, event-driven architecture

### Task 273 — Kanban Board (Trello Clone) ⚫

- Features:
  - Multiple boards
  - Columns (To Do, In Progress, Done, custom)
  - Cards with title, description, labels, due date
  - Drag & drop between columns
  - Card editing (modal)
  - Labels with colors
  - Search ও filter
  - Board ও card data in localStorage
  - Keyboard navigation
  - Responsive (works on mobile)
  - **Concepts used:** Drag & Drop API, DOM manipulation, localStorage, custom events, CSS Grid

### Task 274 — Markdown Editor with Live Preview ⚫

- Features:
  - Split view: editor | preview
  - Real-time preview (debounced)
  - Markdown → HTML parser (build from scratch!):
    - Headers (#, ##, ###)
    - Bold, italic, strikethrough
    - Links, images
    - Code blocks (with syntax highlighting)
    - Lists (ordered, unordered, nested)
    - Blockquotes
    - Tables
    - Horizontal rules
  - Toolbar buttons (bold, italic, link, image, code)
  - File save/load (File System Access API)
  - Export as HTML
  - Word count, reading time
  - **Concepts used:** Regex, string processing, DOM manipulation, File API, debounce

### Task 275 — Drawing / Whiteboard App ⚫

- Canvas-based:
  - Free draw (pen tool)
  - Shapes: line, rectangle, circle, ellipse, triangle
  - Color picker
  - Brush size
  - Eraser
  - Fill tool
  - Text tool
  - Undo/redo (store canvas states)
  - Layers
  - Zoom ও pan
  - Export as PNG/SVG
  - Touch support (mobile drawing)
  - **Concepts used:** Canvas API, mouse/touch events, File API, object pooling

### Task 276 — Spreadsheet (Mini Excel) ⚫

- Features:
  - Grid with cells (A1, B2, etc.)
  - Cell editing (click to edit)
  - Formula support:
    - Basic math: `=A1+B1`, `=A1*2`
    - Functions: `=SUM(A1:A10)`, `=AVERAGE(B1:B5)`, `=MAX()`, `=MIN()`, `=COUNT()`
    - Cell references (auto-update when reference changes)
  - Column resize (drag)
  - Row/column add/delete
  - Cell formatting (bold, color, alignment)
  - Copy/paste
  - CSV export/import
  - Multiple sheets (tabs)
  - **Concepts used:** DOM table, custom event system, formula parser, observer pattern

### Task 277 — Image Editor ⚫

- Canvas-based image manipulation:
  - Load image (File API)
  - Filters: brightness, contrast, saturation, grayscale, sepia, blur, sharpen
  - Crop tool
  - Resize
  - Rotate ও flip
  - Text overlay
  - Drawing on image
  - Filter history (undo/redo)
  - Export (Canvas toBlob/toDataURL)
  - Slider controls for each filter
  - **Concepts used:** Canvas API, image data manipulation (pixel-by-pixel), File/Blob API, Web Workers (for heavy filters)

### Task 278 — Music Player ⚫

- Web Audio API:
  - Play/pause, next/previous, shuffle, repeat
  - Volume control ও mute
  - Progress bar (seek)
  - Playlist management
  - Audio visualizer (frequency bars using Canvas)
  - File loading (local files or URL)
  - Keyboard controls (space = play/pause, arrows = seek)
  - Album art display
  - Playlist persistence (localStorage)
  - Background playback (Service Worker)
  - **Concepts used:** Web Audio API, Canvas, File API, keyboard events, Media Session API

---

## Section B: Utility & Tool Projects (Tasks 279-286)

### Task 279 — Task/Project Management Dashboard ⚫

- Features:
  - Project CRUD
  - Task CRUD within projects
  - Task assignment (users)
  - Timeline view (Gantt chart using Canvas)
  - Progress tracking (percentage)
  - Due dates ও overdue alerts
  - Dashboard with charts (pie, bar — Canvas)
  - Filter ও sort
  - localStorage persistence
  - **Concepts used:** Canvas (charts), DOM manipulation, data modeling, observer pattern

### Task 280 — E-Commerce Product Page ⚫

- Complete shopping experience:
  - Product listing with grid/list view
  - Product detail page (image gallery, zoom)
  - Shopping cart (add, remove, quantity, total)
  - Checkout form (validation)
  - Order summary
  - Product search ও filter (price, category, rating)
  - Sort (price, name, popularity)
  - Responsive design
  - Cart persistence (localStorage)
  - SPA navigation (History API router)
  - **Concepts used:** SPA routing, localStorage, form validation, responsive CSS, DOM templating

### Task 281 — Quiz / Exam Platform ⚫

- Features:
  - Quiz creation (admin view):
    - Multiple choice, true/false, fill-in-blank
    - Timer per question ও overall
    - Question ordering (fixed/random)
  - Quiz taking:
    - Timer with countdown
    - Question navigation
    - Mark for review
    - Progress indicator
  - Results:
    - Score calculation
    - Answer review (correct/wrong highlighting)
    - Statistics (time per question, accuracy)
  - Quiz data in JSON (localStorage)
  - **Concepts used:** Timer, DOM manipulation, data structures, statistics

### Task 282 — Weather Dashboard ⚫

- Features:
  - Fetch weather data (use free API — OpenWeather)
  - Current weather display
  - 5-day forecast
  - Search by city
  - Geolocation (current location)
  - Weather charts (temperature, humidity — Canvas)
  - Unit toggle (Celsius/Fahrenheit)
  - Favorite cities (localStorage)
  - Auto-refresh (poll every 30 min)
  - Background changes based on weather
  - **Concepts used:** Fetch API, Geolocation, Canvas charts, localStorage, responsive design

### Task 283 — File Manager ⚫

- Browser-based file management:
  - Open files (File System Access API)
  - Directory browsing
  - File preview (text, image, JSON)
  - File operations: create, rename, delete
  - File search (by name, content)
  - Breadcrumb navigation
  - Grid ও list view
  - File info (size, type, modified date)
  - Drag & drop files
  - Context menu (right-click)
  - **Concepts used:** File System Access API, Drag & Drop, context menu, DOM manipulation

### Task 284 — Budget / Expense Tracker ⚫

- Financial management:
  - Add income ও expense
  - Categories (food, transport, bills, entertainment, etc.)
  - Monthly ও yearly view
  - Charts: pie (category distribution), bar (monthly trend), line (balance over time)
  - Budget setting per category
  - Budget alerts (overspent warning)
  - Export as CSV
  - Statistics (average spending, highest category)
  - Multi-currency support (Intl.NumberFormat)
  - **Concepts used:** Canvas charts, Intl API, localStorage, CSV generation, data aggregation

### Task 285 — Code Editor (Mini VS Code) ⚫

- Features:
  - Syntax highlighting (JS, HTML, CSS):
    - Tokenizer/Lexer for each language
    - Color coding (keywords, strings, comments, numbers)
  - Line numbers
  - Auto-indent
  - Bracket matching
  - Find ও replace (with regex)
  - Tab support
  - Multiple files (tabs)
  - Auto-save (localStorage)
  - Code execution (eval in sandbox — iframe)
  - Live preview for HTML/CSS
  - Keyboard shortcuts
  - **Concepts used:** Tokenizer/lexer, DOM manipulation, regex, keyboard events, iframe sandboxing

### Task 286 — Terminal Emulator ⚫

- Browser-based terminal:
  - Command input with prompt
  - Command history (up/down arrow)
  - Built-in commands: help, clear, echo, date, ls, cat, mkdir, cd, pwd, history, alias
  - Custom command registration
  - Output formatting (colors using ANSI-like classes)
  - Tab auto-complete
  - Pipe support: `echo "hello" | uppercase`
  - Virtual file system (in-memory)
  - **Concepts used:** DOM, keyboard events, command pattern, virtual file system, string parsing

---

## Section C: Game Projects (Tasks 287-292)

### Task 287 — Snake Game ⚫

- Classic snake:
  - Canvas rendering
  - Snake movement (arrow keys / WASD)
  - Food generation (random position)
  - Score tracking
  - Speed increase with score
  - Game over detection (wall/self collision)
  - High score (localStorage)
  - Pause/resume
  - Grid-based rendering
  - Touch controls (swipe on mobile)
  - **Concepts used:** Canvas, requestAnimationFrame, keyboard events, game loop, collision detection

### Task 288 — Tetris ⚫

- Full Tetris game:
  - 7 tetromino shapes (I, O, T, S, Z, J, L)
  - Rotation (SRS — Super Rotation System)
  - Hard drop ও soft drop
  - Line clearing (with animation)
  - Score: single, double, triple, tetris
  - Level system (speed increase)
  - Next piece preview
  - Hold piece
  - Ghost piece (drop preview)
  - Game over detection
  - High score board
  - **Concepts used:** Canvas, game loop, 2D array manipulation, collision detection, scoring system

### Task 289 — Platformer Game ⚫

- Side-scrolling platformer:
  - Player character (sprite animation)
  - Movement: left, right, jump
  - Gravity ও physics
  - Platforms (different types: normal, moving, breakable)
  - Enemies (basic AI — patrol ও chase)
  - Collectibles (coins, power-ups)
  - Level design (JSON-based levels)
  - Camera scrolling
  - Health system
  - Score ও lives
  - **Concepts used:** Canvas, game loop, physics engine, collision detection, sprite animation, state machine (for player states)

### Task 290 — Chess Game ⚫

- Two-player chess:
  - Board rendering (Canvas or DOM)
  - All piece movements (pawn, rook, knight, bishop, queen, king)
  - Special moves: castling, en passant, pawn promotion
  - Move validation (legal moves only)
  - Check ও checkmate detection
  - Stalemate detection
  - Move history (algebraic notation)
  - Undo move
  - Highlight possible moves
  - Captured pieces display
  - Turn indicator
  - **Concepts used:** 2D array, complex game logic, state management, algorithm (move validation)

### Task 291 — Card Game (Memory / Solitaire) ⚫

- Pick one:
  - **Memory Game:**
    - Grid of face-down cards
    - Flip two at a time
    - Match pairs
    - Move count ও timer
    - Difficulty levels (grid size)
    - Best scores
  - **OR Solitaire:**
    - 7 columns, draw pile
    - Drag & drop cards
    - Auto-complete when possible
    - Undo
    - Win detection
  - **Concepts used:** Canvas/DOM, shuffle algorithm (Fisher-Yates), drag & drop, animation, game state management

### Task 292 — Multiplayer Game (WebSocket) ⚫

- Real-time multiplayer:
  - WebSocket server (Node.js)
  - Game lobby (create/join rooms)
  - Pick a game: Tic-Tac-Toe, Connect Four, or Battleship
  - Real-time moves
  - Player turn management
  - Win/lose detection
  - Chat during game
  - Rematch option
  - Spectator mode
  - **Concepts used:** WebSocket, Node.js, game state sync, event-driven architecture, real-time communication

---

## Section D: Advanced Full-Stack Projects (Tasks 293-300)

### Task 293 — URL Shortener (Full Stack) ⚫

- Complete service:
  - Node.js server (your mini-Express)
  - Short URL generation (base62 encoding)
  - Redirect: `short.ly/abc` → `https://original-url.com/long-path`
  - Click tracking (count, timestamp, referrer, user agent)
  - Analytics dashboard
  - API: create, get stats, delete
  - Rate limiting
  - Custom short codes
  - QR code generation (Canvas)
  - **Concepts used:** Node.js, HTTP, hashing, database (SQLite), Canvas

### Task 294 — Blog Platform (Full Stack) ⚫

- Complete blog:
  - Backend: REST API (your framework from Phase 07)
  - Auth: register, login (JWT)
  - Posts: CRUD, markdown rendering
  - Comments: nested, CRUD
  - Tags ও categories
  - Search (full-text)
  - Pagination
  - SEO: server-side rendering
  - RSS feed
  - Admin dashboard
  - **Frontend:** SPA with your router
  - **Concepts used:** Everything from Phase 07 + frontend from Phase 06

### Task 295 — Real-Time Collaboration Tool ⚫

- Google Docs-lite:
  - WebSocket for real-time sync
  - Rich text editor (contenteditable + execCommand or custom)
  - Multiple cursors (show other users' cursors)
  - Operational Transform (OT) or CRDT for conflict resolution
  - User presence (who's online)
  - Document save (auto-save)
  - Document list/management
  - **This is the hardest project** — OT/CRDT alone is a major challenge
  - **Concepts used:** WebSocket, OT/CRDT algorithms, contenteditable, real-time sync

### Task 296 — API Gateway / Proxy (Backend) ⚫

- Microservice gateway:
  - Route requests to different backends
  - Authentication middleware
  - Rate limiting (per user, per endpoint)
  - Request/response logging
  - Circuit breaker (don't call failing services)
  - Caching layer
  - Load balancing (round-robin)
  - Health checks
  - API key management
  - Request transformation (add headers, modify body)
  - **Concepts used:** Node.js HTTP, streams, async patterns, design patterns

### Task 297 — Testing Framework (Build Your Own) ⚫

- Build a mini Jest/Mocha:

  ```js
  describe('Calculator', () => {
    it('should add numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    it('should handle errors', () => {
      expect(() => divide(1, 0)).toThrow('Division by zero');
    });
  });
  ```

  - Features:
    - `describe`, `it`, `test` blocks
    - `expect` with matchers: `toBe`, `toEqual`, `toThrow`, `toBeTruthy`, `toContain`, `toMatch`
    - `beforeEach`, `afterEach`, `beforeAll`, `afterAll`
    - Async test support
    - Test reporter (console output with colors)
    - Mock functions: `jest.fn()`, `jest.spyOn()`
    - Code coverage (basic — count executed lines)
    - CLI runner: `node test-runner tests/`
  - **Concepts used:** AST (for coverage), metaprogramming, CLI, reporter pattern

### Task 298 — Module Bundler (Build Your Own) ⚫

- Build a mini Webpack:

  ```bash
  node bundler.js --entry src/index.js --output dist/bundle.js
  ```

  - Features:
    - Parse `import/require` statements → build dependency graph
    - Bundle all modules into single file
    - Module wrapping (each module in its own scope)
    - Circular dependency handling
    - Source map generation (basic)
    - File watching (rebuild on change)
    - Minification (basic — remove whitespace, shorten variables)
  - **Concepts used:** AST parsing, graph algorithms, file system, CLI

### Task 299 — Static Site Generator ⚫

- Build your own 11ty/Hugo:

  ```bash
  node ssg.js build    # Generate static HTML
  node ssg.js serve    # Dev server with hot reload
  ```

  - Features:
    - Markdown → HTML (your parser from Task 274)
    - Template engine (your engine from Task 203)
    - Front matter parsing (YAML-like metadata)
    - Layouts ও partials
    - Asset pipeline (copy CSS, images)
    - Pagination
    - Tags ও categories
    - RSS feed generation
    - Dev server with live reload (WebSocket)
    - **Output:** Build your portfolio site with this tool

### Task 300 — JavaScript Runtime (Build Your Own) ⚫

- Phase 10 ও FINAL graduation — the ultimate project:
  - **Extend your mini language from Task 245:**
    - Add more features:
      - Arrays: `let arr = [1, 2, 3]`
      - Objects: `let obj = { name: "John" }`
      - For loops: `for (let i = 0; i < 10; i = i + 1) { ... }`
      - Standard library: `print()`, `len()`, `push()`, `pop()`, `type()`
    - **OR build a JS subset interpreter:**
      - Lexer: tokenize JS-like syntax
      - Parser: build AST
      - Interpreter: walk AST ও execute
      - Scope chain ও closures
      - Basic event loop simulation
    - REPL mode: interactive command line
    - File execution: `node myruntime.js script.js`
  - **This project alone proves you understand JS deeply — because you built a language runtime**

---

## 🏆 Phase 10 Completion = JS MASTERY

### What You've Built (50 Projects):

| #   | Project               | Key Concepts                          |
| --- | --------------------- | ------------------------------------- |
| 271 | To-Do App             | DOM, events, localStorage, drag&drop  |
| 272 | Chat App              | WebSocket, Node.js, real-time         |
| 273 | Kanban Board          | Drag&Drop, CRUD, custom events        |
| 274 | Markdown Editor       | Regex, parser, File API               |
| 275 | Drawing App           | Canvas, mouse/touch events            |
| 276 | Spreadsheet           | Formula parser, observer pattern      |
| 277 | Image Editor          | Canvas pixel manipulation             |
| 278 | Music Player          | Web Audio API, Canvas visualizer      |
| 279 | Project Dashboard     | Canvas charts, data modeling          |
| 280 | E-Commerce            | SPA routing, form validation          |
| 281 | Quiz Platform         | Timer, statistics, data structures    |
| 282 | Weather Dashboard     | Fetch, Geolocation, Canvas charts     |
| 283 | File Manager          | File System Access API                |
| 284 | Budget Tracker        | Intl API, Canvas charts, CSV          |
| 285 | Code Editor           | Tokenizer, syntax highlighting        |
| 286 | Terminal              | Command pattern, virtual FS           |
| 287 | Snake Game            | Game loop, Canvas, collision          |
| 288 | Tetris                | 2D arrays, rotation, physics          |
| 289 | Platformer            | Physics engine, sprite animation      |
| 290 | Chess                 | Complex algorithms, state management  |
| 291 | Card Game             | Shuffle, drag&drop, animation         |
| 292 | Multiplayer Game      | WebSocket, real-time sync             |
| 293 | URL Shortener         | Full-stack, hashing, analytics        |
| 294 | Blog Platform         | REST API, auth, SSR, SPA              |
| 295 | Collaboration Tool    | OT/CRDT, real-time, cursors           |
| 296 | API Gateway           | Proxy, circuit breaker, rate limit    |
| 297 | Testing Framework     | AST, metaprogramming, CLI             |
| 298 | Module Bundler        | AST, dependency graph, CLI            |
| 299 | Static Site Generator | Template engine, markdown, dev server |
| 300 | JS Runtime            | Lexer, parser, interpreter, REPL      |
| 301 | Virtual DOM Library   | Diffing, patching, reconciliation     |
| 302 | Reactive State System | Proxy, dependency tracking, effects   |
| 303 | Database Engine       | B-Tree, indexing, query parsing       |
| 304 | Operating System Sim  | Process scheduler, memory management  |
| 305 | Neural Network        | Matrix math, backpropagation          |
| 306 | Blockchain            | Hashing, proof-of-work, P2P           |
| 307 | Video Editor          | Canvas, MediaRecorder, timeline       |
| 308 | 3D Renderer           | Matrix transforms, projection, Canvas |
| 309 | Regex Engine          | NFA, DFA, state machines              |
| 310 | HTTP/2 Server         | Binary framing, multiplexing, HPACK   |
| 311 | Distributed KV Store  | Raft consensus, replication           |
| 312 | Git Clone             | SHA hashing, tree structures, diff    |
| 313 | Web Browser Engine    | HTML parser, CSS parser, layout       |
| 314 | Package Manager       | Dependency resolution, semver         |
| 315 | Search Engine         | Inverted index, TF-IDF, ranking       |
| 316 | WebAssembly Compiler  | IR, code gen, binary format           |
| 317 | Real-time OS Kernel   | Scheduling, interrupts, memory        |
| 318 | Peer-to-Peer Network  | DHT, NAT traversal, WebRTC            |
| 319 | Container Runtime     | Namespaces, cgroups simulation        |
| 320 | Full Language + IDE   | Compiler + editor + debugger          |

---

## Section E: God-Level Advanced Projects (Tasks 301-320)

> এই 20টা project industry-level engineering concepts শেখাবে। এগুলো complete করলে তুমি framework author, tool builder, system designer — সবকিছু।

### Task 301 — Virtual DOM Library ⚫

- Build React's core concept from scratch:
  - Virtual DOM tree creation: `createElement(tag, props, ...children)`
  - `render(vdom)` → real DOM
  - **Diffing algorithm:** compare old tree vs new tree
  - **Patching:** apply minimum changes to real DOM
  - Component system: function components with props
  - Event handling: synthetic event system
  - State: `useState`-like hook with re-rendering
  - Keyed reconciliation (list diffing)
  - Benchmark: compare your VDOM vs direct DOM manipulation
  - **Concepts used:** Tree data structure, diffing algorithms, DOM API, closures, recursion
  - **Why:** React এর heart হলো Virtual DOM — এটা বানালে React ভিতর থেকে বুঝবে

### Task 302 — Reactive State Management System ⚫

- Build Vue.js/MobX-style reactivity:
  - `reactive(obj)` — Proxy-based reactive objects
  - `computed(() => expr)` — auto-updating derived values
  - `effect(() => { ... })` — auto-re-run on dependency change
  - `watch(source, callback)` — watch specific values
  - Dependency tracking (track which effects depend on which properties)
  - Batch updates (don't re-run effects during batch)
  - Deep reactivity (nested objects & arrays)
  - `ref()` for primitive values
  - DevTools: log dependency graph
  - **Concepts used:** Proxy, WeakMap, dependency graph, observer pattern, topological sorting
  - **Why:** State management এর core concept — Redux, MobX, Zustand সব এটার variation

### Task 303 — Database Engine (SQLite-like) ⚫

- Build a complete database:
  - **Storage engine:** B-Tree for indexing
  - **Query parser:** parse SQL-like syntax:
    ```
    SELECT name, age FROM users WHERE age > 25 ORDER BY name
    INSERT INTO users (name, age) VALUES ('John', 30)
    CREATE TABLE users (id INT, name TEXT, age INT)
    ```
  - **Query executor:** execute parsed queries against data
  - **Indexes:** B-Tree index for fast lookups
  - **Persistence:** save to file system (Node.js)
  - **Transactions:** basic BEGIN, COMMIT, ROLLBACK
  - **CLI interface:** interactive SQL shell
  - **Concepts used:** B-Tree, parsing, file I/O, data serialization, indexing algorithms
  - **Why:** Database internally কীভাবে কাজ করে বুঝলে — efficient queries লিখতে পারবে

### Task 304 — Operating System Simulator ⚫

- Simulate OS concepts in JavaScript:
  - **Process scheduler:** Round Robin, Priority Queue, FCFS
  - **Memory management:** paging, page table, page faults
  - **File system:** inode-based, directory tree, file operations
  - **Process states:** new → ready → running → waiting → terminated
  - **Inter-process communication:** message passing, shared memory simulation
  - **Deadlock detection:** resource allocation graph
  - **Shell:** command-line interface for the simulated OS
  - **Visualization:** Canvas-based animation of scheduler, memory
  - **Concepts used:** Queues, priority queues, graphs, state machines, scheduling algorithms
  - **Why:** OS concepts বুঝলে Node.js event loop, memory, processes — সব crystal clear

### Task 305 — Neural Network from Scratch ⚫

- Build ML without libraries:
  - **Matrix class:** add, multiply, transpose, element-wise operations
  - **Layers:** Dense (fully connected) layer
  - **Activation functions:** sigmoid, tanh, ReLU, softmax
  - **Forward pass:** input → layers → output
  - **Loss functions:** MSE, cross-entropy
  - **Backpropagation:** calculate gradients, update weights
  - **Training loop:** epochs, batches, learning rate
  - **Demo:** Train on XOR problem, digit recognition (basic)
  - **Visualization:** Canvas-based network visualization, loss curve
  - **Concepts used:** Linear algebra, calculus (derivatives), optimization, TypedArrays for performance
  - **Why:** AI/ML fundamentally কী করে — না বুঝে AI tool use করলে তুমি AI dependent, না AI augmented

### Task 306 — Blockchain from Scratch ⚫

- Build cryptocurrency concepts:
  - **Block:** data, hash, previousHash, timestamp, nonce
  - **Blockchain:** chain of blocks, validation
  - **Proof of Work:** mining (find nonce where hash starts with 0000...)
  - **Transaction system:** send, receive, balance
  - **Wallet:** public/private key pair (Node.js crypto)
  - **P2P Network:** WebSocket-based node communication
  - **Consensus:** longest chain wins
  - **Mining reward:** new coins for miners
  - **Transaction pool:** pending transactions
  - **Explorer:** web UI to view blocks & transactions
  - **Concepts used:** Cryptographic hashing, public-key crypto, P2P networking, consensus algorithms
  - **Why:** Distributed systems, cryptography, consensus — advanced CS concepts hands-on

### Task 307 — Video Editor (Browser-based) ⚫

- Canvas + MediaRecorder based:
  - **Timeline:** visual track with clips
  - **Import videos:** File API + video element
  - **Trim/Cut:** select portion of video
  - **Merge:** concatenate multiple clips
  - **Transitions:** fade, dissolve between clips (Canvas)
  - **Text overlay:** add text at specific timestamps
  - **Audio track:** separate audio control
  - **Filters:** brightness, contrast, saturation (Canvas pixel manipulation)
  - **Export:** MediaRecorder API → save as WebM/MP4
  - **Undo/redo:** command pattern for all operations
  - **Concepts used:** Canvas, Video API, MediaRecorder, Web Audio, File/Blob API, requestAnimationFrame
  - **Why:** Media processing পুরোটা browser API দিয়ে — multimedia mastery

### Task 308 — 3D Renderer (Software Rendering) ⚫

- Build a 3D rendering engine without WebGL:
  - **3D Math:** Vector3, Matrix4x4, quaternion rotation
  - **Camera:** perspective projection (3D → 2D)
  - **Wireframe rendering:** draw edges on Canvas
  - **Solid rendering:** painter's algorithm or z-buffer
  - **Lighting:** ambient, diffuse, specular (Phong model)
  - **Objects:** cube, sphere, custom meshes
  - **Transformations:** translate, rotate, scale
  - **OBJ file parser:** load 3D models
  - **User interaction:** rotate/zoom with mouse
  - **Animation:** rotate, orbit, morphing
  - **Concepts used:** Linear algebra, projection math, rendering pipeline, Canvas 2D
  - **Why:** 3D math ও rendering pipeline বুঝলে — WebGL, Three.js, game dev সব সহজ

### Task 309 — Regular Expression Engine ⚫

- Build regex from scratch:
  - **Parser:** parse regex pattern → AST
  - **NFA construction:** Thompson's construction algorithm
  - **NFA → DFA conversion:** subset construction
  - **Matcher:** run input string through DFA
  - Supported features:
    - Literals: `a`, `b`
    - Concatenation: `ab`
    - Alternation: `a|b`
    - Quantifiers: `*`, `+`, `?`, `{n,m}`
    - Character classes: `[a-z]`, `[^0-9]`
    - Anchors: `^`, `$`
    - Groups: `(abc)`, capture groups
    - Escape: `\.`, `\d`, `\w`, `\s`
  - **Visualization:** draw NFA/DFA state diagram (Canvas)
  - **Concepts used:** Finite automata theory, graph algorithms, parsing, state machines
  - **Why:** Regex internally কীভাবে কাজ করে — performance implications বুঝবে

### Task 310 — HTTP/2 Server from Scratch ⚫

- Build HTTP/2 protocol:
  - **Frame parser:** parse HTTP/2 binary frames
  - **HPACK:** header compression algorithm
  - **Multiplexing:** multiple streams over single connection
  - **Stream management:** stream states, flow control
  - **Server push:** push resources to client
  - **TLS integration:** ALPN negotiation (using Node.js tls module)
  - **Settings frame:** SETTINGS, WINDOW_UPDATE
  - **Error handling:** RST_STREAM, GOAWAY
  - Compare performance with HTTP/1.1 server
  - **Concepts used:** Binary protocols, state machines, compression, networking
  - **Why:** HTTP/2 protocol level বুঝলে — web performance deeply বুঝবে

### Task 311 — Distributed Key-Value Store ⚫

- Build a distributed database:
  - **Single node:** in-memory key-value store with persistence
  - **Multiple nodes:** TCP/WebSocket communication between nodes
  - **Replication:** write to leader, replicate to followers
  - **Consensus:** basic Raft algorithm implementation:
    - Leader election
    - Log replication
    - Safety guarantees
  - **Partitioning:** hash-based key distribution
  - **Failure handling:** node failure detection, re-election
  - **Client library:** `get(key)`, `set(key, value)`, `delete(key)`
  - **Consistency model:** eventual consistency vs strong consistency
  - **Concepts used:** Distributed systems, consensus algorithms, networking, replication
  - **Why:** Distributed system design — system design interview এর #1 topic

### Task 312 — Git Clone (Version Control) ⚫

- Build Git core concepts:
  - **Object model:** blob (file), tree (directory), commit
  - **SHA-1 hashing:** content-addressable storage
  - **init:** create repository structure (`.mygit/`)
  - **add:** stage files (create blobs, update index)
  - **commit:** create commit object with tree and parent
  - **log:** traverse commit history
  - **diff:** compare files/commits (LCS diff algorithm)
  - **branch:** create/list/delete branches (just pointers to commits)
  - **checkout:** switch branches, restore files
  - **merge:** three-way merge (basic)
  - **CLI interface:** `mygit init`, `mygit add .`, `mygit commit -m "msg"`
  - **Concepts used:** Hashing, tree structures, graph traversal, diff algorithms, file I/O
  - **Why:** Version control internally কীভাবে কাজ করে — Git mastery

### Task 313 — Web Browser Rendering Engine (Simplified) ⚫

- Build a mini browser engine:
  - **HTML Parser:** tokenize HTML → parse → DOM tree
  - **CSS Parser:** parse CSS → stylesheet rules
  - **Style computation:** match CSS selectors to DOM nodes, compute styles
  - **Layout engine:** box model, block layout, inline layout
    - Calculate width, height, x, y for each box
  - **Paint:** render layout boxes to Canvas
  - **Render pipeline:** DOM + CSSOM → Render Tree → Layout → Paint
  - **Demo:** render simple HTML+CSS pages
  - **Concepts used:** Parsers, tree structures, CSS specificity, layout algorithms
  - **Why:** Browser internally কীভাবে HTML render করে — performance optimization deeply বুঝবে

### Task 314 — Package Manager (npm clone) ⚫

- Build a dependency manager:
  - **Registry:** simple HTTP server with package metadata
  - **install:** resolve dependencies, download, extract
  - **Dependency resolution:** semver matching, conflict resolution
  - **Dependency tree:** flatten (hoisting) vs nested
  - **Lock file:** deterministic installs
  - **publish:** upload package to registry
  - **Scripts:** `"scripts": { "start": "node index.js" }` execution
  - **Cache:** downloaded packages local cache
  - **CLI:** `mypkg init`, `mypkg install lodash`, `mypkg publish`
  - **Concepts used:** Graph algorithms, semver, HTTP, file I/O, CLI, tar/gzip
  - **Why:** npm/yarn/pnpm internally কীভাবে কাজ করে

### Task 315 — Search Engine (Full-Text) ⚫

- Build Google-lite:
  - **Crawler:** fetch web pages (HTTP), extract text and links
  - **Indexer:** inverted index (word → document list)
  - **Tokenizer:** split text into tokens, normalize (lowercase, stemming)
  - **TF-IDF scoring:** term frequency × inverse document frequency
  - **Query parser:** parse search queries, boolean operators (AND, OR, NOT)
  - **Ranking:** sort results by relevance score
  - **PageRank:** basic link analysis (simplified)
  - **Web UI:** search bar, results page, pagination
  - **Spelling correction:** edit distance (Levenshtein)
  - **Concepts used:** Inverted index, TF-IDF, graph algorithms, text processing, HTTP crawling
  - **Why:** Information retrieval fundamentals — search is everywhere

### Task 316 — WebAssembly Compiler (Basic) ⚫

- Compile a simple language to WASM:
  - **Source language:** simple math expressions & functions
    ```
    fn add(a: i32, b: i32) -> i32 { return a + b; }
    fn factorial(n: i32) -> i32 { if n <= 1 { return 1; } return n * factorial(n - 1); }
    ```
  - **Lexer:** tokenize source code
  - **Parser:** parse to AST
  - **Type checker:** verify types match
  - **Code generator:** emit WASM binary format
  - **WASM sections:** type, function, code, export
  - **LEB128 encoding:** variable-length integer encoding
  - **Loader:** load .wasm file in browser, call exported functions
  - **Concepts used:** Compiler design, binary encoding, type systems, WASM spec
  - **Why:** WebAssembly কীভাবে কাজ করে — next-gen web performance

### Task 317 — Real-Time Operating System Kernel (Simulation) ⚫

- Simulate an RTOS in JavaScript:
  - **Task scheduler:** preemptive priority-based scheduling
  - **Timer system:** tick-based time management
  - **Interrupt handling:** simulated hardware interrupts
  - **Semaphores & Mutexes:** synchronization primitives
  - **Memory allocator:** first-fit, best-fit, buddy system
  - **Message queues:** inter-task communication
  - **Shell:** command interface for the kernel
  - **Task creation/deletion:** dynamic task management
  - **Visualization:** Canvas-based timeline of task execution
  - **Concepts used:** Scheduling algorithms, synchronization, memory management, state machines
  - **Why:** Low-level systems understanding — helps with Node.js event loop, performance

### Task 318 — Peer-to-Peer Network ⚫

- Build decentralized networking:
  - **WebRTC data channels:** browser-to-browser communication
  - **Signaling server:** WebSocket-based connection establishment
  - **Distributed Hash Table (DHT):** Kademlia-like routing
  - **NAT traversal:** STUN/TURN concepts
  - **Peer discovery:** find and connect to peers
  - **File sharing:** split files into chunks, distribute
  - **Message routing:** multi-hop message delivery
  - **Gossip protocol:** spread information across network
  - **Resilience:** handle peer disconnection gracefully
  - **UI:** peer list, file sharing interface, chat
  - **Concepts used:** WebRTC, distributed algorithms, networking, binary data
  - **Why:** Decentralized architecture — beyond client-server thinking

### Task 319 — Container Runtime Simulator ⚫

- Simulate Docker concepts:
  - **Image:** layered file system (copy-on-write simulation)
  - **Container:** isolated execution environment
  - **Process isolation:** simulated namespace (separate variable scopes)
  - **Resource limits:** simulated cgroups (CPU time, memory limits)
  - **Networking:** virtual network between containers
  - **Volume mounts:** shared data between containers
  - **Dockerfile parser:** parse and execute build steps
  - **Container lifecycle:** create, start, stop, remove
  - **Registry:** push/pull images
  - **CLI:** `mycontainer run`, `mycontainer build`, `mycontainer ps`
  - **Concepts used:** File system abstraction, process management, networking, CLI
  - **Why:** Container orchestration internally কীভাবে কাজ করে — DevOps mastery

### Task 320 — Complete Language + IDE (Final Boss) ⚫

- The ultimate capstone — build everything:
  - **Programming language:**
    - Variables, functions, closures
    - If/else, loops (for, while)
    - Arrays, objects, strings
    - First-class functions
    - Error handling (try/catch)
    - Standard library (print, math, string, array methods)
    - Module system (import/export)
  - **Compiler/Interpreter:**
    - Lexer → Parser → AST → Interpreter (or bytecode compiler)
  - **IDE (browser-based):**
    - Code editor with syntax highlighting for YOUR language
    - Auto-complete (basic)
    - Error highlighting (inline)
    - Console output panel
    - File management
    - Debugger (breakpoints, step through, variable inspection)
  - **Documentation:** language spec, tutorial, examples
  - **Concepts used:** Everything from all phases — this is the graduation project
  - **Why:** তুমি একটা programming language + IDE build করেছো — তুমি আর কারো থেকে replaceable না

---

## ✅ FINAL Completion Checklist

- [ ] All 50 projects complete ও deployed (where applicable)
- [ ] Each project has clean, well-structured code
- [ ] Each project handles errors gracefully
- [ ] Each project is responsive (where applicable)
- [ ] Portfolio website built with your own tools (Task 299)
- [ ] At least 15 projects on GitHub with detailed README
- [ ] Can explain any project's architecture ও design decisions
- [ ] Can rebuild any project from scratch without reference
- [ ] At least 5 projects demonstrate "built from scratch" concepts (VDOM, DB, Compiler, etc.)

---

## 🎓 CONGRATULATIONS — You Are Now a JS God!

তুমি যদি 50টা project complete করো, তাহলে তুমি:

1. **JS Engine** থেকে **AST** পর্যন্ত জানো
2. **Every data structure** নিজে implement করতে পারো
3. **Functional Programming** ও **OOP** — দুটোই master
4. **Async** এর প্রতিটা pattern নিজে build করেছো
5. **DOM** framework ছাড়াই manipulate করতে পারো
6. **Node.js** দিয়ে complete backend build করতে পারো
7. **Metaprogramming** দিয়ে language-level features build করেছো
8. **Performance** optimize ও **Security** audit করতে পারো
9. **50টা real project** pure JS দিয়ে build করেছো
10. **নিজের programming language + IDE** build করেছো
11. **Virtual DOM, Database, Regex Engine, Browser Engine** — সব নিজে build করেছো
12. **Distributed Systems, Blockchain, Neural Network** — advanced CS concepts হাতে কলমে শিখেছো

> **তুমি আর শুধু developer না — তুমি একজন JavaScript Engineer।** 🏆
> **Framework তোমাকে ভয় দেখাতে পারবে না — কারণ তুমি framework BUILD করতে পারো।**
