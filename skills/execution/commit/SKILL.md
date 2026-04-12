---
name: commit
description: Create a well-structured git commit with a standardized conventional commit message. Use this skill whenever the user wants to commit changes, save progress, create a checkpoint, or finalize work. Also trigger when the user says "commit this", "save my changes", "commit what we've done", "checkpoint", or has just finished implementing something and needs to commit. This skill ensures every commit message follows a consistent format so that git log remains useful and parseable by both humans and AI agents.
---

# Commit: Standardized Git Commits

Create clean, well-structured commits with conventional commit messages that make `git log` a reliable source of project history for humans and AI agents alike.

## Why standardized commits matter

Git history is one of the first things an AI agent reads when joining a project (via `prime` or just `git log`). Inconsistent commit messages — vague subjects, missing context, mixed formats — make that history useless. Standardized commits turn `git log` into a navigable changelog that any future agent or developer can quickly parse to understand what changed, why, and where.

---

## Step 1: Review Changes

Before committing anything, understand what's being committed.

Run these commands to get the full picture:

```bash
git status
git diff HEAD
```

From this, determine:
- **What files changed** — new, modified, deleted
- **What the changes actually do** — read the diffs, don't just look at filenames
- **Whether these changes belong in one commit or several** — a commit should be atomic (one logical change). If the diff contains unrelated changes, they should be separate commits.

If there are files that shouldn't be committed (`.env`, credentials, large binaries, build artifacts), leave them out and mention it to the user.

---

## Step 2: Stage Files

Stage the files that belong in this commit:

- Prefer staging specific files by name over `git add .` or `git add -A` — this avoids accidentally including sensitive files or unrelated changes
- If all changes are related and belong together, staging everything is fine
- For new untracked files, verify they should be tracked (not generated files, secrets, etc.)

---

## Step 3: Write the Commit Message

Use the **Conventional Commits** format. This is the standard that makes `git log` machine-parseable and human-scannable.

### Format

```
<type>(<scope>): <subject>

<body>
```

### Type (required)

Pick the one that best describes the change:

| Type | When to use |
|------|------------|
| `feat` | A new feature or capability |
| `fix` | A bug fix |
| `refactor` | Code restructuring that doesn't change behavior |
| `docs` | Documentation changes only |
| `test` | Adding or updating tests |
| `chore` | Build config, dependencies, tooling, CI |
| `style` | Formatting, whitespace, semicolons — no logic change |
| `perf` | Performance improvement |
| `ci` | CI/CD pipeline changes |
| `revert` | Reverting a previous commit |

### Scope (optional but encouraged)

A short identifier for the area of the codebase affected. Use whatever makes sense for the project:
- Component or module name: `auth`, `api`, `database`, `ui`
- Feature area: `search`, `payments`, `onboarding`
- File or directory: `config`, `utils`, `middleware`

Examples: `feat(auth)`, `fix(api)`, `refactor(database)`

### Subject (required)

The subject line is the most important part. Rules:

- **Start with a lowercase verb** in imperative mood ("add", "fix", "update" — not "added", "fixes", "updating")
- **Be specific** about what changed — "fix login error when session expires" not "fix bug"
- **Keep it under 72 characters** so it displays cleanly in `git log --oneline`
- **Don't end with a period**

Good subjects:
- `feat(auth): add JWT refresh token rotation`
- `fix(api): handle null response from payment gateway`
- `refactor(utils): extract date formatting into shared helper`
- `docs: add API authentication guide`

Bad subjects:
- `fix: bug fix` (what bug?)
- `feat: updates` (what updates?)
- `chore: stuff` (what stuff?)
- `Fixed the thing that was broken` (wrong tense, vague, no type)

### Body (when needed)

Add a body when the subject alone doesn't tell the full story. Use it to explain:
- **Why** the change was made (not just what — the diff shows what)
- **Context** that isn't obvious from the code (e.g., "the upstream API changed their response format")
- **Trade-offs** or decisions made (e.g., "chose approach X over Y because...")
- **Breaking changes** — prefix with `BREAKING CHANGE:` if applicable

Separate the body from the subject with a blank line. Wrap at 72 characters.

---

## Step 4: Commit

Create the commit using a heredoc to preserve formatting:

```bash
git commit -m "$(cat <<'EOF'
type(scope): subject line here

Body paragraph here if needed, explaining the why
behind the change.
EOF
)"
```

After committing, run `git status` to confirm the working tree is in the expected state.

---

## Step 5: Report

After the commit is created, provide a brief summary:

```markdown
## Committed

**`<commit hash short>`** `<type>(<scope>): <subject>`

### Changes
- <file>: <what changed>
- <file>: <what changed>

### Next
<Suggest logical next steps — more commits, push, PR, etc.>
```

---

## Multiple Logical Changes

If the staged changes contain multiple unrelated things, split them into separate commits. Each commit should represent one atomic, logical change. For example:

- Don't combine a bug fix and a new feature in one commit
- Don't combine a refactor and a behavior change in one commit
- Do group related file changes (e.g., a new component + its test + its types) into one commit

Ask the user if you're unsure whether to split or combine.

---

## Examples

**Simple feature:**
```
feat(search): add fuzzy matching for product names
```

**Bug fix with context:**
```
fix(auth): prevent session fixation on password reset

The previous implementation reused the existing session ID after
password reset, which allowed session fixation attacks. Now generates
a new session ID after successful password change.
```

**Refactor:**
```
refactor(api): consolidate error response formatting

All API endpoints now use shared formatError() instead of
constructing error responses inline. No behavior change —
response format is identical.
```

**Chore with scope:**
```
chore(deps): upgrade express from 4.18 to 4.21
```

**Docs:**
```
docs: add deployment guide for AWS ECS
```
