---
name: prime
description: Load and internalize full project context by analyzing the codebase structure, documentation, key files, and current git state. Use this skill whenever the user wants to prime Claude on a project, load project context, get oriented in a codebase, start a deep work session, or needs Claude to understand the full picture before making changes. Also trigger when the user says "prime", "load context", "get up to speed", "understand this project", "orient yourself", "read the codebase", or wants Claude to build a mental model of the project before diving into work.
---

# Prime: Load Project Context

Build a comprehensive understanding of the current codebase so you can work effectively from the very first task. This is the "read before you write" step — investing a few minutes here prevents misguided suggestions for the rest of the session.

## Why prime?

Starting work on a codebase without context leads to generic suggestions that ignore the project's conventions, architecture, and current state. Priming loads all of that into your working memory so every subsequent response is grounded in how this specific project actually works.

## Process

Work through these steps in order. Parallelize where possible — steps 1 and 2 can run concurrently since they're independent reads.

---

### Step 1: Analyze Project Structure

Get the lay of the land. Run these to understand what exists and how it's organized:

**List all tracked files:**
```
git ls-files
```

**Show directory structure** (up to 3 levels deep, excluding noise):
```
# macOS/Linux
find . -maxdepth 3 -type d \
  ! -path '*/node_modules/*' \
  ! -path '*/.git/*' \
  ! -path '*/dist/*' \
  ! -path '*/build/*' \
  ! -path '*/__pycache__/*' \
  ! -path '*/.next/*' \
  ! -path '*/target/*' \
  ! -path '*/.venv/*' \
  ! -path '*/vendor/*' \
  | head -80
```

If `tree` is available, prefer:
```
tree -L 3 -d -I 'node_modules|__pycache__|.git|dist|build|.next|target|.venv|vendor'
```

From this, note:
- How the project is organized (monorepo? flat? feature-based? layer-based?)
- Where source code, tests, config, and docs live
- Any unexpected or notable directories

---

### Step 2: Read Core Documentation

Read whatever project documentation exists, in priority order:

1. **CLAUDE.md** — Project rules and conventions (highest priority — this is context written specifically for you)
2. **README.md** — Project overview, setup instructions, purpose
3. **PRD.md** or spec files — Product requirements, feature specs
4. **Architecture docs** — `ARCHITECTURE.md`, `docs/architecture.md`, `docs/design.md`, or similar
5. **Contributing guide** — `CONTRIBUTING.md`, coding standards
6. **Changelog** — `CHANGELOG.md` for recent evolution

Don't force it — read what exists. If none of these files are present, that's fine — you'll learn from the code itself.

---

### Step 3: Read Key Configuration

Read the project's configuration files to understand the tech stack and tooling:

- **Package/dependency config**: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `build.gradle`, `pom.xml`
- **Language config**: `tsconfig.json`, `.python-version`, `rust-toolchain.toml`
- **Build config**: `vite.config.*`, `next.config.*`, `webpack.config.*`, `Makefile`
- **Lint/format config**: `.eslintrc.*`, `.prettierrc`, `ruff.toml`, `.rubocop.yml`
- **Database config**: `drizzle.config.*`, `prisma/schema.prisma`, `alembic.ini`, migration files
- **CI/CD**: `.github/workflows/*.yml`, `.gitlab-ci.yml`, `Jenkinsfile`
- **Environment**: `.env.example`, `.env.local.example` (never read actual `.env` files)

Only read what's present. Extract: languages, frameworks, versions, available scripts/commands, and tooling choices.

---

### Step 4: Identify and Read Key Source Files

Based on what you learned in steps 1-3, identify and read the most important source files. Focus on:

- **Entry points** — `main.py`, `index.ts`, `app.py`, `main.go`, `src/lib.rs`, `App.tsx`, etc.
- **Schema/model definitions** — Database schemas, type definitions, API schemas, Protobuf files
- **Core business logic** — The files that implement the project's primary functionality
- **Shared utilities** — Helper functions, common patterns used across the codebase
- **Route/API definitions** — How endpoints or pages are structured
- **Configuration/constants** — App-level settings, feature flags, enums

Read 5-10 key files, not the entire codebase. Prioritize files that reveal patterns and architecture over files that are just implementation details.

---

### Step 5: Understand Current State

Check what's been happening recently:

**Recent commits:**
```
git log -10 --oneline
```

**Current branch and working state:**
```
git status
```

**Recent branches** (to understand active work streams):
```
git branch --sort=-committerdate | head -10
```

From this, note:
- What branch you're on and whether there's uncommitted work
- What the recent development focus has been
- Whether there are any in-progress features or fixes

---

## Output Report

After completing all steps, provide a concise summary. This report serves two purposes: it confirms to the user that you've loaded the right context, and it crystallizes your understanding so you can reference it throughout the session.

```markdown
## Project Context Loaded

### Project Overview
- What this project is and what it does
- Primary purpose and target users

### Tech Stack
- Languages and runtimes (with versions if notable)
- Frameworks and major libraries
- Database and ORM (if any)
- Build tools and package manager
- Testing framework
- Linting/formatting tools

### Architecture
- Overall structure and organization pattern
- Key architectural decisions
- Important directories and their purposes
- Data flow (if applicable)

### Patterns and Conventions
- Naming conventions observed
- Code organization patterns
- Error handling approach
- Testing patterns
- Import style

### Current State
- Active branch: {branch name}
- Working tree: {clean / has uncommitted changes}
- Recent focus: {what recent commits suggest}
- Notable: {anything that stands out — TODOs, WIP, potential issues}

### Key Files
- {file path} — {what it does and why it matters}
- (list 5-10 most important files)
```

Keep the report scannable — bullet points and clear headers. The user should be able to glance at it and confirm "yes, Claude understands my project" in under 30 seconds.

---

## Adapting to Project Size

- **Small projects** (< 50 files): You can likely read most of the source code. Be thorough.
- **Medium projects** (50-500 files): Read entry points, schemas, and a representative sample from each major directory. Focus on understanding the patterns.
- **Large projects** (500+ files): Focus on the top-level structure, documentation, and configuration. Read key files from the areas most relevant to the user's likely tasks. Don't try to read everything — understand the architecture and conventions, then dive deeper as needed during actual tasks.
- **Monorepos**: Start with the root config and docs, then ask the user which package(s) they'll be working in. Prime deeply on those, lightly on the rest.
