# Backend Phase 01 — Internet, Networking & How Servers Work (Tasks 1-35)

> **Backend developer হতে হলে সবার আগে বুঝতে হবে — internet কিভাবে কাজ করে।**
> **HTTP request browser থেকে server এ যায় কিভাবে? Data কিভাবে travel করে? DNS কী করে?**
> **এই phase skip করলে তুমি সারাজীবন "কোড চলে কিন্তু কেন চলে জানি না" এই level এ থাকবে।**

---

## Section A: How the Internet Works (Tasks 1-8)

### Task 1 — Internet Fundamentals 🟢

- Internet কিভাবে কাজ করে:
  - **Internet = network of networks** — millions of computers connected
  - **Client-Server Model:**
    ```
    Browser (Client) ──── Request ────→ Server
                     ←─── Response ────
    ```
  - **IP Address:** প্রতিটা device এর unique address (192.168.1.1, ::1)
  - **IPv4 vs IPv6:**
    - IPv4: 32-bit (4.3 billion addresses — running out!)
    - IPv6: 128-bit (340 undecillion addresses)
  - **Port:** একটা computer এ multiple services চলে — port দিয়ে distinguish করে
    - HTTP: 80, HTTPS: 443, SSH: 22, PostgreSQL: 5432, MongoDB: 27017
  - **Exercise:**
    1. `ping google.com` — response time দেখো
    2. `traceroute google.com` (Linux) / `tracert google.com` (Windows) — packet route দেখো
    3. `nslookup google.com` — DNS resolution দেখো
    4. `netstat -an` — active connections দেখো
    5. Document: data কিভাবে তোমার PC থেকে Google server এ যায়

### Task 2 — OSI Model & TCP/IP Stack 🟡

- Network layers বোঝো:
  ```
  OSI Model (7 Layers)          TCP/IP Model (4 Layers)
  ─────────────────────         ───────────────────────
  7. Application (HTTP, DNS)  ─→ Application
  6. Presentation (SSL/TLS)   ─→ Application
  5. Session                  ─→ Application
  4. Transport (TCP, UDP)     ─→ Transport
  3. Network (IP, ICMP)       ─→ Internet
  2. Data Link (Ethernet)     ─→ Network Access
  1. Physical (Cables, WiFi)  ─→ Network Access
  ```

  - **কেন জানতে হবে?**
    - Network debugging করতে (কোন layer এ problem?)
    - Performance optimization (TCP tuning)
    - Security (কোন layer এ encryption?)
  - **Exercise:**
    1. প্রতিটা layer এর 3টা করে protocol জানো
    2. Wireshark install করে HTTP request capture করো
    3. দেখো: একটা HTTP request কোন কোন layer দিয়ে যায়
    4. TCP 3-way handshake capture করো Wireshark এ

### Task 3 — TCP Deep Dive 🟡

- TCP (Transmission Control Protocol):
  - **Reliable, ordered, connection-oriented**
  - **3-Way Handshake (Connection establish):**
    ```
    Client ──── SYN ────────→ Server
    Client ←─── SYN-ACK ────  Server
    Client ──── ACK ────────→ Server
    (Connection established!)
    ```
  - **4-Way Teardown (Connection close):**
    ```
    Client ──── FIN ────────→ Server
    Client ←─── ACK ─────── Server
    Client ←─── FIN ─────── Server
    Client ──── ACK ────────→ Server
    ```
  - **TCP Features:**
    - **Flow Control:** receiver বলে কতটুকু data handle করতে পারবে (Window Size)
    - **Congestion Control:** network congested হলে slow down (Slow Start, AIMD)
    - **Retransmission:** packet lost হলে আবার পাঠায়
    - **Ordering:** sequence number দিয়ে order maintain করে
  - **Exercise:**
    1. TCP server + client build করো Node.js `net` module দিয়ে:

       ```js
       const net = require('net');

       // TCP Server
       const server = net.createServer((socket) => {
         console.log('Client connected');
         socket.on('data', (data) => {
           console.log('Received:', data.toString());
           socket.write('Echo: ' + data.toString());
         });
         socket.on('end', () => console.log('Client disconnected'));
       });

       server.listen(3000, () => console.log('TCP Server on port 3000'));

       // TCP Client (separate file)
       const client = net.createConnection({ port: 3000 }, () => {
         client.write('Hello Server!');
       });
       client.on('data', (data) => {
         console.log('Server says:', data.toString());
         client.end();
       });
       ```

    2. Wireshark দিয়ে 3-way handshake capture করো
    3. Multiple clients connect করো simultaneously

### Task 4 — UDP & When to Use It 🟢

- UDP (User Datagram Protocol):
  - **Unreliable, unordered, connectionless** — but FAST!
  - **No handshake, no guarantee**
  - **Use cases:**
    - DNS queries (fast lookup)
    - Video streaming (some frame loss OK)
    - Online gaming (speed > reliability)
    - VoIP (voice calls)
  - **TCP vs UDP:**
    ```
    Feature        TCP              UDP
    ────────────   ───────────────  ───────────────
    Connection     Yes (handshake)  No
    Reliability    Guaranteed       Best-effort
    Ordering       Yes              No
    Speed          Slower           Faster
    Overhead       More             Less
    Use case       HTTP, Email, DB  DNS, Gaming, Video
    ```
  - **Exercise:**
    1. UDP server + client build করো:

       ```js
       const dgram = require('dgram');

       // UDP Server
       const server = dgram.createSocket('udp4');
       server.on('message', (msg, rinfo) => {
         console.log(`${rinfo.address}:${rinfo.port} — ${msg}`);
         server.send('Got it!', rinfo.port, rinfo.address);
       });
       server.bind(3000);

       // UDP Client
       const client = dgram.createSocket('udp4');
       client.send('Hello UDP!', 3000, 'localhost', (err) => {
         if (err) console.error(err);
       });
       client.on('message', (msg) => {
         console.log('Server:', msg.toString());
         client.close();
       });
       ```

    2. TCP vs UDP latency benchmark করো

### Task 5 — DNS — The Internet's Phone Book 🟡

- DNS (Domain Name System):
  - **google.com → 142.250.190.14** (domain → IP translation)
  - **DNS Resolution Process:**
    ```
    Browser Cache → OS Cache → Router Cache → ISP DNS → Root DNS → TLD DNS → Authoritative DNS
    ```
  - **DNS Record Types:**
    ```
    A      — Domain → IPv4 address
    AAAA   — Domain → IPv6 address
    CNAME  — Domain → Another domain (alias)
    MX     — Domain → Mail server
    NS     — Domain → Name server
    TXT    — Domain → Text (SPF, DKIM verification)
    SRV    — Domain → Service location (port + host)
    PTR    — IP → Domain (reverse lookup)
    SOA    — Zone authority information
    ```
  - **TTL (Time To Live):** কতক্ষণ cache রাখবে (seconds)
  - **Exercise:**
    1. `dig google.com` — full DNS query দেখো
    2. `dig +trace google.com` — step by step resolution দেখো
    3. Node.js `dns` module দিয়ে DNS lookup করো:
       ```js
       const dns = require('dns');
       dns.resolve4('google.com', (err, addresses) => {
         console.log('A records:', addresses);
       });
       dns.resolveMx('google.com', (err, records) => {
         console.log('MX records:', records);
       });
       ```
    4. নিজে simple DNS resolver build করো (UDP দিয়ে DNS query পাঠাও)

### Task 6 — HTTP/1.1 Deep Dive 🔴

- HTTP protocol thoroughly বোঝো:
  - **Request structure:**
    ```
    GET /api/users HTTP/1.1        ← Request Line (Method, Path, Version)
    Host: api.example.com          ← Required header
    Accept: application/json       ← What response format I want
    Authorization: Bearer xyz123   ← Auth token
    Content-Type: application/json ← Body format (for POST/PUT)
    Connection: keep-alive         ← Persistent connection
    Cache-Control: no-cache        ← Caching directive
                                   ← Empty line (separates headers from body)
    {"name": "John"}               ← Request Body (optional)
    ```
  - **Response structure:**
    ```
    HTTP/1.1 200 OK               ← Status Line
    Content-Type: application/json ← Response format
    Content-Length: 45             ← Body size
    Set-Cookie: session=abc123    ← Cookie
    Cache-Control: max-age=3600   ← Cache 1 hour
    X-Request-Id: uuid-here       ← Custom header
                                   ← Empty line
    {"id": 1, "name": "John"}     ← Response Body
    ```
  - **HTTP Methods (Verbs):**
    ```
    GET     — Read data (idempotent, safe, cacheable)
    POST    — Create data (not idempotent)
    PUT     — Replace entire resource (idempotent)
    PATCH   — Partial update (not necessarily idempotent)
    DELETE  — Remove resource (idempotent)
    HEAD    — Same as GET but no body (check if resource exists)
    OPTIONS — What methods are supported? (CORS preflight)
    ```
  - **Status Codes (MUST memorize):**

    ```
    1xx — Informational
      100 Continue
      101 Switching Protocols (WebSocket upgrade)

    2xx — Success
      200 OK
      201 Created (after POST)
      204 No Content (after DELETE)

    3xx — Redirection
      301 Moved Permanently (SEO — permanent redirect)
      302 Found (temporary redirect)
      304 Not Modified (cache still valid)

    4xx — Client Error
      400 Bad Request (validation failed)
      401 Unauthorized (not authenticated)
      403 Forbidden (authenticated but not authorized)
      404 Not Found
      405 Method Not Allowed
      409 Conflict (duplicate resource)
      422 Unprocessable Entity (semantic error)
      429 Too Many Requests (rate limited)

    5xx — Server Error
      500 Internal Server Error
      502 Bad Gateway (upstream server error)
      503 Service Unavailable (server overloaded)
      504 Gateway Timeout
    ```

  - **Exercise:**
    1. Raw HTTP server build করো (NO Express, শুধু Node.js `http` module):

       ```js
       const http = require('http');

       const server = http.createServer((req, res) => {
         const { method, url, headers } = req;
         console.log(`${method} ${url}`);
         console.log('Headers:', headers);

         let body = '';
         req.on('data', (chunk) => (body += chunk));
         req.on('end', () => {
           // Route handling
           if (method === 'GET' && url === '/api/users') {
             res.writeHead(200, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify([{ id: 1, name: 'John' }]));
           } else if (method === 'POST' && url === '/api/users') {
             const user = JSON.parse(body);
             res.writeHead(201, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify({ id: 2, ...user }));
           } else {
             res.writeHead(404);
             res.end('Not Found');
           }
         });
       });

       server.listen(3000, () => console.log('Server: http://localhost:3000'));
       ```

    2. সব HTTP method handle করো (GET, POST, PUT, PATCH, DELETE)
    3. Custom headers send ও receive করো
    4. curl / Postman দিয়ে test করো

### Task 7 — HTTP/2 & HTTP/3 🟡

- Modern HTTP versions:
  - **HTTP/1.1 Problems:**
    - Head-of-line blocking (একটা request block করলে বাকি সব wait করে)
    - 6 connection limit per domain
    - Text-based protocol (slow to parse)
    - No server push
  - **HTTP/2 Solutions:**
    - **Multiplexing:** multiple requests on single connection
    - **Header Compression:** HPACK (repeated headers compress হয়)
    - **Server Push:** server client কে ask না করেই resource পাঠাতে পারে
    - **Binary Protocol:** text না, binary framing (faster parsing)
    - **Stream Priority:** important requests আগে handle হয়

    ```
    HTTP/1.1:  Request 1 ────→  Response 1
               Request 2 ────→  Response 2 (WAIT for 1!)
               Request 3 ────→  Response 3 (WAIT for 2!)

    HTTP/2:    Request 1 ─┐
               Request 2 ─┼──→ All multiplexed on 1 connection
               Request 3 ─┘    Responses arrive independently
    ```

  - **HTTP/3 (QUIC):**
    - **UDP-based** (not TCP!)
    - 0-RTT connection establishment
    - No head-of-line blocking at transport level
    - Built-in encryption (TLS 1.3 integrated)
  - **Exercise:**
    1. HTTP/2 server build করো Node.js দিয়ে:

       ```js
       const http2 = require('http2');
       const fs = require('fs');

       const server = http2.createSecureServer({
         key: fs.readFileSync('key.pem'),
         cert: fs.readFileSync('cert.pem'),
       });

       server.on('stream', (stream, headers) => {
         stream.respond({ ':status': 200, 'content-type': 'text/html' });
         stream.end('<h1>Hello HTTP/2!</h1>');
       });

       server.listen(8443);
       ```

    2. HTTP/1.1 vs HTTP/2 performance comparison করো
    3. Server Push implement করো

### Task 8 — HTTPS & TLS/SSL 🟡

- Encryption in transit:
  - **TLS Handshake (simplified):**
    ```
    Client ──── ClientHello (supported ciphers) ────→ Server
    Client ←─── ServerHello + Certificate ──────────  Server
    Client ──── Key Exchange (pre-master secret) ───→ Server
    Both derive session keys from pre-master secret
    Client ──── Finished (encrypted) ───────────────→ Server
    Client ←─── Finished (encrypted) ──────────────── Server
    (Encrypted communication begins!)
    ```
  - **Key Concepts:**
    - **Certificate:** server identity prove করে (signed by CA)
    - **CA (Certificate Authority):** Let's Encrypt, DigiCert
    - **Symmetric Encryption:** AES (fast, for data transfer)
    - **Asymmetric Encryption:** RSA/ECDSA (slow, for key exchange)
    - **TLS 1.3:** faster handshake (1-RTT), removed weak ciphers
  - **Exercise:**
    1. Self-signed certificate generate করো:
       ```bash
       openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
       ```
    2. HTTPS server build করো:
       ```js
       const https = require('https');
       const fs = require('fs');
       const server = https.createServer(
         {
           key: fs.readFileSync('key.pem'),
           cert: fs.readFileSync('cert.pem'),
         },
         (req, res) => {
           res.writeHead(200);
           res.end('Secure!');
         }
       );
       server.listen(443);
       ```
    3. Let's Encrypt দিয়ে free certificate পাওয়ার process শেখো
    4. TLS handshake Wireshark এ capture করো

---

## Section B: Sockets & Real-time Communication (Tasks 9-15)

### Task 9 — Socket Programming Fundamentals 🟡

- Sockets = low-level network communication:
  - **Socket = IP Address + Port + Protocol**
  - **Types:**
    - **Stream Socket (SOCK_STREAM):** TCP-based, reliable
    - **Datagram Socket (SOCK_DGRAM):** UDP-based, fast
  - **Socket Lifecycle:**
    ```
    Server: socket() → bind() → listen() → accept() → read/write → close()
    Client: socket() → connect() → read/write → close()
    ```
  - **Exercise:**
    1. Chat application build করো raw sockets দিয়ে:

       ```js
       // Chat Server
       const net = require('net');
       const clients = [];

       const server = net.createServer((socket) => {
         clients.push(socket);
         socket.write('Welcome to the chat!\n');

         socket.on('data', (data) => {
           const msg = data.toString().trim();
           // Broadcast to all other clients
           clients.forEach((c) => {
             if (c !== socket && !c.destroyed) {
               c.write(`User: ${msg}\n`);
             }
           });
         });

         socket.on('end', () => {
           clients.splice(clients.indexOf(socket), 1);
         });
       });

       server.listen(3000, () => console.log('Chat server on 3000'));
       ```

    2. Multiple users join করে chat করতে পারে
    3. Username system add করো
    4. Private messaging add করো

### Task 10 — WebSocket Protocol 🟡

- Full-duplex communication:
  - **HTTP = Request-Response (client initiates)**
  - **WebSocket = Bidirectional (both can send anytime)**
  - **WebSocket Handshake (HTTP Upgrade):**

    ```
    Client: GET /chat HTTP/1.1
            Upgrade: websocket
            Connection: Upgrade
            Sec-WebSocket-Key: base64-encoded-key

    Server: HTTP/1.1 101 Switching Protocols
            Upgrade: websocket
            Connection: Upgrade
            Sec-WebSocket-Accept: computed-accept-key
    ```

  - **Frame format:** opcode (text/binary/close/ping/pong) + payload
  - **Exercise:**
    1. Raw WebSocket server build করো (NO ws library):

       ```js
       const http = require('http');
       const crypto = require('crypto');

       const server = http.createServer();

       server.on('upgrade', (req, socket) => {
         const key = req.headers['sec-websocket-key'];
         const acceptKey = crypto
           .createHash('sha1')
           .update(key + '258EAFA5-E914-47DA-95CA-C5AB0DC85B11')
           .digest('base64');

         socket.write(
           'HTTP/1.1 101 Switching Protocols\r\n' +
             'Upgrade: websocket\r\n' +
             'Connection: Upgrade\r\n' +
             `Sec-WebSocket-Accept: ${acceptKey}\r\n\r\n`
         );

         socket.on('data', (buffer) => {
           // Parse WebSocket frame
           const opcode = buffer[0] & 0x0f;
           const isMasked = (buffer[1] & 0x80) !== 0;
           let payloadLength = buffer[1] & 0x7f;
           let offset = 2;

           if (payloadLength === 126) {
             payloadLength = buffer.readUInt16BE(2);
             offset = 4;
           }

           const mask = isMasked ? buffer.slice(offset, offset + 4) : null;
           offset += isMasked ? 4 : 0;

           const data = buffer.slice(offset, offset + payloadLength);
           if (isMasked) {
             for (let i = 0; i < data.length; i++) {
               data[i] ^= mask[i % 4];
             }
           }

           console.log('Received:', data.toString());
           // Echo back (unmasked frame)
           const response = Buffer.alloc(2 + data.length);
           response[0] = 0x81; // text frame
           response[1] = data.length;
           data.copy(response, 2);
           socket.write(response);
         });
       });

       server.listen(8080);
       ```

    2. পরে `ws` library দিয়ে same কাজ করো — difference বোঝো
    3. Real-time notification system build করো

### Task 11 — Server-Sent Events (SSE) 🟢

- Server → Client one-way stream:
  - **WebSocket = bidirectional, SSE = server to client only**
  - **SSE is simpler, uses regular HTTP, auto-reconnects**
  - **Use cases:** live feeds, notifications, stock prices, logs

  ```js
  // SSE Server
  const http = require('http');

  http
    .createServer((req, res) => {
      if (req.url === '/events') {
        res.writeHead(200, {
          'Content-Type': 'text/event-stream',
          'Cache-Control': 'no-cache',
          Connection: 'keep-alive',
          'Access-Control-Allow-Origin': '*',
        });

        // Send event every 2 seconds
        let id = 0;
        const interval = setInterval(() => {
          res.write(`id: ${++id}\n`);
          res.write(`event: message\n`);
          res.write(`data: ${JSON.stringify({ time: new Date(), count: id })}\n\n`);
        }, 2000);

        req.on('close', () => clearInterval(interval));
      }
    })
    .listen(3000);

  // Client (browser)
  // const source = new EventSource('http://localhost:3000/events');
  // source.onmessage = (e) => console.log(JSON.parse(e.data));
  ```

  - **Exercise:**
    1. Live log viewer (server logs stream to browser)
    2. Real-time dashboard (CPU/Memory usage)
    3. Compare: WebSocket vs SSE — কখন কোনটা use করবে?

### Task 12 — Long Polling & Short Polling 🟢

- Polling patterns:
  - **Short Polling:** client regularly asks "any updates?"
    ```js
    // Client polls every 5 seconds
    setInterval(async () => {
      const res = await fetch('/api/notifications');
      const data = await res.json();
      if (data.length > 0) updateUI(data);
    }, 5000);
    ```
  - **Long Polling:** client asks, server holds connection until data available

    ```js
    // Server
    app.get('/api/poll', async (req, res) => {
      const lastId = req.query.lastId || 0;

      // Wait for new data (max 30 seconds)
      const data = await waitForNewData(lastId, 30000);

      if (data) {
        res.json(data);
      } else {
        res.status(204).end(); // No new data, client will retry
      }
    });
    ```

  - **Comparison:**
    ```
    Method         Latency    Server Load   Complexity   Use Case
    ────────────── ────────── ───────────── ──────────── ─────────────────
    Short Polling  High       High          Low          Simple dashboards
    Long Polling   Medium     Medium        Medium       Chat (fallback)
    SSE            Low        Low           Low          Notifications
    WebSocket      Lowest     Lowest        High         Real-time apps
    ```
  - **Exercise:** Build the same chat app using all 4 methods → compare

### Task 13 — CORS (Cross-Origin Resource Sharing) 🟡

- Browser security mechanism:
  - **Same-Origin Policy:** browser blocks requests to different origins
  - **Origin = Protocol + Host + Port**
    ```
    http://example.com:80  → Same origin
    https://example.com:443 → Different (protocol)
    http://api.example.com  → Different (host)
    http://example.com:8080 → Different (port)
    ```
  - **CORS Headers:**
    ```
    Access-Control-Allow-Origin: https://myapp.com  (or *)
    Access-Control-Allow-Methods: GET, POST, PUT, DELETE
    Access-Control-Allow-Headers: Content-Type, Authorization
    Access-Control-Allow-Credentials: true
    Access-Control-Max-Age: 86400  (preflight cache)
    ```
  - **Preflight Request (OPTIONS):**

    ```
    Browser ──── OPTIONS /api/users ────→ Server
                 Origin: https://myapp.com
                 Access-Control-Request-Method: POST
                 Access-Control-Request-Headers: Content-Type

    Server  ←─── 204 No Content ─────── Server
                  Access-Control-Allow-Origin: https://myapp.com
                  Access-Control-Allow-Methods: POST
                  Access-Control-Allow-Headers: Content-Type
    (Browser now sends actual POST request)
    ```

  - **Exercise:**
    1. CORS middleware নিজে build করো:

       ```js
       function cors(options = {}) {
         const {
           origin = '*',
           methods = 'GET,HEAD,PUT,PATCH,POST,DELETE',
           headers = 'Content-Type,Authorization',
           credentials = false,
           maxAge = 86400,
         } = options;

         return (req, res, next) => {
           const requestOrigin = req.headers.origin;

           // Check if origin is allowed
           if (typeof origin === 'function') {
             res.setHeader('Access-Control-Allow-Origin', origin(requestOrigin));
           } else {
             res.setHeader('Access-Control-Allow-Origin', origin);
           }

           if (credentials) {
             res.setHeader('Access-Control-Allow-Credentials', 'true');
           }

           // Handle preflight
           if (req.method === 'OPTIONS') {
             res.setHeader('Access-Control-Allow-Methods', methods);
             res.setHeader('Access-Control-Allow-Headers', headers);
             res.setHeader('Access-Control-Max-Age', maxAge);
             res.writeHead(204);
             return res.end();
           }

           next();
         };
       }
       ```

    2. Frontend (port 3000) → Backend (port 5000) communication test করো
    3. Credential-based CORS (cookies cross-origin) implement করো

### Task 14 — Cookies, Sessions & Headers Deep Dive 🟡

- HTTP state management:
  - **Cookies:**
    ```
    Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Strict; Max-Age=3600; Path=/; Domain=.example.com
    ```

    - **HttpOnly:** JavaScript access blocked (XSS protection)
    - **Secure:** HTTPS only
    - **SameSite:** CSRF protection (Strict / Lax / None)
    - **Max-Age:** expiry in seconds
    - **Domain:** which domains can read cookie
    - **Path:** which paths can read cookie
  - **Important Headers for Backend Devs:**

    ```
    Request Headers:
    ───────────────
    Authorization: Bearer <token>   — Auth token
    Content-Type: application/json  — Request body format
    Accept: application/json        — Expected response format
    User-Agent: ...                 — Client info
    X-Forwarded-For: ...            — Real client IP (behind proxy)
    X-Request-Id: ...               — Request tracing
    If-None-Match: "etag-value"     — Conditional request (caching)
    If-Modified-Since: <date>       — Conditional request (caching)

    Response Headers:
    ────────────────
    Content-Type: application/json  — Response format
    Cache-Control: ...              — Caching policy
    ETag: "hash-of-content"         — Content fingerprint
    X-RateLimit-Limit: 100         — Rate limit info
    X-RateLimit-Remaining: 95      — Remaining requests
    Retry-After: 60                — When to retry (rate limited)
    ```

  - **Exercise:**
    1. Cookie parser middleware build করো (no library)
    2. Session system build করো cookies দিয়ে
    3. ETag-based caching implement করো
    4. Rate limit headers implement করো

### Task 15 — Proxy, Reverse Proxy & Load Balancer 🟡

- Traffic management:
  - **Forward Proxy:** client এর হয়ে request করে (VPN-like)
    ```
    Client → Forward Proxy → Internet → Server
    (Server sees proxy IP, not client IP)
    ```
  - **Reverse Proxy:** server এর সামনে বসে (Nginx, HAProxy)
    ```
    Client → Reverse Proxy → Server 1
                           → Server 2
                           → Server 3
    (Client sees proxy, not actual servers)
    ```
  - **Load Balancer Algorithms:**
    ```
    Round Robin       — Request 1→S1, 2→S2, 3→S3, 4→S1...
    Weighted Round    — S1 gets 3x, S2 gets 2x, S3 gets 1x
    Least Connections — Send to server with fewest active connections
    IP Hash          — Same client IP always goes to same server
    Random           — Random server selection
    ```
  - **Exercise:**
    1. Simple reverse proxy build করো Node.js দিয়ে:

       ```js
       const http = require('http');

       const servers = [
         { host: 'localhost', port: 3001 },
         { host: 'localhost', port: 3002 },
         { host: 'localhost', port: 3003 },
       ];
       let current = 0;

       const proxy = http.createServer((req, res) => {
         const target = servers[current];
         current = (current + 1) % servers.length; // Round Robin

         const proxyReq = http.request(
           {
             hostname: target.host,
             port: target.port,
             path: req.url,
             method: req.method,
             headers: { ...req.headers, 'X-Forwarded-For': req.socket.remoteAddress },
           },
           (proxyRes) => {
             res.writeHead(proxyRes.statusCode, proxyRes.headers);
             proxyRes.pipe(res);
           }
         );

         req.pipe(proxyReq);
       });

       proxy.listen(3000, () => console.log('Load Balancer on :3000'));
       ```

    2. 3টা backend server চালাও → load balancer দিয়ে distribute করো
    3. Least connections algorithm implement করো
    4. Health check add করো (dead server skip করো)

---

## Section C: API Concepts & Data Formats (Tasks 16-25)

### Task 16 — REST API Design Principles 🟡

- RESTful API design:
  - **REST Constraints:**
    1. **Client-Server** — separated concerns
    2. **Stateless** — server stores no client state between requests
    3. **Cacheable** — responses must define if cacheable
    4. **Uniform Interface** — consistent URL patterns
    5. **Layered System** — client doesn't know if talking to server or proxy
    6. **Code on Demand** (optional) — server can send executable code
  - **URL Design:**

    ```
    ✅ Good:
    GET    /api/v1/users              — List all users
    GET    /api/v1/users/123          — Get user 123
    POST   /api/v1/users              — Create user
    PUT    /api/v1/users/123          — Replace user 123
    PATCH  /api/v1/users/123          — Update user 123 partially
    DELETE /api/v1/users/123          — Delete user 123
    GET    /api/v1/users/123/orders   — User 123's orders

    ❌ Bad:
    GET    /api/getUsers
    POST   /api/createUser
    GET    /api/deleteUser?id=123
    POST   /api/user/update
    ```

  - **Pagination:**

    ```
    GET /api/users?page=2&limit=20
    GET /api/users?cursor=eyJpZCI6MTAwfQ&limit=20  (cursor-based — better!)

    Response:
    {
      "data": [...],
      "pagination": {
        "total": 1000,
        "page": 2,
        "limit": 20,
        "totalPages": 50,
        "hasNext": true,
        "hasPrev": true,
        "nextCursor": "eyJpZCI6MTIwfQ"
      }
    }
    ```

  - **Filtering, Sorting, Searching:**
    ```
    GET /api/users?status=active&role=admin           — Filter
    GET /api/users?sort=-createdAt,name                — Sort (- = desc)
    GET /api/users?search=john                         — Search
    GET /api/users?fields=id,name,email                — Field selection
    ```
  - **Exercise:** Design complete REST API for an e-commerce system

### Task 17 — JSON, XML & Data Serialization 🟢

- Data formats:
  - **JSON (JavaScript Object Notation):**
    ```json
    {
      "name": "John",
      "age": 30,
      "scores": [98, 87, 95],
      "address": { "city": "Dhaka" },
      "active": true,
      "deleted": null
    }
    ```
  - **XML (legacy, still used in SOAP):**
    ```xml
    <user>
      <name>John</name>
      <age>30</age>
    </user>
    ```
  - **Protocol Buffers (protobuf — binary, fast):**
    ```protobuf
    message User {
      string name = 1;
      int32 age = 2;
      repeated int32 scores = 3;
    }
    ```
  - **MessagePack (binary JSON — smaller, faster):**
  - **Exercise:**
    1. JSON parser build করো from scratch (no `JSON.parse`)
    2. JSON.stringify edge cases handle করো (circular references, BigInt, undefined)
    3. Compare serialization speed: JSON vs MessagePack vs Protobuf

### Task 18 — GraphQL Basics 🟡

- Query language for APIs:
  - **REST Problem:** over-fetching ও under-fetching

    ```
    REST: GET /api/users/123         → gets ALL user fields
          GET /api/users/123/posts   → separate request for posts

    GraphQL: Single request, get exactly what you need
    ```

  - **GraphQL Query:**
    ```graphql
    query {
      user(id: 123) {
        name
        email
        posts(limit: 5) {
          title
          createdAt
        }
      }
    }
    ```
  - **GraphQL Server (basic):**

    ```js
    const { buildSchema } = require('graphql');
    const { createHandler } = require('graphql-http/lib/use/http');

    const schema = buildSchema(`
      type User { id: ID!, name: String!, email: String!, posts: [Post!]! }
      type Post { id: ID!, title: String!, content: String! }
      type Query { user(id: ID!): User, users: [User!]! }
      type Mutation { createUser(name: String!, email: String!): User! }
    `);

    const root = {
      user: ({ id }) => getUserById(id),
      users: () => getAllUsers(),
      createUser: ({ name, email }) => createUser(name, email),
    };
    ```

  - **REST vs GraphQL:**
    ```
    Feature      REST              GraphQL
    ──────────── ────────────────  ───────────────
    Endpoints    Multiple          Single (/graphql)
    Data fetch   Fixed response    Client chooses fields
    Versioning   /v1/, /v2/       Schema evolution
    Caching      HTTP caching      More complex
    Learning     Easy              Medium
    ```
  - **Exercise:** Build GraphQL API for a blog (users, posts, comments)

### Task 19 — gRPC — High Performance RPC 🟡

- Remote Procedure Call framework:
  - **gRPC = Google's RPC using Protocol Buffers**
  - **HTTP/2 based, binary serialization, bidirectional streaming**
  - **Use cases:** microservice communication, real-time data
  - **Proto file:**

    ```protobuf
    syntax = "proto3";

    service UserService {
      rpc GetUser (GetUserRequest) returns (User);
      rpc ListUsers (ListUsersRequest) returns (stream User);  // Server streaming
      rpc CreateUser (User) returns (User);
    }

    message User {
      int32 id = 1;
      string name = 2;
      string email = 3;
    }

    message GetUserRequest { int32 id = 1; }
    message ListUsersRequest { int32 limit = 1; }
    ```

  - **Exercise:**
    1. gRPC server ও client build করো Node.js দিয়ে
    2. Unary, server streaming, client streaming, bidirectional streaming
    3. Compare: REST vs GraphQL vs gRPC performance

### Task 20 — API Versioning Strategies 🟢

- API evolve করার সময় backward compatibility:
  - **URL versioning:** `/api/v1/users`, `/api/v2/users`
  - **Header versioning:** `Accept: application/vnd.api.v2+json`
  - **Query parameter:** `/api/users?version=2`
  - **Each approach pros/cons:**
    ```
    URL:    ✅ Simple, clear    ❌ URL changes, caching issues
    Header: ✅ Clean URLs       ❌ Hidden, harder to test
    Query:  ✅ Easy to switch   ❌ Can be messy
    ```
  - **Best practice:** URL versioning (most common, most clear)
  - **Exercise:**
    1. API with v1 ও v2 running simultaneously
    2. Deprecation headers add করো
    3. Migration guide: v1 → v2

### Task 21 — API Rate Limiting 🟡

- Protect server from abuse:
  - **Algorithms:**
    - **Fixed Window:** 100 requests per minute (resets every minute)
    - **Sliding Window:** rolling 60-second window
    - **Token Bucket:** tokens replenish at fixed rate, request uses 1 token
    - **Leaky Bucket:** requests processed at constant rate, overflow rejected
  - **Exercise:**
    1. Rate limiter middleware build করো:

       ```js
       function rateLimiter(windowMs, maxRequests) {
         const clients = new Map();

         return (req, res, next) => {
           const ip = req.ip || req.socket.remoteAddress;
           const now = Date.now();
           const windowStart = now - windowMs;

           if (!clients.has(ip)) {
             clients.set(ip, []);
           }

           const timestamps = clients.get(ip).filter((t) => t > windowStart);
           clients.set(ip, timestamps);

           if (timestamps.length >= maxRequests) {
             res.writeHead(429, {
               'Retry-After': Math.ceil(windowMs / 1000),
               'X-RateLimit-Limit': maxRequests,
               'X-RateLimit-Remaining': 0,
             });
             return res.end(JSON.stringify({ error: 'Too many requests' }));
           }

           timestamps.push(now);
           res.setHeader('X-RateLimit-Limit', maxRequests);
           res.setHeader('X-RateLimit-Remaining', maxRequests - timestamps.length);
           next();
         };
       }
       ```

    2. Token Bucket algorithm implement করো
    3. Redis-based distributed rate limiter build করো (Phase 09 preview)

### Task 22 — API Documentation (OpenAPI/Swagger) 🟢

- API document করো properly:
  - **OpenAPI Specification (Swagger):**
    ```yaml
    openapi: 3.0.0
    info:
      title: User API
      version: 1.0.0
    paths:
      /api/users:
        get:
          summary: List all users
          parameters:
            - name: page
              in: query
              schema: { type: integer, default: 1 }
            - name: limit
              in: query
              schema: { type: integer, default: 20 }
          responses:
            '200':
              description: Success
              content:
                application/json:
                  schema:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
        post:
          summary: Create user
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  $ref: '#/components/schemas/CreateUser'
          responses:
            '201':
              description: Created
    components:
      schemas:
        User:
          type: object
          properties:
            id: { type: integer }
            name: { type: string }
            email: { type: string, format: email }
    ```
  - **Exercise:**
    1. Complete OpenAPI spec লেখো তোমার API এর জন্য
    2. Swagger UI setup করো
    3. Postman collection generate করো from OpenAPI spec

### Task 23 — Request Validation & Error Handling 🟡

- Input validation — security ও reliability:
  - **Never trust client input!**
  - **Validation Library (build your own):**

    ```js
    class Validator {
      constructor(data) {
        this.data = data;
        this.errors = [];
      }

      required(field) {
        if (!this.data[field] && this.data[field] !== 0) {
          this.errors.push({ field, message: `${field} is required` });
        }
        return this;
      }

      string(field, { min, max } = {}) {
        const value = this.data[field];
        if (value !== undefined) {
          if (typeof value !== 'string') {
            this.errors.push({ field, message: `${field} must be a string` });
          } else {
            if (min && value.length < min) {
              this.errors.push({ field, message: `${field} must be at least ${min} chars` });
            }
            if (max && value.length > max) {
              this.errors.push({ field, message: `${field} must be at most ${max} chars` });
            }
          }
        }
        return this;
      }

      email(field) {
        const value = this.data[field];
        if (value && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
          this.errors.push({ field, message: `${field} must be a valid email` });
        }
        return this;
      }

      isValid() {
        return this.errors.length === 0;
      }
    }

    // Usage
    const v = new Validator(req.body)
      .required('name')
      .string('name', { min: 2, max: 50 })
      .required('email')
      .email('email')
      .required('password')
      .string('password', { min: 8 });

    if (!v.isValid()) {
      return res.status(400).json({ errors: v.errors });
    }
    ```

  - **Error Response Format (consistent):**
    ```json
    {
      "success": false,
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid request data",
        "details": [
          { "field": "email", "message": "Must be a valid email" },
          { "field": "password", "message": "Must be at least 8 characters" }
        ]
      },
      "requestId": "req-uuid-123"
    }
    ```
  - **Exercise:**
    1. Full validation library build করো (string, number, email, date, array, object, custom)
    2. Global error handler middleware build করো
    3. Custom error classes: `AppError`, `ValidationError`, `NotFoundError`, `AuthError`

### Task 24 — API Idempotency 🟡

- Same request multiple times = same result:
  - **Idempotent methods:** GET, PUT, DELETE, HEAD, OPTIONS
  - **NOT idempotent:** POST (multiple POSTs create multiple resources)
  - **Making POST idempotent (Idempotency Key):**

    ```js
    // Client sends unique key
    // POST /api/payments
    // Idempotency-Key: uuid-unique-key-123

    const idempotencyStore = new Map(); // Use Redis in production

    function idempotency(req, res, next) {
      const key = req.headers['idempotency-key'];
      if (!key) return next();

      if (idempotencyStore.has(key)) {
        const cached = idempotencyStore.get(key);
        res.writeHead(cached.status, cached.headers);
        return res.end(JSON.stringify(cached.body));
      }

      // Intercept response to cache it
      const originalJson = res.json.bind(res);
      res.json = (body) => {
        idempotencyStore.set(key, {
          status: res.statusCode,
          headers: res.getHeaders(),
          body,
        });
        originalJson(body);
      };

      next();
    }
    ```

  - **Why important:** Payment APIs, order creation (user clicks "Pay" twice)
  - **Exercise:** Implement idempotent payment endpoint

### Task 25 — Webhook Design & Implementation 🟡

- Server-to-server notifications:
  - **Webhook = server calls YOUR endpoint when event happens**

  ```
  Your Server ──── Register webhook URL ────→ GitHub
  GitHub      ──── POST to your URL ────────→ Your Server
               (when push/PR/issue happens)
  ```

  - **Webhook server:**

    ```js
    const crypto = require('crypto');

    app.post('/webhook/github', (req, res) => {
      // 1. Verify signature (IMPORTANT for security!)
      const signature = req.headers['x-hub-signature-256'];
      const hmac = crypto.createHmac('sha256', process.env.WEBHOOK_SECRET);
      const digest = 'sha256=' + hmac.update(JSON.stringify(req.body)).digest('hex');

      if (!crypto.timingSafeEqual(Buffer.from(signature), Buffer.from(digest))) {
        return res.status(401).json({ error: 'Invalid signature' });
      }

      // 2. Process event
      const event = req.headers['x-github-event'];
      const payload = req.body;

      switch (event) {
        case 'push':
          handlePush(payload);
          break;
        case 'pull_request':
          handlePR(payload);
          break;
      }

      // 3. Respond quickly (process async if heavy)
      res.status(200).json({ received: true });
    });
    ```

  - **Webhook best practices:**
    - Always verify signatures
    - Respond within 5 seconds (process async)
    - Implement retry logic (with exponential backoff)
    - Store events for replay
    - Idempotent event handling
  - **Exercise:**
    1. Webhook sender ও receiver build করো
    2. Signature verification implement করো
    3. Retry with exponential backoff implement করো

---

## Section D: Networking Tools & Debugging (Tasks 26-35)

### Task 26 — cURL Mastery 🟢

- Command-line HTTP client:

  ```bash
  # GET request
  curl http://localhost:3000/api/users

  # POST with JSON body
  curl -X POST http://localhost:3000/api/users \
    -H "Content-Type: application/json" \
    -d '{"name": "John", "email": "john@example.com"}'

  # With auth header
  curl -H "Authorization: Bearer token123" http://localhost:3000/api/me

  # See headers
  curl -v http://localhost:3000/api/users

  # Only headers (no body)
  curl -I http://localhost:3000/api/users

  # Follow redirects
  curl -L http://example.com

  # Upload file
  curl -X POST -F "file=@photo.jpg" http://localhost:3000/upload

  # Save response
  curl -o response.json http://localhost:3000/api/users

  # With cookies
  curl -b "session=abc123" http://localhost:3000/api/me
  ```

  - **Exercise:** সব HTTP method test করো curl দিয়ে

### Task 27 — Postman / Insomnia API Testing 🟢

- API testing tools:
  - **Collections:** organize related requests
  - **Environments:** dev, staging, production variables
  - **Pre-request scripts:** dynamic data generation
  - **Tests:** response validation
  - **Exercise:**
    1. Complete collection build করো তোমার API এর জন্য
    2. Environment variables use করো (base URL, tokens)
    3. Automated test scripts লেখো
    4. Collection runner দিয়ে batch test করো

### Task 28 — Wireshark & Network Packet Analysis 🟡

- Network traffic capture ও analysis:
  - **Filter expressions:**
    ```
    http                     — HTTP traffic only
    tcp.port == 3000         — Port 3000 traffic
    ip.addr == 192.168.1.1   — Specific IP
    http.request.method == POST — POST requests only
    tcp.flags.syn == 1       — TCP SYN packets (new connections)
    ```
  - **Exercise:**
    1. HTTP request/response full lifecycle capture করো
    2. TCP handshake identify করো
    3. TLS handshake analyze করো
    4. Find: headers, body, status code in raw packets

### Task 29 — Network Debugging with Node.js 🟢

- Debugging tools:

  ```js
  // DNS lookup timing
  const dns = require('dns');
  const { performance } = require('perf_hooks');

  const start = performance.now();
  dns.lookup('google.com', (err, address) => {
    console.log(`DNS: ${(performance.now() - start).toFixed(2)}ms → ${address}`);
  });

  // HTTP request timing
  const http = require('http');
  const req = http.get('http://example.com', (res) => {
    const timing = {
      dns: 0,
      connect: 0,
      ttfb: 0,
      total: 0,
    };
    // ... measure each phase
  });

  // Connection pool info
  const agent = new http.Agent({ maxSockets: 10 });
  console.log('Free sockets:', agent.freeSockets);
  console.log('Active requests:', agent.requests);
  ```

  - **Exercise:** HTTP timing tool build করো (DNS, TCP connect, TLS, TTFB, download)

### Task 30 — Content Negotiation 🟢

- Server responds in client's preferred format:

  ```js
  app.get('/api/users/:id', (req, res) => {
    const user = getUserById(req.params.id);
    const accept = req.headers.accept;

    if (accept.includes('application/xml')) {
      res.type('application/xml');
      res.send(`<user><name>${user.name}</name></user>`);
    } else if (accept.includes('text/csv')) {
      res.type('text/csv');
      res.send(`name,email\n${user.name},${user.email}`);
    } else {
      res.json(user); // Default: JSON
    }
  });
  ```

  - **Exercise:** API that responds in JSON, XML, CSV, YAML based on Accept header

### Task 31 — HTTP Caching Deep Dive 🟡

- Caching strategies:
  - **Cache-Control directives:**
    ```
    Cache-Control: public, max-age=3600     — Cache for 1 hour (CDN + browser)
    Cache-Control: private, max-age=300     — Cache 5 min (browser only)
    Cache-Control: no-cache                 — Must revalidate before using cache
    Cache-Control: no-store                 — Never cache (sensitive data)
    Cache-Control: stale-while-revalidate=60 — Serve stale, revalidate in background
    ```
  - **ETag (Entity Tag):**

    ```js
    // Server generates ETag
    const crypto = require('crypto');
    const data = JSON.stringify(users);
    const etag = crypto.createHash('md5').update(data).digest('hex');

    app.get('/api/users', (req, res) => {
      if (req.headers['if-none-match'] === etag) {
        return res.status(304).end(); // Not Modified (use cache!)
      }
      res.setHeader('ETag', etag);
      res.setHeader('Cache-Control', 'public, max-age=60');
      res.json(users);
    });
    ```

  - **Last-Modified:**

    ```js
    app.get('/api/users', (req, res) => {
      const lastModified = getLastModifiedDate();
      const ifModifiedSince = new Date(req.headers['if-modified-since']);

      if (ifModifiedSince >= lastModified) {
        return res.status(304).end();
      }

      res.setHeader('Last-Modified', lastModified.toUTCString());
      res.json(users);
    });
    ```

  - **Exercise:** Full caching system implement করো (ETag + Cache-Control + Last-Modified)

### Task 32 — Compression (gzip, Brotli, deflate) 🟢

- Response compression:

  ```js
  const zlib = require('zlib');

  function compression(req, res, next) {
    const acceptEncoding = req.headers['accept-encoding'] || '';
    const originalEnd = res.end.bind(res);

    res.end = (data) => {
      if (!data || data.length < 1024) return originalEnd(data); // Skip small responses

      if (acceptEncoding.includes('br')) {
        res.setHeader('Content-Encoding', 'br');
        zlib.brotliCompress(Buffer.from(data), (err, compressed) => {
          originalEnd(compressed);
        });
      } else if (acceptEncoding.includes('gzip')) {
        res.setHeader('Content-Encoding', 'gzip');
        zlib.gzip(Buffer.from(data), (err, compressed) => {
          originalEnd(compressed);
        });
      } else {
        originalEnd(data);
      }
    };
    next();
  }
  ```

  - **Brotli vs gzip:** Brotli = ~20% better compression, but slower to compress
  - **Exercise:** Compression middleware build করো + benchmark (with/without compression)

### Task 33 — File Upload & Download 🟡

- Multipart form data handling:

  ```js
  const http = require('http');
  const fs = require('fs');
  const path = require('path');

  http
    .createServer((req, res) => {
      if (req.method === 'POST' && req.url === '/upload') {
        const boundary = req.headers['content-type'].split('boundary=')[1];
        let body = Buffer.alloc(0);

        req.on('data', (chunk) => {
          body = Buffer.concat([body, chunk]);
        });

        req.on('end', () => {
          // Parse multipart body (simplified)
          const parts = body.toString().split(`--${boundary}`);
          parts.forEach((part) => {
            const filenameMatch = part.match(/filename="(.+?)"/);
            if (filenameMatch) {
              const filename = filenameMatch[1];
              const contentStart = part.indexOf('\r\n\r\n') + 4;
              const contentEnd = part.lastIndexOf('\r\n');
              const fileContent = part.slice(contentStart, contentEnd);

              // Sanitize filename to prevent path traversal
              const safeName = path.basename(filename);
              fs.writeFileSync(path.join('uploads', safeName), fileContent);
            }
          });
          res.writeHead(200);
          res.end('Uploaded!');
        });
      }
    })
    .listen(3000);
  ```

  - **File download with streaming:**

    ```js
    app.get('/download/:filename', (req, res) => {
      const safeName = path.basename(req.params.filename); // Prevent path traversal
      const filePath = path.join(__dirname, 'uploads', safeName);

      if (!fs.existsSync(filePath)) {
        return res.status(404).json({ error: 'File not found' });
      }

      const stat = fs.statSync(filePath);
      res.setHeader('Content-Length', stat.size);
      res.setHeader('Content-Disposition', `attachment; filename="${safeName}"`);

      const stream = fs.createReadStream(filePath);
      stream.pipe(res);
    });
    ```

  - **Security:**
    - Sanitize filenames (prevent path traversal: `../../etc/passwd`)
    - Validate file type (check magic bytes, not just extension)
    - Limit file size
    - Scan for malware (in production)
  - **Exercise:** Complete file upload system (upload, download, delete, list, size limit, type validation)

### Task 34 — Streaming Responses 🟡

- Send data in chunks (don't buffer entire response):

  ```js
  app.get('/api/export', (req, res) => {
    res.setHeader('Content-Type', 'application/json');
    res.setHeader('Transfer-Encoding', 'chunked');

    res.write('[\n'); // Start JSON array

    let first = true;
    const cursor = db.collection('users').find().cursor();

    cursor.on('data', (doc) => {
      if (!first) res.write(',\n');
      res.write(JSON.stringify(doc));
      first = false;
    });

    cursor.on('end', () => {
      res.write('\n]');
      res.end();
    });
  });
  ```

  - **NDJSON (Newline Delimited JSON):**
    ```js
    res.setHeader('Content-Type', 'application/x-ndjson');
    cursor.on('data', (doc) => {
      res.write(JSON.stringify(doc) + '\n');
    });
    ```
  - **Exercise:** Export 1 million rows as streaming JSON/CSV (memory efficient)

### Task 35 — Build: Complete HTTP Framework from Scratch ⚫

- **Phase 01 Final Project:**
  - Build your own mini HTTP framework (like Express, but simpler):

    ```js
    const framework = require('./my-framework');
    const app = framework();

    // Middleware
    app.use(framework.json()); // JSON body parser
    app.use(framework.cors()); // CORS
    app.use(framework.compression()); // Gzip
    app.use(framework.rateLimit({ windowMs: 60000, max: 100 }));

    // Routes
    app.get('/api/users', (req, res) => {
      res.json([{ id: 1, name: 'John' }]);
    });

    app.post('/api/users', (req, res) => {
      res.status(201).json({ id: 2, ...req.body });
    });

    app.get('/api/users/:id', (req, res) => {
      res.json({ id: req.params.id });
    });

    // Error handler
    app.use((err, req, res, next) => {
      res.status(500).json({ error: err.message });
    });

    app.listen(3000, () => console.log('Server on 3000'));
    ```

  - **Features to implement:**
    - Routing (GET, POST, PUT, PATCH, DELETE)
    - Route params (`:id`)
    - Query string parsing
    - JSON body parsing
    - Middleware chain (use, route-level)
    - Error handling middleware
    - Static file serving
    - CORS middleware
    - Compression middleware
    - Rate limiting middleware
    - Cookie parsing
    - Response helpers (json, send, status, redirect)
  - **This is YOUR Express.js — you understand HTTP completely now**

---

## ✅ Phase 01 Completion Checklist

- [ ] সব 35 tasks complete
- [ ] TCP/IP, UDP, DNS explain করতে পারো
- [ ] Raw HTTP server build করতে পারো (no framework)
- [ ] WebSocket protocol বুঝো + implement করতে পারো
- [ ] CORS, Cookies, Caching সব manually handle করতে পারো
- [ ] API design principles জানো (REST, GraphQL, gRPC)
- [ ] নিজে HTTP framework build করেছো
- [ ] Wireshark দিয়ে network debug করতে পারো
- [ ] Load balancer build করতে পারো

> _"Express.js install করার আগে তুমি নিজে Express build করেছো।"_
> _"এখন যেকোনো backend framework দেখলে বুঝবে — এটা internally কী করছে।"_
