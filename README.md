# Claude Agentic Toolkit

A collection of reusable skills for Claude Code that provide structured workflows for common software engineering tasks. These skills help Claude work more effectively on your projects — from understanding a codebase to planning features, executing implementations, and committing clean code.

## Skills

### Context — Set up, maintain, and optimize Claude's project understanding

| Skill | Purpose |
|-------|---------|
| **prime** | Load full project context at the start of a session — read docs, key files, git state, and build a mental model |
| **create-rules** | Generate a `CLAUDE.md` file by analyzing your codebase — extracting patterns, conventions, tech stack, and project structure |
| **context-audit** | Audit your Claude Code setup for token waste and context bloat — scores your config and tells you what to cut |

### Planning — Scope, specify, and stress-test before building

| Skill | Purpose |
|-------|---------|
| **plan-feature** | Transform a feature request into a comprehensive implementation plan that an agent can execute in one pass |
| **create-prd** | Generate a comprehensive Product Requirements Document from conversation context |
| **grill-me** | Stress-test a plan or design through relentless questioning until all assumptions are examined |

### Execution — Build and ship

| Skill | Purpose |
|-------|---------|
| **execute** | Carry out an implementation plan task by task, with validation at every step |
| **commit** | Create standardized conventional commits so `git log` stays useful for humans and agents |

## Recommended Workflow

```
create-rules  →  prime  →  plan-feature  →  execute  →  commit
     ↑                          ↑                          ↓
  one-time setup         per feature cycle          context-audit
                                                   (periodic checkup)
```

1. **`create-rules`** — Run once to generate `CLAUDE.md` with your project's context and conventions
2. **`prime`** — Run at the start of a deep work session to load project context
3. **`plan-feature`** — Describe what you want built; get a detailed implementation plan
4. **`execute`** — Point it at the plan file; it implements task by task
5. **`commit`** — Commit the work with a clean conventional commit message

Use **`grill-me`** anytime you want to pressure-test a plan or design before building it. Use **`create-prd`** when you need to formalize a product idea into a structured requirements doc. Run **`context-audit`** periodically to find and fix token waste in your Claude Code setup.

## Installation

### Install all skills

Clone the repo and install every skill at once:

```bash
git clone https://github.com/shermainesngg/claude-agentic-toolkit.git
cd claude-agentic-toolkit

# Install all skills
for skill in skills/*/*/; do
  claude skill install "$skill"
done
```

### Install individual skills

Install only the skills you need:

```bash
# From a local clone
claude skill install /path/to/claude-agentic-toolkit/skills/context/prime
claude skill install /path/to/claude-agentic-toolkit/skills/context/create-rules
claude skill install /path/to/claude-agentic-toolkit/skills/context/context-audit
claude skill install /path/to/claude-agentic-toolkit/skills/planning/plan-feature
claude skill install /path/to/claude-agentic-toolkit/skills/planning/create-prd
claude skill install /path/to/claude-agentic-toolkit/skills/planning/grill-me
claude skill install /path/to/claude-agentic-toolkit/skills/execution/execute
claude skill install /path/to/claude-agentic-toolkit/skills/execution/commit
```

### Install directly from GitHub

```bash
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/context/prime
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/context/create-rules
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/context/context-audit
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/planning/plan-feature
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/planning/create-prd
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/planning/grill-me
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/execution/execute
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/execution/commit
```

## Usage

Once installed, skills trigger automatically based on context, or you can invoke them directly:

```
/create-rules
/prime
/plan-feature add user authentication with OAuth
/execute .agents/plans/add-user-authentication.md
/commit
/create-prd PRD.md
/grill-me
/context-audit
```

## Contributing

To add a new skill, create a directory under the appropriate category in `skills/` (`context/`, `planning/`, or `execution/`) with a `SKILL.md` file following the frontmatter format:

```yaml
---
name: skill-name
description: When and why to use this skill.
---
```

See existing skills for examples of the full format.
