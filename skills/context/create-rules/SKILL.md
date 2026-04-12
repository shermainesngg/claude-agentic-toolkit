---
name: create-rules
description: Generate a CLAUDE.md file by analyzing the codebase to extract project context, patterns, conventions, and key information. Use this skill whenever the user wants to create project rules, generate a CLAUDE.md, bootstrap Claude context for a repo, set up coding guidelines, or extract codebase conventions. Also trigger when the user says "create rules", "generate CLAUDE.md", "set up project context", "analyze this codebase for Claude", or wants to help Claude understand their project structure and patterns.
---

# Create Rules: Generate CLAUDE.md from Codebase Analysis

Analyze the current codebase and generate a `CLAUDE.md` file that gives Claude rich, actionable context about the project — what it is, how it's built, how the code is organized, and what conventions to follow.

## Why this matters

A well-crafted CLAUDE.md is the single highest-leverage file for improving Claude's effectiveness on a project. Without it, Claude has to rediscover the project's conventions, tech stack, and structure every conversation. With it, Claude starts each session already understanding the codebase — leading to fewer mistakes, better suggestions, and less time wasted on context-gathering.

## Phased Approach

Work through these four phases in order. Each phase builds on the previous one.

---

### Phase 1: DISCOVER

Figure out what kind of project this is and how it's laid out.

**Identify the project type** by looking at the root directory and config files:

| Type | Indicators |
|------|------------|
| Web App (Full-stack) | Separate client/server dirs, API routes, both frontend and backend code |
| Web App (Frontend) | React/Vue/Svelte/Angular, no server code |
| API/Backend | Express/FastAPI/Rails/Spring/etc, no frontend |
| Library/Package | `main`/`exports` in package.json, `lib` in Cargo.toml, publishable artifact |
| CLI Tool | `bin` in package.json, `console_scripts` in pyproject.toml, command-line interface |
| Monorepo | Multiple packages, workspaces config, turborepo/nx/lerna |
| Mobile App | React Native, Flutter, Swift/Kotlin project files |
| Data/ML | Jupyter notebooks, model files, training scripts |
| Infrastructure | Terraform, CloudFormation, Pulumi, Dockerfiles |
| Script/Automation | Standalone scripts, task-focused, no package structure |

**Analyze root configuration files** — look for whatever exists:

```
# JavaScript/TypeScript
package.json, tsconfig.json, vite.config.*, next.config.*, webpack.config.*

# Python
pyproject.toml, setup.py, setup.cfg, requirements.txt, Pipfile, poetry.lock

# Rust
Cargo.toml, Cargo.lock

# Go
go.mod, go.sum

# Java/Kotlin
build.gradle*, pom.xml, settings.gradle*

# Ruby
Gemfile, Rakefile, .ruby-version

# General
Dockerfile, docker-compose.yml, Makefile, .github/workflows/*, .env.example
```

**Map the directory structure** — understand where things live:
- Where does source code live? (`src/`, `lib/`, `app/`, `cmd/`, etc.)
- Where are tests? (`tests/`, `__tests__/`, `spec/`, alongside source files?)
- Any shared/common code? (`shared/`, `common/`, `utils/`, `pkg/`)
- Configuration locations?
- Documentation locations?

---

### Phase 2: ANALYZE

Dig deeper into the codebase to extract patterns and conventions.

**Extract the tech stack** from config files and imports:
- Language and runtime (Node, Bun, Deno, Python, Rust, Go, etc.)
- Framework(s) and their versions
- Database (if any) and ORM/query builder
- Testing tools and test runner
- Build tools and bundler
- Linting and formatting (ESLint, Prettier, Ruff, rustfmt, etc.)
- CI/CD setup

**Identify patterns** by reading a sample of source files (3-5 representative files):
- **Naming**: How are files, functions, variables, classes, and modules named? (camelCase, snake_case, PascalCase, kebab-case for files?)
- **Structure**: How is code organized within files? (exports at top/bottom, function ordering)
- **Errors**: How are errors created and handled? (custom error classes, Result types, error codes?)
- **Types**: How are types/interfaces defined? (separate files, co-located, generated?)
- **Tests**: How are tests structured? (describe/it blocks, test classes, naming conventions?)
- **Imports**: Absolute vs relative? Path aliases? Barrel files?

**Find key files** that are important to understand:
- Entry points (main, index, app)
- Configuration files that affect behavior
- Core business logic
- Shared utilities and helpers
- Type definitions or schemas
- Database migrations or schema files

---

### Phase 3: GENERATE

Create `CLAUDE.md` at the project root. Adapt the content to what you actually found — omit sections that don't apply, and add project-specific sections where relevant.

**Core sections** (include these for every project):

1. **Project Overview** — What is this project? What problem does it solve? One short paragraph.

2. **Tech Stack** — List the key technologies, frameworks, and tools. Be specific about versions when they matter.

3. **Commands** — How to run common development tasks. Extract these from package.json scripts, Makefile targets, or whatever task runner the project uses:
   - Install dependencies
   - Run development server
   - Build for production
   - Run tests
   - Lint / format
   - Any other common commands

4. **Project Structure** — A brief, high-level map of where things live. Don't list every file — focus on the directories and their purposes.

5. **Patterns and Conventions** — The coding conventions this project follows. This is the most valuable section — it prevents Claude from introducing inconsistent patterns. Include:
   - Naming conventions
   - File organization patterns
   - Error handling approach
   - Import style
   - Testing conventions
   - Any other project-specific patterns

6. **Key Files** — Files that are especially important to understand when working on this project, with a one-line explanation of each.

**Optional sections** (add only when relevant):

- **Architecture** — For complex apps: how the system is structured, data flow, key design decisions
- **API Endpoints** — For backends: route structure, authentication, response format conventions
- **Component Patterns** — For frontends: component structure, state management, styling approach
- **Database** — Schema overview, migration approach, query patterns
- **Deployment** — How the project is deployed, environment configuration
- **Environment Variables** — Required env vars and what they configure (don't include actual values)

**Style guidance for the CLAUDE.md:**
- Keep it scannable — use headings, bullet points, and short paragraphs
- Be concrete — show actual file paths, actual command names, actual patterns from the code
- Don't duplicate other docs — if there's a detailed API doc or architecture doc, link to it rather than repeating it
- Focus on what Claude needs to know to write code that fits in — patterns, conventions, and gotchas
- Keep it under 200 lines if possible. Shorter is better, as long as the important context is there

---

### Phase 4: OUTPUT

After generating the file, report back to the user:

```markdown
## CLAUDE.md Created

**File**: `CLAUDE.md`

### Detected Project Type
{What type of project this is}

### Tech Stack
{Key technologies found}

### Structure Overview
{Brief description of how the code is organized}

### What's Included
{List of sections generated and why}

### Next Steps
1. Review the generated `CLAUDE.md` and correct anything that's off
2. Add project-specific notes that aren't discoverable from code alone (e.g., "we're migrating from X to Y", "don't use library Z because of issue W")
3. Remove any sections that aren't useful for your project
4. Consider adding on-demand context pointers for large reference docs
```

---

## Edge Cases

- **Monorepos**: Generate a root `CLAUDE.md` that covers the overall structure, then mention that individual packages may benefit from their own `CLAUDE.md` files.
- **Empty/new projects**: If the project has minimal code, focus on the tech stack and intended structure based on config files. Note that the file should be updated as the project grows.
- **Existing CLAUDE.md**: If one already exists, read it first. Ask the user whether to replace it or merge new findings into the existing file.
- **Very large codebases**: Don't try to read everything. Sample representative files from each major directory to identify patterns. Focus on breadth over depth.
