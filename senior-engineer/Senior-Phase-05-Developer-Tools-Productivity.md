# Senior Skills Phase 05 — Developer Tools & Productivity (Tasks 166-190)

> **ভালো developer = ভালো tools use করে। Tools master হলে productivity 5x বাড়বে।**
> **VS Code shortcuts, Terminal tricks, Monorepo tools, Linting — এগুলো daily use হবে।**
> **10 মিনিটের কাজ 2 মিনিটে করবে — just because তুমি tools জানো।**

---

## Section A: IDE & Editor Mastery (Tasks 166-175)

### Task 166 — VS Code Power User 🟢

- Keyboard shortcuts: 50+ essential shortcuts memorize করো
- Multi-cursor editing, Find ও Replace (regex), Command Palette
- **Exercise:** VS Code shortcuts cheat sheet বানাও, 1 সপ্তাহ mouse ছাড়া code করো

### Task 167 — VS Code Extensions 🟢

- Essential: ESLint, Prettier, GitLens, Error Lens, Thunder Client
- Productivity: GitHub Copilot, Todo Tree, Bookmarks, Project Manager
- Language: TypeScript, Prisma, Tailwind CSS IntelliSense
- **Exercise:** Perfect VS Code setup: extensions + settings.json + keybindings.json

### Task 168 — VS Code Debugging 🟡

- Breakpoints, Watch, Call Stack, Variables
- launch.json for Node.js, React, Tests
- Conditional breakpoints, Logpoints
- **Exercise:** Debug a complex bug using VS Code debugger (not console.log!)

### Task 169 — Terminal Mastery 🟡

- Bash/Zsh: aliases, functions, scripts
- Oh My Zsh, Starship prompt, zsh plugins
- ```bash
  # Useful aliases
  alias gs="git status"
  alias gc="git commit -m"
  alias dev="npm run dev"
  alias test="npm test"

  # Functions
  mkcd() { mkdir -p "$1" && cd "$1"; }
  ```

- **Exercise:** Terminal customization: 30+ aliases, 10+ functions, custom prompt

### Task 170 — Vim Basics 🟢

- Server এ file edit করতে Vim লাগবে (VS Code নেই server এ!)
- Normal mode, Insert mode, Visual mode
- Essential: `i`, `Esc`, `:wq`, `:q!`, `dd`, `yy`, `p`, `/search`, `:s/old/new/g`
- **Exercise:** Vim দিয়ে config file edit করো comfortably

### Task 171 — Remote Development 🟡

- VS Code Remote SSH, Dev Containers, GitHub Codespaces
- **Exercise:** Remote server এ VS Code দিয়ে develop করো

### Task 172 — Snippets ও Code Generation 🟢

- VS Code snippets, Emmet, custom snippet files
- **Exercise:** 20 টা custom snippet for Node.js/TypeScript/React

### Task 173 — Workspace Settings ও Multi-Root Workspace 🟢

- Per-project settings, Recommended extensions
- `.vscode/settings.json`, `.vscode/extensions.json`
- **Exercise:** Team-shared VS Code config setup

### Task 174 — AI-Assisted Development 🟡

- GitHub Copilot: effective use, prompt patterns
- Copilot Chat: code explanation, refactoring, test generation
- **Exercise:** Copilot effectively use করে development speed 2x করো

### Task 175 — Browser DevTools for Backend 🟡

- Network tab (API debugging), Performance profiling
- Node.js Inspector in Chrome DevTools
- **Exercise:** Debug API performance issue using Chrome DevTools

---

## Section B: Development Workflow (Tasks 176-185)

### Task 176 — Package Managers Deep Dive 🟡

- npm vs yarn vs pnpm
- `package.json` deep dive: scripts, engines, exports, workspaces
- Lock files: why they matter, how they work
- **Exercise:** pnpm workspace setup with 3 packages

### Task 177 — Monorepo Tools 🟡

- **Turborepo:** build caching, parallel execution, task pipelines
- **Nx:** project graph, affected commands, plugins
- **Exercise:** Turborepo monorepo: shared packages, apps, parallel builds

### Task 178 — ESLint Advanced Configuration 🟡

- Custom rules, plugins, shared configs
- @typescript-eslint, eslint-plugin-import
- Flat config (eslint.config.js)
- **Exercise:** Team ESLint config with 50+ rules

### Task 179 — Prettier ও Code Formatting 🟢

- Prettier config, ESLint + Prettier integration
- `.prettierrc`, `.prettierignore`
- **Exercise:** Prettier + ESLint conflict-free setup

### Task 180 — Pre-commit Hooks (Husky + lint-staged) 🟡

- Auto-lint ও format on commit
- ```json
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
  ```
- **Exercise:** Pre-commit: lint + format + type-check + test

### Task 181 — Bundlers ও Build Tools 🟡

- Webpack, Vite, esbuild, SWC, Rollup — কখন কোনটা
- **Exercise:** Build tool comparison benchmark

### Task 182 — Documentation Tools 🟡

- JSDoc/TSDoc for code documentation
- Typedoc for API docs auto-generation
- **Exercise:** Auto-generated API documentation for a library

### Task 183 — API Testing Tools 🟢

- Postman collections, Thunder Client, HTTPie, curl advanced
- **Exercise:** Postman collection with tests ও environment variables

### Task 184 — Database Tools 🟢

- pgAdmin, DBeaver, MongoDB Compass, Redis Insight
- **Exercise:** Database management workflow setup

### Task 185 — Docker Desktop & Tools 🟢

- Docker Desktop, Lazydocker (TUI), Docker VS Code extension
- **Exercise:** Docker workflow optimization

---

## Section C: Productivity & Automation (Tasks 186-190)

### Task 186 — Time Management for Developers 🟢

- Pomodoro technique, Deep work, Flow state
- Time tracking tools, Focus techniques
- **Exercise:** 1 week Pomodoro technique follow করো, productivity measure করো

### Task 187 — Task Estimation 🟡

- Story points, T-shirt sizing, Planning Poker
- Why developers underestimate: multiply your estimate by 2-3x
- **Exercise:** 10 টা feature estimate করো, actual time compare করো

### Task 188 — Automation Scripts 🟡

- Shell scripts, Node.js scripts for repetitive tasks
- project setup, data migration, log analysis, report generation
- **Exercise:** 5 টা automation script বানাও for your daily workflow

### Task 189 — Personal Knowledge Management 🟢

- Note-taking system (Obsidian/Notion), Code snippets library
- Today I Learned (TIL) habit
- **Exercise:** Knowledge management system setup করো

### Task 190 — Build: Personal CLI Tool ⚫

- **Phase 5 Final Project:**
  - CLI tool (using Commander.js / Oclif):
    - Project scaffolding (create new project with your preferred config)
    - Git workflow automation (branch, commit, PR)
    - Code generation (model, route, test from template)
    - Database utilities (seed, migrate, backup)
    - Published on npm
    - Beautiful output (chalk, ora, inquirer)

---

## ✅ Phase 5 Completion Checklist

- [ ] সব 25 tasks complete
- [ ] VS Code power user (shortcuts, debugging, extensions)
- [ ] Terminal fluent (aliases, scripts, customization)
- [ ] Monorepo tools (Turborepo/Nx) use করতে পারো
- [ ] Linting ও formatting automated
- [ ] Personal CLI tool published

> _"Tools master = Fast developer। Fast developer = Valuable developer।"_
