# Backend Phase 02 — Operating System & Linux for Backend Devs (Tasks 36-65)

> **99% backend server Linux এ চলে। Linux না জানলে তুমি production এ survive করতে পারবে না।**
> **OS concepts বুঝলে তুমি বুঝবে — কেন server crash করে, memory leak হয়, process hang করে।**
> **Senior devs Linux commands জানে — Boss-level devs OS internals জানে।**

---

## Section A: Operating System Fundamentals (Tasks 36-45)

### Task 36 — Process & Thread Fundamentals 🟡

- OS কিভাবে program চালায়:
  - **Process:** running program (own memory space, PID)
  - **Thread:** lightweight process (shares memory with parent process)

  ```
  Process (PID: 1234)
  ├── Thread 1 (Main thread)
  ├── Thread 2 (Worker)
  ├── Thread 3 (Worker)
  └── Thread 4 (I/O)

  Each process has:
  ├── Code segment (program instructions)
  ├── Data segment (global variables)
  ├── Heap (dynamic memory — malloc/new)
  ├── Stack (function calls — per thread)
  └── File descriptors (open files, sockets)
  ```

  - **Process vs Thread:**
    ```
    Feature          Process              Thread
    ──────────────── ──────────────────── ────────────────────
    Memory           Separate             Shared (within process)
    Creation cost    High                 Low
    Communication    IPC (pipes, sockets) Shared memory
    Crash impact     Only self            Entire process
    Context switch   Expensive            Cheaper
    ```
  - **Process States:**
    ```
    New → Ready → Running → Waiting → Ready → Running → Terminated
                     ↓                                      ↑
                  Blocked (I/O wait) ──────────────────────→
    ```
  - **Exercise (Node.js):**

    ```js
    const { fork, exec } = require('child_process');
    const { Worker, isMainThread, parentPort } = require('worker_threads');

    // 1. Fork a child process
    if (process.argv[2] === 'child') {
      console.log(`Child PID: ${process.pid}`);
      process.send({ result: 'done' });
    } else {
      const child = fork(__filename, ['child']);
      child.on('message', (msg) => console.log('From child:', msg));
      console.log(`Parent PID: ${process.pid}`);
    }

    // 2. Worker thread
    if (isMainThread) {
      const worker = new Worker(__filename);
      worker.on('message', (msg) => console.log('From worker:', msg));
    } else {
      // Heavy computation in worker
      let sum = 0;
      for (let i = 0; i < 1e9; i++) sum += i;
      parentPort.postMessage({ sum });
    }
    ```

    1. Process tree দেখো: `ps aux --forest` (Linux) / Task Manager (Windows)
    2. Child process fork করো ও message passing করো
    3. Worker thread দিয়ে CPU-intensive task করো
    4. Compare: child_process vs worker_threads performance

### Task 37 — Memory Management 🟡

- OS কিভাবে memory manage করে:
  - **Virtual Memory:** প্রতিটা process মনে করে সে পুরো RAM পাচ্ছে
  - **Memory Layout:**
    ```
    High Address
    ┌─────────────────┐
    │ Stack            │ ← Function calls, local vars (grows down)
    │ (grows ↓)       │
    ├─────────────────┤
    │                  │ ← Free space
    ├─────────────────┤
    │ Heap             │ ← Dynamic allocation (grows up)
    │ (grows ↑)       │
    ├─────────────────┤
    │ BSS              │ ← Uninitialized globals
    ├─────────────────┤
    │ Data             │ ← Initialized globals
    ├─────────────────┤
    │ Text (Code)      │ ← Program instructions
    └─────────────────┘
    Low Address
    ```
  - **Page Table:** virtual address → physical address mapping
  - **Page Fault:** requested page not in RAM → load from disk (slow!)
  - **OOM Killer:** Linux kills process when RAM runs out
  - **Node.js Memory:**

    ```js
    // Check memory usage
    console.log(process.memoryUsage());
    // {
    //   rss: 30MB,        — Resident Set Size (total RAM used)
    //   heapTotal: 7MB,   — V8 heap allocated
    //   heapUsed: 5MB,    — V8 heap used
    //   external: 1MB,    — C++ objects linked to JS
    //   arrayBuffers: 0   — ArrayBuffer memory
    // }

    // Memory leak example
    const leaks = [];
    setInterval(() => {
      leaks.push(Buffer.alloc(1024 * 1024)); // 1MB every interval
      console.log(`Heap: ${(process.memoryUsage().heapUsed / 1024 / 1024).toFixed(2)} MB`);
    }, 100);
    ```

  - **Exercise:**
    1. Memory usage monitor build করো (real-time)
    2. Memory leak detect করো ও fix করো
    3. `--max-old-space-size` দিয়ে V8 heap limit test করো

### Task 38 — File System & I/O 🟡

- How OS handles files:
  - **File Descriptors:** integer that represents open file
    - 0 = stdin, 1 = stdout, 2 = stderr, 3+ = opened files/sockets
  - **I/O Models:**
    ```
    Blocking I/O:      Thread waits until I/O completes
    Non-blocking I/O:  Returns immediately (EAGAIN if not ready)
    I/O Multiplexing:  Monitor multiple FDs (select, poll, epoll, kqueue)
    Async I/O:         OS notifies when I/O complete (io_uring in Linux)
    ```
  - **epoll (Linux) / kqueue (macOS):** How Node.js handles thousands of connections
    ```
    Traditional: 1 thread per connection (10,000 connections = 10,000 threads = 💀)
    epoll/kqueue: 1 thread monitors all connections (10,000 connections = 1 thread = ✅)
    ```
  - **Exercise:**

    ```js
    const fs = require('fs');

    // 1. File descriptors
    const fd = fs.openSync('test.txt', 'w');
    console.log('File descriptor:', fd);
    fs.writeSync(fd, 'Hello World');
    fs.closeSync(fd);

    // 2. Blocking vs Non-blocking
    // Blocking
    const data = fs.readFileSync('big-file.txt'); // Thread blocked!

    // Non-blocking
    fs.readFile('big-file.txt', (err, data) => {
      // Thread free to do other work!
    });

    // 3. Stream (memory efficient)
    const readStream = fs.createReadStream('huge-file.txt', { highWaterMark: 64 * 1024 });
    let lines = 0;
    readStream.on('data', (chunk) => {
      lines += chunk.toString().split('\n').length;
    });
    readStream.on('end', () => console.log(`${lines} lines`));
    ```

    1. File descriptor limit check করো: `ulimit -n`
    2. Large file read করো blocking vs streaming — memory compare করো
    3. Watch file changes: `fs.watch` implement করো

### Task 39 — Concurrency vs Parallelism 🟡

- Key distinction:

  ```
  Concurrency: Multiple tasks PROGRESS together (maybe on 1 CPU)
  ┌─────────────────────────────────────────┐
  │ Task A ██░░██░░░░██                      │
  │ Task B ░░██░░██░░░░██                    │  1 CPU, context switching
  │ Task C ░░░░░░░░██░░░░██                  │
  └─────────────────────────────────────────┘

  Parallelism: Multiple tasks RUN simultaneously (multiple CPUs)
  ┌─────────────────────────────────────────┐
  │ CPU 1: Task A ██████████████             │
  │ CPU 2: Task B ██████████████             │  Multiple CPUs
  │ CPU 3: Task C ██████████████             │
  └─────────────────────────────────────────┘
  ```

  - **Node.js = Concurrent (single-threaded event loop)**
  - **Node.js + Worker Threads = Parallel**
  - **Race Conditions:**

    ```js
    // Race condition example
    let balance = 100;

    async function withdraw(amount) {
      const current = balance; // Read
      await delay(10); // Simulate async work
      balance = current - amount; // Write (based on stale read!)
    }

    // Both read balance=100, both write their result
    await Promise.all([withdraw(30), withdraw(50)]);
    console.log(balance); // Expected: 20, Actual: 50 or 70 (RACE CONDITION!)

    // Fix: use a mutex/lock
    class Mutex {
      #queue = [];
      #locked = false;

      async acquire() {
        if (this.#locked) {
          await new Promise((resolve) => this.#queue.push(resolve));
        }
        this.#locked = true;
      }

      release() {
        if (this.#queue.length > 0) {
          this.#queue.shift()();
        } else {
          this.#locked = false;
        }
      }
    }

    const mutex = new Mutex();
    async function safeWithdraw(amount) {
      await mutex.acquire();
      try {
        const current = balance;
        await delay(10);
        balance = current - amount;
      } finally {
        mutex.release();
      }
    }
    ```

  - **Exercise:**
    1. Race condition reproduce করো ও fix করো
    2. Mutex, Semaphore implement করো
    3. Deadlock scenario create করো ও understand করো

### Task 40 — Signals & Inter-Process Communication (IPC) 🟡

- Process communication:
  - **Signals:** OS → Process notifications
    ```
    SIGTERM (15) — Graceful shutdown request
    SIGKILL (9)  — Force kill (can't catch!)
    SIGINT (2)   — Ctrl+C (interrupt)
    SIGHUP (1)   — Terminal closed
    SIGUSR1      — Custom signal (Node.js: start debugger)
    SIGUSR2      — Custom signal
    ```
  - **Graceful Shutdown:**

    ```js
    const http = require('http');
    const server = http.createServer(/* ... */);

    // Track active connections
    const connections = new Set();
    server.on('connection', (conn) => {
      connections.add(conn);
      conn.on('close', () => connections.delete(conn));
    });

    function gracefulShutdown(signal) {
      console.log(`\n${signal} received. Starting graceful shutdown...`);

      // 1. Stop accepting new connections
      server.close(() => {
        console.log('Server closed. No new connections.');
        // 3. Close database connections
        // 4. Flush logs
        // 5. Exit
        process.exit(0);
      });

      // 2. Close existing connections (after current request finishes)
      connections.forEach((conn) => conn.end());

      // Force shutdown after 10 seconds
      setTimeout(() => {
        console.error('Forced shutdown after timeout');
        process.exit(1);
      }, 10000);
    }

    process.on('SIGTERM', () => gracefulShutdown('SIGTERM'));
    process.on('SIGINT', () => gracefulShutdown('SIGINT'));

    // Uncaught exceptions
    process.on('uncaughtException', (err) => {
      console.error('Uncaught Exception:', err);
      gracefulShutdown('uncaughtException');
    });

    process.on('unhandledRejection', (reason) => {
      console.error('Unhandled Rejection:', reason);
      gracefulShutdown('unhandledRejection');
    });
    ```

  - **IPC Methods:**
    ```
    Pipes          — Unidirectional (stdin/stdout)
    Named Pipes    — Filesystem-based
    Message Queues — OS-level queuing
    Shared Memory  — Fastest (direct memory access)
    Sockets        — Network or Unix domain sockets
    Signals        — Simple notifications
    ```
  - **Exercise:**
    1. Graceful shutdown implement করো (stop server, close DB, flush logs)
    2. Parent-child process communication (fork + message passing)
    3. Unix domain socket দিয়ে IPC implement করো

### Task 41 — File Permissions & User Management 🟢

- Linux file permissions:

  ```
  $ ls -la
  -rwxr-xr-- 1 user group 4096 Apr 16 10:00 script.js

  Type  Owner  Group  Others
  -     rwx    r-x    r--

  r = read (4)   w = write (2)   x = execute (1)

  rwxr-xr-- = 754
  Owner: 7 (4+2+1 = rwx)
  Group: 5 (4+0+1 = r-x)
  Others: 4 (4+0+0 = r--)
  ```

  - **Commands:**
    ```bash
    chmod 755 script.js     # Set permissions
    chmod +x script.js      # Add execute
    chown user:group file   # Change owner
    whoami                  # Current user
    id                      # User details
    sudo <command>          # Run as root
    ```
  - **Exercise:**
    1. File permissions experiment করো
    2. Non-root user দিয়ে Node.js server চালাও (security best practice)
    3. Understand: why Node.js shouldn't run as root

### Task 42 — Environment Variables & Configuration 🟢

- Application configuration:

  ```bash
  # Set env vars
  export DATABASE_URL="postgresql://user:pass@localhost:5432/mydb"
  export NODE_ENV="production"
  export PORT=3000
  export JWT_SECRET="super-secret-key"

  # In Node.js
  # process.env.DATABASE_URL
  ```

  - **12-Factor App Config:**
    - Store config in environment variables
    - Never hardcode secrets in code
    - Different config per environment (dev, staging, prod)
  - **.env file (development only):**

    ```js
    // .env loader (build your own dotenv)
    const fs = require('fs');
    const path = require('path');

    function loadEnv(filePath = '.env') {
      const envPath = path.resolve(process.cwd(), filePath);
      if (!fs.existsSync(envPath)) return;

      const content = fs.readFileSync(envPath, 'utf-8');
      content.split('\n').forEach((line) => {
        line = line.trim();
        if (!line || line.startsWith('#')) return; // Skip empty & comments

        const [key, ...valueParts] = line.split('=');
        let value = valueParts.join('=').trim();

        // Remove quotes
        if (
          (value.startsWith('"') && value.endsWith('"')) ||
          (value.startsWith("'") && value.endsWith("'"))
        ) {
          value = value.slice(1, -1);
        }

        if (!process.env[key]) {
          // Don't override existing
          process.env[key] = value;
        }
      });
    }

    loadEnv();
    ```

  - **NEVER commit `.env` to git!** Add to `.gitignore`
  - **Exercise:**
    1. dotenv library নিজে build করো (above code improve করো)
    2. Config validation: required vars check করো at startup
    3. Different configs for dev/staging/prod

### Task 43 — Networking from OS Perspective 🟡

- How OS handles network:
  - **Socket = File Descriptor** (in Linux, everything is a file!)
  - **Connection states:**
    ```
    LISTEN      — Server waiting for connections
    ESTABLISHED — Active connection
    TIME_WAIT   — Connection closed, waiting for cleanup
    CLOSE_WAIT  — Remote closed, waiting for local close
    FIN_WAIT_1  — Local initiated close
    FIN_WAIT_2  — Remote acknowledged close
    ```
  - **Check connections:**
    ```bash
    # Active connections
    ss -tuln           # or netstat -tuln
    # Which process uses port 3000?
    lsof -i :3000      # Linux/macOS
    # TCP connection states
    ss -s               # Summary
    # Time wait connections (too many = problem)
    ss -o state time-wait | wc -l
    ```
  - **Backlog queue:** pending connections before `accept()`
    ```js
    // server.listen(port, backlog)
    server.listen(3000, 511); // 511 = default backlog in Node.js
    // If queue full → connection refused/dropped
    ```
  - **Exercise:**
    1. Server চালিয়ে `ss`/`netstat` দিয়ে connections দেখো
    2. TIME_WAIT connections monitor করো
    3. Backlog full করে connection refused reproduce করো

### Task 44 — Cron Jobs & Scheduled Tasks 🟢

- Time-based task scheduling:

  ```bash
  # Cron expression format
  # ┌───── minute (0-59)
  # │ ┌───── hour (0-23)
  # │ │ ┌───── day of month (1-31)
  # │ │ │ ┌───── month (1-12)
  # │ │ │ │ ┌───── day of week (0-7, 0 & 7 = Sunday)
  # │ │ │ │ │
  # * * * * *

  # Examples:
  0 * * * *      — Every hour
  */5 * * * *    — Every 5 minutes
  0 0 * * *      — Midnight daily
  0 0 * * 0      — Midnight every Sunday
  0 9 1 * *      — 9 AM on 1st of every month
  ```

  - **Node.js Scheduler:**

    ```js
    class CronScheduler {
      #jobs = [];

      schedule(expression, callback) {
        const parsed = this.#parse(expression);
        this.#jobs.push({ parsed, callback });
      }

      #parse(expr) {
        const [minute, hour, dayOfMonth, month, dayOfWeek] = expr.split(' ');
        return { minute, hour, dayOfMonth, month, dayOfWeek };
      }

      #matches(parsed, date) {
        return (
          this.#fieldMatches(parsed.minute, date.getMinutes()) &&
          this.#fieldMatches(parsed.hour, date.getHours()) &&
          this.#fieldMatches(parsed.dayOfMonth, date.getDate()) &&
          this.#fieldMatches(parsed.month, date.getMonth() + 1) &&
          this.#fieldMatches(parsed.dayOfWeek, date.getDay())
        );
      }

      #fieldMatches(field, value) {
        if (field === '*') return true;
        if (field.includes('/')) {
          const step = parseInt(field.split('/')[1]);
          return value % step === 0;
        }
        return parseInt(field) === value;
      }

      start() {
        setInterval(() => {
          const now = new Date();
          this.#jobs.forEach((job) => {
            if (this.#matches(job.parsed, now)) {
              job.callback();
            }
          });
        }, 60000); // Check every minute
      }
    }

    const cron = new CronScheduler();
    cron.schedule('*/5 * * * *', () => console.log('Every 5 minutes'));
    cron.schedule('0 0 * * *', () => console.log('Midnight daily'));
    cron.start();
    ```

  - **Exercise:**
    1. Cron scheduler library build করো
    2. Database backup scheduler
    3. Log rotation scheduler

### Task 45 — System Resource Monitoring 🟡

- Server health monitoring:

  ```js
  const os = require('os');

  function getSystemInfo() {
    return {
      platform: os.platform(),
      arch: os.arch(),
      hostname: os.hostname(),
      uptime: `${(os.uptime() / 3600).toFixed(1)} hours`,
      cpu: {
        model: os.cpus()[0].model,
        cores: os.cpus().length,
        usage: getCpuUsage(),
      },
      memory: {
        total: `${(os.totalmem() / 1024 / 1024 / 1024).toFixed(1)} GB`,
        free: `${(os.freemem() / 1024 / 1024 / 1024).toFixed(1)} GB`,
        used: `${((os.totalmem() - os.freemem()) / 1024 / 1024 / 1024).toFixed(1)} GB`,
        usagePercent: `${((1 - os.freemem() / os.totalmem()) * 100).toFixed(1)}%`,
      },
      loadAvg: os.loadavg(), // [1min, 5min, 15min]
      network: os.networkInterfaces(),
    };
  }

  function getCpuUsage() {
    const cpus = os.cpus();
    let totalIdle = 0,
      totalTick = 0;
    cpus.forEach((cpu) => {
      const times = cpu.times;
      totalTick += times.user + times.nice + times.sys + times.idle + times.irq;
      totalIdle += times.idle;
    });
    return `${((1 - totalIdle / totalTick) * 100).toFixed(1)}%`;
  }
  ```

  - **Exercise:**
    1. System dashboard build করো (CPU, RAM, disk, network)
    2. Alert system: CPU > 80% → send notification
    3. Health check endpoint: `/health`
       ```js
       app.get('/health', (req, res) => {
         const health = {
           status: 'ok',
           uptime: process.uptime(),
           timestamp: Date.now(),
           memory: process.memoryUsage(),
           cpu: process.cpuUsage(),
         };
         res.json(health);
       });
       ```

---

## Section B: Linux Commands & Shell Scripting (Tasks 46-55)

### Task 46 — Essential Linux Commands 🟢

- প্রতিদিন use হবে:

  ```bash
  # Navigation
  pwd                    # Current directory
  ls -la                 # List files (detailed)
  cd /var/log            # Change directory
  mkdir -p src/config    # Create nested dirs

  # File operations
  cp file.txt backup/    # Copy
  mv old.txt new.txt     # Move/rename
  rm -rf build/          # Remove (CAREFUL!)
  cat file.txt           # View file
  less file.txt          # Paginated view
  head -n 20 file.txt    # First 20 lines
  tail -n 50 file.txt    # Last 50 lines
  tail -f /var/log/app.log  # Live log watching

  # Search
  find . -name "*.js" -type f          # Find files
  find . -name "*.log" -mtime +7       # Files older than 7 days
  grep -r "TODO" src/                  # Search in files
  grep -rn "error" --include="*.js"    # With line numbers

  # Disk
  df -h                  # Disk usage
  du -sh node_modules/   # Directory size
  ncdu /var/log          # Interactive disk usage

  # Process
  ps aux                 # All processes
  top / htop             # Real-time process monitor
  kill -15 <PID>         # Graceful kill
  kill -9 <PID>          # Force kill
  pkill -f "node index"  # Kill by name

  # Network
  curl http://localhost:3000/api/health
  wget https://example.com/file.zip
  ss -tuln               # Open ports
  lsof -i :3000          # What's using port 3000
  ping example.com
  traceroute example.com
  dig example.com
  ```

  - **Exercise:** প্রতিটা command 5 বার practice করো

### Task 47 — Pipe, Redirect & Text Processing 🟡

- Data flow:

  ```bash
  # Pipe: output of cmd1 → input of cmd2
  cat access.log | grep "ERROR" | wc -l     # Count errors
  ps aux | grep node | awk '{print $2}'     # PIDs of node processes

  # Redirect
  echo "Hello" > file.txt    # Overwrite
  echo "World" >> file.txt   # Append
  node app.js 2> error.log   # Redirect stderr
  node app.js > out.log 2>&1 # Both stdout & stderr

  # Text processing
  # awk — column extraction
  cat /etc/passwd | awk -F: '{print $1, $3}'   # Username & UID

  # sed — stream editor
  sed -i 's/old/new/g' file.txt    # Replace in file
  sed -n '10,20p' file.txt          # Print lines 10-20

  # sort & uniq
  cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
  # ^ Top 10 IP addresses by request count

  # cut
  echo "name:age:city" | cut -d: -f2    # age

  # tr
  echo "HELLO" | tr 'A-Z' 'a-z'        # hello

  # xargs
  find . -name "*.log" | xargs rm       # Delete all .log files
  ```

  - **Exercise:**
    1. Log file analyze করো (top IPs, error count, request/minute)
    2. CSV processing করো command line দিয়ে
    3. System report generate করো (CPU, RAM, disk, processes)

### Task 48 — Shell Scripting Basics 🟡

- Automation:

  ```bash
  #!/bin/bash
  set -euo pipefail  # Exit on error, undefined vars, pipe failures

  # Variables
  APP_NAME="my-backend"
  PORT=${PORT:-3000}  # Default value

  # Functions
  log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1"
  }

  # Conditionals
  if [[ "$NODE_ENV" == "production" ]]; then
    log "Running in production mode"
  else
    log "Running in development mode"
  fi

  # Loops
  for service in api worker scheduler; do
    log "Starting $service..."
    node "src/${service}/index.js" &
  done

  # Check if command exists
  if ! command -v node &> /dev/null; then
    log "ERROR: Node.js not installed"
    exit 1
  fi

  # Health check
  check_health() {
    local max_retries=30
    local count=0
    while [[ $count -lt $max_retries ]]; do
      if curl -sf "http://localhost:${PORT}/health" > /dev/null 2>&1; then
        log "Service is healthy!"
        return 0
      fi
      count=$((count + 1))
      log "Waiting for service... ($count/$max_retries)"
    done
    log "ERROR: Service failed to start"
    return 1
  }
  ```

  - **Exercise:**
    1. Deployment script লেখো (pull, install, build, restart)
    2. Database backup script লেখো
    3. Log rotation script লেখো

### Task 49 — SSH & Remote Server Management 🟢

- Remote server access:

  ```bash
  # Connect
  ssh user@192.168.1.100
  ssh -i ~/.ssh/my-key.pem ubuntu@ec2-xx-xx-xx-xx.compute.amazonaws.com

  # Key-based auth (no password!)
  ssh-keygen -t ed25519 -C "your@email.com"
  # Copy public key to server
  ssh-copy-id user@server

  # SSH Config (~/.ssh/config)
  Host myserver
    HostName 192.168.1.100
    User ubuntu
    IdentityFile ~/.ssh/my-key.pem
    Port 22
  # Now: ssh myserver

  # File transfer
  scp file.txt user@server:/path/to/destination
  scp -r folder/ user@server:/path/
  rsync -avz --progress local/ user@server:/remote/  # Better than scp

  # Port forwarding (tunnel)
  ssh -L 5432:localhost:5432 user@server  # Local → Remote
  # Now localhost:5432 → server's PostgreSQL

  # Run command remotely
  ssh user@server "cd /app && git pull && npm run build"
  ```

  - **Exercise:**
    1. SSH key pair generate করো
    2. Remote server access করো (use a VPS or local VM)
    3. SSH tunnel দিয়ে remote database access করো

### Task 50 — systemd & Service Management 🟡

- Run Node.js as a system service:

  ```ini
  # /etc/systemd/system/my-backend.service
  [Unit]
  Description=My Backend API
  After=network.target postgresql.service
  Requires=postgresql.service

  [Service]
  Type=simple
  User=nodeapp
  Group=nodeapp
  WorkingDirectory=/opt/my-backend
  ExecStart=/usr/bin/node /opt/my-backend/src/index.js
  Restart=on-failure
  RestartSec=5
  StandardOutput=journal
  StandardError=journal
  Environment=NODE_ENV=production
  Environment=PORT=3000
  EnvironmentFile=/opt/my-backend/.env

  # Security hardening
  NoNewPrivileges=true
  ProtectSystem=strict
  ProtectHome=true
  ReadWritePaths=/opt/my-backend/logs /opt/my-backend/uploads

  [Install]
  WantedBy=multi-user.target
  ```

  - **Commands:**
    ```bash
    sudo systemctl start my-backend      # Start
    sudo systemctl stop my-backend       # Stop
    sudo systemctl restart my-backend    # Restart
    sudo systemctl enable my-backend     # Start on boot
    sudo systemctl status my-backend     # Check status
    journalctl -u my-backend -f          # View logs (live)
    journalctl -u my-backend --since "1 hour ago"
    ```
  - **Exercise:**
    1. Node.js app systemd service হিসেবে deploy করো
    2. Auto-restart on crash test করো
    3. Log rotation configure করো

### Task 51 — Nginx Configuration 🟡

- Web server / reverse proxy:

  ```nginx
  # /etc/nginx/sites-available/my-backend
  upstream backend {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    server 127.0.0.1:3003;
    # Load balancing (default: round-robin)
  }

  server {
    listen 80;
    server_name api.example.com;

    # Redirect HTTP → HTTPS
    return 301 https://$host$request_uri;
  }

  server {
    listen 443 ssl http2;
    server_name api.example.com;

    # SSL
    ssl_certificate /etc/letsencrypt/live/api.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.example.com/privkey.pem;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Strict-Transport-Security "max-age=31536000" always;

    # Gzip
    gzip on;
    gzip_types application/json text/plain text/css;

    # Rate limiting
    limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;

    location /api/ {
      limit_req zone=api burst=20 nodelay;
      proxy_pass http://backend;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_cache_bypass $http_upgrade;
    }

    # Static files
    location /static/ {
      alias /opt/my-backend/public/;
      expires 30d;
      add_header Cache-Control "public, immutable";
    }
  }
  ```

  - **Exercise:**
    1. Nginx install ও configure করো
    2. Reverse proxy setup (Nginx → Node.js)
    3. SSL certificate setup (Let's Encrypt)
    4. Load balancing (multiple Node.js instances)

### Task 52 — PM2 — Process Manager for Node.js 🟢

- Production process management:

  ```bash
  # Install
  npm install -g pm2

  # Start app
  pm2 start src/index.js --name "api" -i max   # Cluster mode (all CPUs)
  pm2 start ecosystem.config.js                  # Config file

  # Management
  pm2 list                  # List all processes
  pm2 logs api              # View logs
  pm2 monit                 # Real-time monitor
  pm2 restart api           # Restart
  pm2 reload api            # Zero-downtime restart
  pm2 stop api              # Stop
  pm2 delete api            # Remove

  # Startup
  pm2 startup               # Generate startup script
  pm2 save                  # Save current process list
  ```

  - **ecosystem.config.js:**
    ```js
    module.exports = {
      apps: [
        {
          name: 'api',
          script: 'src/index.js',
          instances: 'max', // Use all CPUs
          exec_mode: 'cluster', // Cluster mode
          env: {
            NODE_ENV: 'development',
            PORT: 3000,
          },
          env_production: {
            NODE_ENV: 'production',
            PORT: 3000,
          },
          max_memory_restart: '1G', // Restart if > 1GB
          error_file: 'logs/err.log',
          out_file: 'logs/out.log',
          log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
          merge_logs: true,
          watch: false, // No watch in production
        },
      ],
    };
    ```
  - **Exercise:**
    1. PM2 দিয়ে cluster mode এ app চালাও
    2. Zero-downtime restart test করো
    3. Memory limit সেট করো ও test করো

### Task 53 — Log Management 🟡

- Structured logging:

  ```js
  class Logger {
    #levels = { error: 0, warn: 1, info: 2, http: 3, debug: 4 };
    #level;

    constructor(level = 'info') {
      this.#level = this.#levels[level] ?? 2;
    }

    #log(level, message, meta = {}) {
      if (this.#levels[level] > this.#level) return;

      const entry = {
        timestamp: new Date().toISOString(),
        level,
        message,
        ...meta,
        pid: process.pid,
        hostname: require('os').hostname(),
      };

      const output = JSON.stringify(entry);

      if (level === 'error') {
        process.stderr.write(output + '\n');
      } else {
        process.stdout.write(output + '\n');
      }
    }

    error(msg, meta) {
      this.#log('error', msg, meta);
    }
    warn(msg, meta) {
      this.#log('warn', msg, meta);
    }
    info(msg, meta) {
      this.#log('info', msg, meta);
    }
    http(msg, meta) {
      this.#log('http', msg, meta);
    }
    debug(msg, meta) {
      this.#log('debug', msg, meta);
    }

    // HTTP request logger middleware
    middleware() {
      return (req, res, next) => {
        const start = Date.now();
        res.on('finish', () => {
          this.http('Request completed', {
            method: req.method,
            url: req.url,
            status: res.statusCode,
            duration: `${Date.now() - start}ms`,
            ip: req.ip,
            userAgent: req.headers['user-agent'],
          });
        });
        next();
      };
    }
  }

  const logger = new Logger(process.env.LOG_LEVEL || 'info');
  ```

  - **Log format (JSON — structured):**
    ```json
    {"timestamp":"2026-04-16T10:30:00.000Z","level":"info","message":"Server started","port":3000,"pid":1234}
    {"timestamp":"2026-04-16T10:30:05.000Z","level":"http","message":"Request completed","method":"GET","url":"/api/users","status":200,"duration":"45ms"}
    {"timestamp":"2026-04-16T10:30:10.000Z","level":"error","message":"Database connection failed","error":"ECONNREFUSED","stack":"..."}
    ```
  - **Exercise:**
    1. Full logging library build করো (multiple levels, JSON output)
    2. Request logging middleware build করো
    3. Log rotation implement করো (daily files, max size)
    4. Error tracking: log errors with stack traces ও context

### Task 54 — Security Hardening (OS Level) 🟡

- Server security:

  ```bash
  # 1. Update system
  sudo apt update && sudo apt upgrade -y

  # 2. Firewall (UFW)
  sudo ufw default deny incoming
  sudo ufw default allow outgoing
  sudo ufw allow 22/tcp    # SSH
  sudo ufw allow 80/tcp    # HTTP
  sudo ufw allow 443/tcp   # HTTPS
  sudo ufw enable

  # 3. Disable root SSH login
  # /etc/ssh/sshd_config
  PermitRootLogin no
  PasswordAuthentication no  # Key-only auth
  MaxAuthTries 3

  # 4. Fail2ban (auto-block brute force)
  sudo apt install fail2ban
  # /etc/fail2ban/jail.local
  [sshd]
  enabled = true
  maxretry = 3
  bantime = 3600

  # 5. Automatic security updates
  sudo apt install unattended-upgrades
  sudo dpkg-reconfigure unattended-upgrades
  ```

  - **Exercise:**
    1. Server firewall configure করো
    2. SSH hardening (key-only, no root)
    3. Fail2ban setup করো
    4. Security audit: `sudo lynis audit system`

### Task 55 — Docker Basics Preview 🟢

- Container fundamentals (detailed in Phase 13):

  ```dockerfile
  # Dockerfile for Node.js app
  FROM node:20-alpine

  WORKDIR /app

  # Copy package files first (better caching)
  COPY package*.json ./
  RUN npm ci --only=production

  # Copy source
  COPY src/ ./src/

  # Non-root user
  USER node

  EXPOSE 3000

  CMD ["node", "src/index.js"]
  ```

  - **Basic commands:**
    ```bash
    docker build -t my-backend .
    docker run -p 3000:3000 --env-file .env my-backend
    docker ps           # Running containers
    docker logs <id>    # Container logs
    docker exec -it <id> sh  # Shell into container
    ```
  - **Exercise:** Node.js app Docker container এ চালাও

---

## Section C: Advanced OS Concepts for Backend (Tasks 56-65)

### Task 56 — Linux File System Deep Dive 🟢

- Directory structure:
  ```
  /               — Root
  ├── bin/        — Essential binaries (ls, cp, cat)
  ├── etc/        — Configuration files
  │   ├── nginx/
  │   ├── ssh/
  │   └── systemd/
  ├── var/        — Variable data
  │   ├── log/    — System logs
  │   ├── lib/    — Application state (databases)
  │   └── www/    — Web server files
  ├── home/       — User directories
  ├── tmp/        — Temporary files (cleared on reboot)
  ├── opt/        — Third-party software
  ├── proc/       — Virtual filesystem (process info)
  │   ├── cpuinfo
  │   ├── meminfo
  │   └── <pid>/  — Per-process info
  └── dev/        — Device files
      ├── null    — Discard output
      ├── zero    — Null bytes source
      └── random  — Random data
  ```

  - **Exercise:**
    1. `/proc` explore করো (CPU info, memory info, process info)
    2. `/var/log` এ system logs পড়ো
    3. Disk I/O monitor: `iostat`, `iotop`

### Task 57 — Linux Namespaces & cgroups (Container Foundation) 🔴

- How Docker actually works:
  - **Namespaces:** isolation
    ```
    PID namespace   — Process sees only its own processes
    NET namespace   — Separate network stack
    MNT namespace   — Separate filesystem
    UTS namespace   — Separate hostname
    IPC namespace   — Separate inter-process communication
    USER namespace  — Separate user/group IDs
    ```
  - **cgroups (Control Groups):** resource limits
    ```
    CPU limit    — Max 50% CPU
    Memory limit — Max 512MB RAM
    I/O limit    — Max 100MB/s disk
    Network      — Bandwidth limit
    ```
  - **Container = Namespace + cgroups + Union FS**
  - **Exercise:**
    1. Linux namespace create করো manually (`unshare` command)
    2. cgroup দিয়ে process resource limit করো
    3. Understand: Docker container = isolated Linux process

### Task 58 — TCP Tuning & Network Optimization 🟡

- Production server networking:

  ```bash
  # /etc/sysctl.conf — kernel parameters

  # Increase connection backlog
  net.core.somaxconn = 65535
  net.ipv4.tcp_max_syn_backlog = 65535

  # Reuse TIME_WAIT connections
  net.ipv4.tcp_tw_reuse = 1

  # Increase file descriptor limit
  fs.file-max = 1000000

  # TCP keepalive (detect dead connections)
  net.ipv4.tcp_keepalive_time = 60
  net.ipv4.tcp_keepalive_intvl = 10
  net.ipv4.tcp_keepalive_probes = 6

  # TCP buffer sizes
  net.core.rmem_max = 16777216
  net.core.wmem_max = 16777216
  ```

  - **Node.js TCP tuning:**
    ```js
    const server = http.createServer(app);
    server.keepAliveTimeout = 65000; // > Nginx's (60s) to prevent 502
    server.headersTimeout = 66000; // > keepAliveTimeout
    server.maxHeadersCount = 100;
    server.timeout = 120000; // 2 min request timeout
    ```
  - **Exercise:** Benchmark before/after TCP tuning

### Task 59 — Disk I/O & Storage 🟢

- Storage concepts:
  - **HDD vs SSD vs NVMe:**
    ```
    HDD:  100-200 IOPS, 100-200 MB/s (mechanical, cheap)
    SSD:  50,000-100,000 IOPS, 500 MB/s (flash, medium)
    NVMe: 500,000+ IOPS, 3500 MB/s (PCIe, fast, expensive)
    ```
  - **IOPS:** Input/Output Operations Per Second
  - **Database performance = I/O performance**
  - **Exercise:**
    1. Disk benchmarks চালাও (`fio`, `dd`)
    2. Database I/O patterns understand করো (sequential vs random)

### Task 60 — CPU Architecture & Performance 🟢

- How CPU affects backend:
  - **Single-threaded performance:** matters for Node.js
  - **Core count:** matters for cluster mode
  - **CPU cache (L1, L2, L3):** data locality matters
  - **NUMA (Non-Uniform Memory Access):** matters for large servers
  - **Context switching:** expensive — avoid too many threads/processes
  - **Exercise:**
    1. CPU benchmark: single-thread vs multi-thread
    2. Node.js cluster: test with different core counts
    3. Monitor context switches: `vmstat 1`

### Task 61 — Security: Principle of Least Privilege 🟢

- Node.js security at OS level:
  - **Never run as root!**
  - **Create dedicated user:**
    ```bash
    sudo useradd -r -s /bin/false nodeapp
    sudo chown -R nodeapp:nodeapp /opt/my-backend
    ```
  - **Limit capabilities:**
    ```bash
    # Allow binding to port 80 without root
    sudo setcap 'cap_net_bind_service=+ep' /usr/bin/node
    ```
  - **Sandboxing:** Docker container, SELinux, AppArmor
  - **Exercise:** Deploy app as non-root user with minimal permissions

### Task 62 — Backup & Disaster Recovery 🟡

- Data protection:

  ```bash
  #!/bin/bash
  # Backup script
  BACKUP_DIR="/backups/$(date +%Y-%m-%d)"
  mkdir -p "$BACKUP_DIR"

  # Database backup
  pg_dump -U postgres mydb | gzip > "$BACKUP_DIR/db.sql.gz"

  # Application files
  tar -czf "$BACKUP_DIR/app.tar.gz" /opt/my-backend/

  # Upload to S3
  aws s3 sync "$BACKUP_DIR" "s3://my-backups/$BACKUP_DIR"

  # Clean old backups (keep 30 days)
  find /backups -mtime +30 -type d -exec rm -rf {} +

  echo "Backup complete: $BACKUP_DIR"
  ```

  - **3-2-1 Rule:** 3 copies, 2 different media, 1 offsite
  - **Exercise:**
    1. Automated backup script লেখো
    2. Restore test করো (backup useless if can't restore!)
    3. Point-in-time recovery strategy document করো

### Task 63 — SSL/TLS Certificate Management 🟢

- Certificate lifecycle:

  ```bash
  # Let's Encrypt with Certbot
  sudo apt install certbot python3-certbot-nginx
  sudo certbot --nginx -d api.example.com

  # Auto-renewal (certbot adds cron automatically)
  sudo certbot renew --dry-run  # Test renewal

  # Manual certificate (for testing)
  openssl req -x509 -newkey rsa:4096 \
    -keyout key.pem -out cert.pem \
    -days 365 -nodes \
    -subj "/CN=localhost"
  ```

  - **Exercise:** SSL certificate setup ও auto-renewal configure করো

### Task 64 — Benchmarking & Load Testing 🟡

- Test server capacity:

  ```bash
  # Apache Bench (ab)
  ab -n 10000 -c 100 http://localhost:3000/api/users
  # -n = total requests, -c = concurrent connections

  # wrk (better)
  wrk -t12 -c400 -d30s http://localhost:3000/api/users
  # -t = threads, -c = connections, -d = duration

  # Autocannon (Node.js based)
  npx autocannon -c 100 -d 30 http://localhost:3000/api/users
  ```

  - **Key Metrics:**
    ```
    Requests/sec       — Throughput
    Latency (p50, p99) — Response time (50th, 99th percentile)
    Errors             — Error rate
    CPU/Memory usage   — Resource consumption
    ```
  - **Exercise:**
    1. Benchmark তোমার API (requests/sec, latency)
    2. Identify bottleneck (CPU? I/O? Memory?)
    3. Optimize ও re-benchmark

### Task 65 — Build: Server Provisioning Script ⚫

- **Phase 02 Final Project:**
  - Automated server setup script:

    ```bash
    #!/bin/bash
    set -euo pipefail

    # This script sets up a fresh Ubuntu server for Node.js backend

    echo "=== Updating system ==="
    sudo apt update && sudo apt upgrade -y

    echo "=== Installing Node.js 20 ==="
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt install -y nodejs

    echo "=== Installing PostgreSQL ==="
    sudo apt install -y postgresql postgresql-contrib

    echo "=== Installing Redis ==="
    sudo apt install -y redis-server

    echo "=== Installing Nginx ==="
    sudo apt install -y nginx

    echo "=== Installing PM2 ==="
    sudo npm install -g pm2

    echo "=== Configuring firewall ==="
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    sudo ufw allow 22/tcp
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    sudo ufw --force enable

    echo "=== Creating app user ==="
    sudo useradd -r -m -s /bin/bash nodeapp

    echo "=== Setting up SSL ==="
    sudo apt install -y certbot python3-certbot-nginx

    echo "=== Security hardening ==="
    # SSH hardening
    sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
    sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
    sudo systemctl restart sshd

    # Fail2ban
    sudo apt install -y fail2ban
    sudo systemctl enable fail2ban

    echo "=== Server provisioning complete! ==="
    ```

  - **Features:**
    - Install: Node.js, PostgreSQL, Redis, Nginx, PM2
    - Security: Firewall, SSH hardening, Fail2ban
    - User: Non-root app user
    - SSL: Let's Encrypt ready
    - Logging: Configured ও rotated
  - **Exercise:** Fresh server setup → deploy → benchmark → secure

---

## ✅ Phase 02 Completion Checklist

- [ ] সব 30 tasks complete
- [ ] Linux commands fluently use করতে পারো
- [ ] Shell script লিখতে পারো
- [ ] Process, Thread, Memory management বুঝো
- [ ] SSH, systemd, Nginx, PM2 configure করতে পারো
- [ ] Server security hardening করতে পারো
- [ ] Backup ও recovery strategy জানো
- [ ] Server provisioning automate করতে পারো

> _"যে developer Linux জানে না, সে backend developer না — সে শুধু local developer।"_
> _"Production server manage করতে পারলে তুমি DevOps ও করতে পারবে।"_
