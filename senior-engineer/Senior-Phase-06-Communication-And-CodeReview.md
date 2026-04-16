# Senior Skills Phase 06 — Technical Communication & Code Review (Tasks 191-215)

> **Senior dev এর #1 daily task = Code Review। Code লেখার চেয়ে বেশি সময় review এ যায়।**
> **Technical writing পারলে তুমি thought leader। Blog, docs, RFC — সব লিখতে পারবে।**
> **Communication skill না থাকলে তুমি senior হতে পারবে না — regardless of technical skill।**

---

## Section A: Technical Writing (Tasks 191-200)

### Task 191 — README Writing 🟢

- Good README: What, Why, How, Installation, Usage, API, Contributing, License
- Badges, Screenshots, GIFs, Architecture diagrams
- **Exercise:** 5 টা project এর README লেখো (world-class quality)

### Task 192 — API Documentation 🟡

- OpenAPI/Swagger spec, Examples, Error codes, Rate limits
- Auto-generated docs from code comments
- **Exercise:** Complete API documentation for an API (interactive playground সহ)

### Task 193 — Architecture Decision Records (ADR) 🟡

- Document WHY a technical decision was made
- Format: Context → Decision → Consequences → Status
- ```markdown
  # ADR-001: Use PostgreSQL over MySQL

  ## Context

  We need a relational database for our SaaS product...

  ## Decision

  We will use PostgreSQL because...

  ## Consequences

  Positive: JSONB support, better performance for complex queries
  Negative: Team needs PostgreSQL training
  ```
- **Exercise:** 10 টা ADR লেখো real project decisions এর জন্য

### Task 194 — Technical Blog Writing 🟡

- Structure: Problem → Context → Solution → Code → Conclusion
- Clear writing, code examples, diagrams
- Publish on: Dev.to, Medium, Hashnode, personal blog
- **Exercise:** 3 টা technical blog post লেখো ও publish করো

### Task 195 — RFC (Request for Comments) 🟡

- Propose significant technical changes to team
- Format: Problem → Proposal → Alternatives → Impact → Timeline
- **Exercise:** RFC লেখো: "Migrate from REST to GraphQL" বা "Adopt TypeScript"

### Task 196 — Incident Post-Mortems 🟡

- What happened → Timeline → Root cause → Impact → What we learned → Action items
- Blameless culture: focus on system, not people
- **Exercise:** 3 টা hypothetical incident post-mortem লেখো

### Task 197 — Technical Specifications 🟡

- Feature spec: requirements, API design, database schema, edge cases
- **Exercise:** Technical spec for a feature (e.g., notification system)

### Task 198 — Runbooks ও Playbooks 🟡

- Step-by-step guides for operations: deployment, incident response, scaling
- **Exercise:** Runbook: "How to deploy", "How to handle database incident"

### Task 199 — Diagram Drawing 🟡

- Architecture diagrams, Sequence diagrams, ER diagrams, Flow charts
- Tools: draw.io, Mermaid (markdown), Excalidraw
- ```mermaid
  sequenceDiagram
    Client->>API Gateway: POST /orders
    API Gateway->>Order Service: Create Order
    Order Service->>Payment Service: Charge
    Payment Service-->>Order Service: Success
    Order Service->>Queue: Order Created Event
    Queue->>Notification Service: Send Confirmation
  ```
- **Exercise:** 10 টা different diagram draw করো real systems এর

### Task 200 — Documentation as Code 🟡

- Docs in Git repo, versioned with code
- Docusaurus, VitePress, Nextra for doc sites
- **Exercise:** Documentation site build করো (auto-deployed on merge)

---

## Section B: Code Review (Tasks 201-210)

### Task 201 — Code Review Fundamentals 🟡

- Why code review: catch bugs, share knowledge, maintain quality
- What to review: logic, security, performance, readability, tests, edge cases
- **Exercise:** Code review checklist তৈরি করো (30+ items)

### Task 202 — Giving Constructive Feedback 🟡

- Be specific, suggest alternatives, explain WHY
- ```
  ❌ "This is wrong"
  ✅ "Consider using Map instead of Object here because Map preserves
     insertion order and has O(1) lookup. Here's an example: ..."

  ❌ "This code is messy"
  ✅ "This function does 3 things (validate, transform, save). Consider
     splitting into separate functions for better testability."
  ```

- Use "Nit:", "Consider:", "Question:", "Blocking:" prefixes
- **Exercise:** 10 টা open source PR review করো constructive feedback দিয়ে

### Task 203 — Security in Code Review 🟡

- SQL injection, XSS, CSRF, Auth bypass, Secrets in code
- **Exercise:** Security-focused review checklist ও practice

### Task 204 — Performance in Code Review 🟡

- N+1 queries, unnecessary re-renders, memory leaks, O(n²) loops
- **Exercise:** Performance-focused review checklist ও practice

### Task 205 — PR Description Writing 🟢

- What changed, Why, How to test, Screenshots, Related issues
- **Exercise:** PR template তৈরি করো, 10 টা PR description লেখো

### Task 206 — Automated Code Review 🟡

- ESLint, Prettier, TypeScript, SonarQube, CodeClimate
- GitHub Actions for auto-review checks
- **Exercise:** Automated review pipeline setup

### Task 207 — Review Speed ও Efficiency 🟡

- Small PRs (<400 lines), Review within 24 hours
- Batch comments, prioritize blocking issues
- **Exercise:** Track review time, optimize to < 30 min average

### Task 208 — Receiving Code Review 🟢

- Don't take it personally, learn from feedback
- Respond to every comment, explain or update
- **Exercise:** Past feedback analysis: what did you learn?

### Task 209 — Pair Programming 🟡

- Driver (types) + Navigator (thinks)
- When to use: complex problems, onboarding, knowledge sharing
- **Exercise:** 5 pair programming sessions, document learnings

### Task 210 — Code Review Culture 🟡

- Establish team review standards, SLAs, escalation
- **Exercise:** Code review guide document for a team

---

## Section C: Communication Skills (Tasks 211-215)

### Task 211 — Explaining Complex Concepts Simply 🟡

- Feynman Technique: explain like teaching a beginner
- Analogies, diagrams, progressive disclosure
- **Exercise:** 10 টা complex topic (Event Loop, ACID, CAP) simply explain করো

### Task 212 — Technical Presentations 🟡

- Structure: Problem → Demo → Solution → Q&A
- Live coding in presentations, slides design
- **Exercise:** 15-minute technical talk prepare ও deliver করো

### Task 213 — Written Communication in Remote Teams 🟡

- Async communication: clear, concise, context-rich messages
- Slack/Discord etiquette, email formatting
- **Exercise:** 1 week async communication practice — every message clear ও actionable

### Task 214 — Stakeholder Communication 🟡

- Non-technical audience কে technical concepts explain করা
- Trade-off discussion: time vs quality vs scope
- **Exercise:** "Why we need to refactor" pitch to non-technical manager

### Task 215 — Build: Technical Content Portfolio ⚫

- **Phase 6 Final Project:**
  - 5 Published blog posts (Dev.to/Medium/Hashnode)
  - 10 ADRs for a project
  - 1 RFC document
  - Complete API documentation for a project
  - 1 Incident post-mortem
  - Architecture diagrams for 3 systems
  - 1 Technical presentation (recorded)

---

## ✅ Phase 6 Completion Checklist

- [ ] সব 25 tasks complete
- [ ] Technical blog consistently লিখতে পারো
- [ ] Code review confidently ও constructively করতে পারো
- [ ] ADR, RFC, Spec documents লিখতে পারো
- [ ] Complex concepts simply explain করতে পারো
- [ ] Technical presentation deliver করতে পারো

> _"Senior dev = Technical skill + Communication skill। দুটো ছাড়া কেউ senior হয় না।"_
