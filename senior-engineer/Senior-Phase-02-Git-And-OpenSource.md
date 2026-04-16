# Senior Skills Phase 02 — Git, GitHub & Open Source (Tasks 51-80)

> **Git শুধু `add, commit, push` না। Advanced Git জানলে team এ কাজ 10x smooth হবে।**
> **Open Source contribute করলে real-world experience + portfolio + reputation পাবে।**
> **Senior devs Git master হয় — conflict resolve, rebase, bisect সব fluent।**

---

## Section A: Git Mastery (Tasks 51-65)

### Task 51 — Git Internals 🟡

- Git objects: blob, tree, commit, tag
- `.git` directory structure: HEAD, refs, objects, index
- How Git stores data (content-addressable storage)
- **Exercise:** Explore `.git/` folder, understand each file ও object

### Task 52 — Branching Strategies 🟡

- **Git Flow:** main, develop, feature/_, release/_, hotfix/\*
- **Trunk-Based Development:** main + short-lived feature branches
- **GitHub Flow:** main + feature branches + PR
- কখন কোনটা use করবে
- **Exercise:** Setup branching strategy for a team project, document rules

### Task 53 — Rebasing Deep Dive 🟡

- `git rebase` vs `git merge` — কখন কোনটা
- Interactive rebase: `git rebase -i` — squash, edit, reorder, drop commits
- ```bash
  git rebase -i HEAD~5   # Last 5 commits edit করো
  # pick, squash, edit, reword, drop
  ```
- **Exercise:** Messy commit history clean করো interactive rebase দিয়ে

### Task 54 — Merge Strategies ও Conflict Resolution 🟡

- Fast-forward, 3-way merge, recursive, octopus
- Conflict resolution: manual, tools (VS Code, kdiff3)
- **Exercise:** Complex merge conflict scenario create ও resolve করো

### Task 55 — Cherry-Pick ও Patch 🟡

- `git cherry-pick` — specific commit অন্য branch এ নাও
- `git format-patch` / `git apply` — patch files
- **Exercise:** Hotfix cherry-pick workflow practice

### Task 56 — Git Stash Advanced 🟢

- `git stash`, `git stash pop`, `git stash list`, `git stash apply stash@{2}`
- Partial stash: `git stash -p`
- **Exercise:** Multi-stash workflow management

### Task 57 — Git Bisect 🟡

- Binary search for the commit that introduced a bug
- ```bash
  git bisect start
  git bisect bad          # current commit is bad
  git bisect good v1.0    # v1.0 was good
  # Git checks out middle commit, you test, mark good/bad
  ```
- **Exercise:** 100 commits এ bug খোঁজো bisect দিয়ে

### Task 58 — Git Reflog 🟡

- Recovery tool: see ALL ref changes (even deleted branches/commits)
- ```bash
  git reflog              # see history
  git checkout HEAD@{5}   # go back to state 5 steps ago
  ```
- **Exercise:** "Accidentally" delete branch/reset, recover using reflog

### Task 59 — Git Hooks 🟡

- Pre-commit, commit-msg, pre-push hooks
- Husky + lint-staged for automated checks
- ```json
  // package.json
  "lint-staged": {
    "*.ts": ["eslint --fix", "prettier --write"]
  }
  ```
- **Exercise:** Setup: pre-commit (lint + format), commit-msg (conventional commits), pre-push (tests)

### Task 60 — Conventional Commits 🟢

- `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- Breaking changes: `feat!:` or `BREAKING CHANGE:` in footer
- Tools: commitlint, commitizen
- **Exercise:** Setup conventional commits with auto-validation

### Task 61 — Git Worktrees 🟡

- Multiple working directories from same repo
- ```bash
  git worktree add ../hotfix hotfix-branch  # New directory for hotfix
  ```
- **Exercise:** Simultaneous work on feature + hotfix using worktrees

### Task 62 — Git Submodules ও Subtrees 🟡

- Submodules: repo inside repo (shared libraries)
- Subtrees: merge another repo into subfolder
- **Exercise:** Shared library as submodule between 2 projects

### Task 63 — Monorepo with Git 🟡

- Sparse checkout, partial clone, shallow clone
- `.gitattributes`, large file management
- **Exercise:** Monorepo setup with efficient Git operations

### Task 64 — Git Performance ও Large Repos 🟡

- `git gc`, `git prune`, `git maintenance`
- Git LFS for large files
- Shallow clone: `git clone --depth 1`
- **Exercise:** Optimize a large Git repository

### Task 65 — Advanced Git Config 🟢

- Aliases, custom merge tools, global `.gitignore`
- `git config` per-repo, global, system
- **Exercise:** Productive Git config: 20+ aliases, tools, settings

---

## Section B: GitHub & Collaboration (Tasks 66-75)

### Task 66 — Pull Request Best Practices 🟡

- Small PRs (<400 lines), descriptive title, detailed description
- PR template, linked issues, screenshots/videos
- **Exercise:** Create PR template for team project

### Task 67 — Code Review Process 🟡

- How to give constructive review
- What to look for: logic, security, performance, readability, tests
- How to receive review: don't take it personally
- **Exercise:** Review 5 open source PRs, write thoughtful comments

### Task 68 — GitHub Actions Advanced 🟡

- Reusable workflows, composite actions, matrix builds
- Caching, artifacts, environments, secrets
- **Exercise:** CI/CD with: lint → type-check → test → build → deploy

### Task 69 — GitHub Features 🟢

- Issues (templates), Projects (kanban), Discussions, Wiki
- CODEOWNERS file, branch protection rules
- **Exercise:** Full GitHub project setup with all features

### Task 70 — GitHub Security 🟡

- Dependabot, CodeQL, Secret scanning
- Security advisories, vulnerability alerts
- **Exercise:** Setup all security features on a repository

### Task 71 — GitHub Packages & Releases 🟡

- Publish npm packages to GitHub Packages
- Semantic versioning, changelogs (auto-generated)
- **Exercise:** Automated release with changelog generation

### Task 72 — GitHub CLI (gh) 🟢

- `gh pr create`, `gh issue list`, `gh repo clone`
- **Exercise:** Common GitHub workflows using CLI only

### Task 73 — Git Workflow Automation 🟡

- Auto-label PRs, auto-assign reviewers, auto-merge
- Scheduled workflows, issue management bots
- **Exercise:** Automated project management with GitHub Actions

### Task 74 — Semantic Versioning (SemVer) 🟢

- MAJOR.MINOR.PATCH (1.2.3)
- When to bump each, pre-release versions (1.0.0-beta.1)
- **Exercise:** Version management strategy document

### Task 75 — Changelog ও Release Notes 🟢

- conventional-changelog, release-please
- Auto-generated from conventional commits
- **Exercise:** Automated changelog from commit history

---

## Section C: Open Source (Tasks 76-80)

### Task 76 — Finding Projects to Contribute 🟢

- Good first issues, "help wanted" labels
- Projects that match your skills (Node.js, React, TypeScript)
- **Exercise:** Find 10 repos you can contribute to, list issues

### Task 77 — Making Your First Contributions 🟡

- Fork → Branch → Code → Test → PR
- Follow CONTRIBUTING.md, code style, commit conventions
- **Exercise:** Submit 3 PRs to different open source projects

### Task 78 — Creating Your Own Open Source Project 🔴

- README, LICENSE, CONTRIBUTING.md, CODE_OF_CONDUCT.md
- GitHub templates (issue, PR), CI/CD, documentation
- **Exercise:** Publish a useful npm package (utility library/CLI tool)

### Task 79 — Building Open Source Reputation 🟡

- Consistent contributions, helpful issue comments
- Blog about open source work
- **Exercise:** 30-day open source contribution streak

### Task 80 — Build: npm Package Publication ⚫

- **Phase 2 Final Project:**
  - Useful TypeScript npm package (e.g., validation library, CLI tool, React hooks)
  - Features: TypeScript, 100% test coverage, CI/CD, auto-publish
  - Documentation: README with examples, API docs, changelog
  - GitHub: Issues templates, PR templates, CODEOWNERS, branch protection
  - Published on npm with semantic versioning
  - Get at least 10 GitHub stars (share on Twitter/Reddit/Dev.to)

---

## ✅ Phase 2 Completion Checklist

- [ ] সব 30 tasks complete
- [ ] Advanced Git operations fluent (rebase, bisect, cherry-pick, worktree)
- [ ] Team Git workflow setup ও manage করতে পারো
- [ ] GitHub Actions CI/CD confident
- [ ] Open source project contribute ও maintain করতে পারো
- [ ] npm package published

> _"Git master = team player। Open source = real-world experience। দুটো মিলে professional developer।"_
