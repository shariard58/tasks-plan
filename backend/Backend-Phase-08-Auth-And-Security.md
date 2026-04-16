# Backend Phase 08 — Authentication, Authorization & Backend Security (Tasks 251-285)

> **Security skip করলে production এ hack হবে। এটা optional না — এটা MUST।**
> **Auth flow, encryption, OWASP Top 10, penetration testing — সব জানবে।**
> **Secure backend build করতে পারলে তুমি instantly 10x more valuable।**

---

## Section A: Authentication (Tasks 251-262)

### Task 251 — Authentication vs Authorization 🟢

- **Authentication:** Who are you? (login, verify identity)
- **Authorization:** What can you do? (permissions, access control)
- **Exercise:** Document the flow for both in your application

### Task 252 — Password Hashing (bcrypt, argon2, scrypt) 🟡

- Never store plain text passwords! Salt + hash
- bcrypt (most common), argon2 (recommended), scrypt
- **Exercise:** Password hashing module: hash, verify, migration between algorithms

### Task 253 — Session-Based Authentication 🟡

- Server-side sessions with cookies: session ID → server lookup → user data
- Session store: memory, Redis, database
- **Exercise:** Complete session auth system (login, logout, session management, Redis store)

### Task 254 — JWT (JSON Web Token) Authentication 🔴

- Structure: Header.Payload.Signature, Access token + Refresh token
- Token rotation, revocation, blacklisting
- **Exercise:** Complete JWT auth: register, login, refresh, logout, token blacklist

### Task 255 — OAuth 2.0 🔴

- Authorization Code flow, PKCE, Client Credentials, Implicit (deprecated)
- Implement "Login with Google/GitHub"
- **Exercise:** OAuth 2.0 server (authorization code + PKCE) ও client

### Task 256 — OpenID Connect (OIDC) 🟡

- Identity layer on top of OAuth 2.0, ID tokens, UserInfo endpoint
- **Exercise:** OIDC provider integration

### Task 257 — Multi-Factor Authentication (MFA) 🟡

- TOTP (Time-based OTP — Google Authenticator), SMS, Email, WebAuthn
- **Exercise:** TOTP-based 2FA implementation

### Task 258 — Passwordless Authentication 🟡

- Magic links (email), WebAuthn (biometric/security key), Passkeys
- **Exercise:** Magic link authentication system

### Task 259 — API Key Authentication 🟢

- API key generation, storage (hashed), rate limiting per key, key rotation
- **Exercise:** API key management system (create, rotate, revoke, rate limit)

### Task 260 — Single Sign-On (SSO) 🟡

- SAML 2.0, centralized authentication server
- **Exercise:** SSO system for multiple applications

### Task 261 — Session Fixation & Hijacking Prevention 🟡

- Regenerate session ID after login, HttpOnly/Secure cookies, IP validation
- **Exercise:** Implement all session security measures

### Task 262 — Token Storage Security 🟡

- Access token in memory, refresh token in HttpOnly cookie
- Never store tokens in localStorage (XSS vulnerable)
- **Exercise:** Secure token storage ও rotation flow

---

## Section B: Authorization (Tasks 263-270)

### Task 263 — Role-Based Access Control (RBAC) 🟡

- Users → Roles → Permissions
- **Exercise:** RBAC system (admin, moderator, user roles with granular permissions)

### Task 264 — Attribute-Based Access Control (ABAC) 🟡

- Policy-based: subject attributes + resource attributes + environment → allow/deny
- **Exercise:** ABAC system for document access (owner, department, classification)

### Task 265 — Permission Middleware 🟡

- Route-level ও resource-level authorization:
  ```js
  function authorize(...permissions) {
    return (req, res, next) => {
      const userPermissions = req.user.role.permissions;
      const hasPermission = permissions.every((p) => userPermissions.includes(p));
      if (!hasPermission) throw new ForbiddenError();
      next();
    };
  }
  app.delete('/api/users/:id', authorize('users:delete'), deleteUser);
  ```
- **Exercise:** Complete permission system with middleware

### Task 266 — Resource Ownership 🟢

- Users can only access their own resources
- Admin can access all resources
- **Exercise:** Ownership checks for CRUD operations

### Task 267 — API Scopes (OAuth 2.0) 🟡

- Limit what API tokens can do (read, write, admin)
- **Exercise:** Scoped API tokens (read-only, read-write, admin)

### Task 268 — Row-Level Security 🟡

- Database-level authorization (PostgreSQL RLS from Phase 06)
- **Exercise:** Multi-tenant authorization at database level

### Task 269 — Audit Logging 🟡

- Track: who did what, when, from where
- **Exercise:** Complete audit trail system (user actions, admin actions, data changes)

### Task 270 — Authorization Testing 🟡

- Test every endpoint for unauthorized access
- **Exercise:** Authorization test suite (positive ও negative test cases)

---

## Section C: Security (OWASP & Beyond) (Tasks 271-285)

### Task 271 — OWASP Top 10 (2021) 🔴

- A01: Broken Access Control — A02: Cryptographic Failures — A03: Injection
- A04: Insecure Design — A05: Security Misconfiguration — A06: Vulnerable Components
- A07: Auth Failures — A08: Software/Data Integrity — A09: Logging Failures — A10: SSRF
- **Exercise:** Audit your app against all 10 categories

### Task 272 — SQL Injection Prevention 🟡

- Always use parameterized queries! Never string concatenation
- **Exercise:** Demonstrate SQL injection attack → fix it

### Task 273 — XSS Prevention (Backend) 🟡

- Input sanitization, output encoding, Content-Security-Policy header
- **Exercise:** Prevent stored XSS, reflected XSS from backend

### Task 274 — CSRF Prevention 🟡

- CSRF tokens, SameSite cookies, double submit cookie
- **Exercise:** CSRF protection middleware

### Task 275 — Security Headers 🟡

- Content-Security-Policy, X-Content-Type-Options, X-Frame-Options
- Strict-Transport-Security, Referrer-Policy, Permissions-Policy
- **Exercise:** Security headers middleware (your own Helmet.js)

### Task 276 — Input Validation & Sanitization 🟡

- Whitelist validation, type checking, length limits, regex patterns
- Sanitize HTML, prevent NoSQL injection, validate file uploads
- **Exercise:** Comprehensive input validation library

### Task 277 — Rate Limiting & DDoS Protection 🟡

- Per-IP, per-user, per-endpoint rate limiting
- Slowloris protection, request size limits
- **Exercise:** Multi-layer rate limiting system

### Task 278 — Encryption at Rest & Transit 🟡

- TLS for transit, AES-256 for sensitive fields (PII, payment data)
- Key management, key rotation
- **Exercise:** Encrypt sensitive database fields (email, phone, SSN)

### Task 279 — Secrets Management 🟡

- Environment variables, HashiCorp Vault, AWS Secrets Manager
- Never commit secrets to git!
- **Exercise:** Secrets management system (load from vault, rotate)

### Task 280 — Dependency Security 🟢

- npm audit, Snyk, Dependabot, lockfile integrity
- **Exercise:** Audit all dependencies, fix vulnerabilities, setup automated scanning

### Task 281 — CORS Security 🟢

- Proper CORS configuration (not `origin: *` in production!)
- **Exercise:** Strict CORS policy for production

### Task 282 — File Upload Security 🟡

- Type validation (magic bytes), size limits, filename sanitization, virus scanning
- Store outside web root, serve through API (never direct file access)
- **Exercise:** Secure file upload system

### Task 283 — Server-Side Request Forgery (SSRF) Prevention 🟡

- Validate URLs, block internal IPs, allowlist domains
- **Exercise:** URL validator that prevents SSRF

### Task 284 — Penetration Testing Basics 🔴

- OWASP ZAP, Burp Suite, manual testing methodology
- **Exercise:** Pen test your own application, document findings, fix vulnerabilities

### Task 285 — Build: Secure Authentication System ⚫

- **Phase 08 Final Project:**
  - Registration (email verification), Login (JWT + refresh tokens)
  - MFA (TOTP), Password reset (secure tokens)
  - OAuth 2.0 (Google, GitHub), RBAC (admin, user, moderator)
  - Session management, Account lockout, Brute force protection
  - Full audit trail, Security headers, Rate limiting
  - Pen tested ও all OWASP Top 10 addressed

---

## ✅ Phase 08 Completion Checklist

- [ ] সব 35 tasks complete
- [ ] JWT ও Session auth both implement করতে পারো
- [ ] OAuth 2.0 flow explain ও implement করতে পারো
- [ ] RBAC ও ABAC authorization systems build করতে পারো
- [ ] OWASP Top 10 সব address করতে পারো
- [ ] Penetration testing basics জানো

> _"Secure backend = employable backend developer। Hack হওয়া app = career disaster।"_
