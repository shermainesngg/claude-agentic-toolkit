# Claude Agentic Toolkit

A collection of reusable skills for Claude Code that provide structured workflows for common software engineering tasks. These skills help Claude work more effectively on your projects — from understanding a codebase to planning features, executing implementations, and committing clean code.

## Skills

| Skill | Purpose |
|-------|---------|
| **create-rules** | Generate a `CLAUDE.md` file by analyzing your codebase — extracting patterns, conventions, tech stack, and project structure |
| **prime** | Load full project context at the start of a session — read docs, key files, git state, and build a mental model |
| **plan-feature** | Transform a feature request into a comprehensive implementation plan that an agent can execute in one pass |
| **execute** | Carry out an implementation plan task by task, with validation at every step |
| **commit** | Create standardized conventional commits so `git log` stays useful for humans and agents |
| **create-prd** | Generate a comprehensive Product Requirements Document from conversation context |
| **grill-me** | Stress-test a plan or design through relentless questioning until all assumptions are examined |

## Recommended Workflow

```
create-rules  →  prime  →  plan-feature  →  execute  →  commit
     ↑                          ↑
  one-time setup         per feature cycle
```

1. **`create-rules`** — Run once to generate `CLAUDE.md` with your project's context and conventions
2. **`prime`** — Run at the start of a deep work session to load project context
3. **`plan-feature`** — Describe what you want built; get a detailed implementation plan
4. **`execute`** — Point it at the plan file; it implements task by task
5. **`commit`** — Commit the work with a clean conventional commit message

Use **`grill-me`** anytime you want to pressure-test a plan or design before building it. Use **`create-prd`** when you need to formalize a product idea into a structured requirements doc.

## Installation

### Install all skills

Clone the repo and install every skill at once:

```bash
git clone https://github.com/shermainesngg/claude-agentic-toolkit.git
cd claude-agentic-toolkit

# Install all skills
for skill in skills/*/; do
  claude skill install "$skill"
done
```

### Install individual skills

Install only the skills you need:

```bash
# From a local clone
claude skill install /path/to/claude-agentic-toolkit/skills/create-rules
claude skill install /path/to/claude-agentic-toolkit/skills/prime
claude skill install /path/to/claude-agentic-toolkit/skills/plan-feature
claude skill install /path/to/claude-agentic-toolkit/skills/execute
claude skill install /path/to/claude-agentic-toolkit/skills/commit
claude skill install /path/to/claude-agentic-toolkit/skills/create-prd
claude skill install /path/to/claude-agentic-toolkit/skills/grill-me
```

### Install directly from GitHub

```bash
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/create-rules
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/prime
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/plan-feature
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/execute
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/commit
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/create-prd
claude skill install https://github.com/shermainesngg/claude-agentic-toolkit/tree/main/skills/grill-me
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
```

## Contributing

To add a new skill, create a directory under `skills/` with a `SKILL.md` file following the frontmatter format:

```yaml
---
name: skill-name
description: When and why to use this skill.
---
```

See existing skills for examples of the full format.
