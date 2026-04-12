---
name: context-audit
description: >
  Audit your Claude Code setup for token waste and context bloat. Use when
  the user says "audit my context", "check my settings", "why is Claude so
  slow", "token optimization", "context audit", or runs /context-audit.
  Also trigger when the user mentions high token usage, slow responses,
  bloated context, or wants to optimize their Claude Code configuration.
  Do NOT trigger for general performance questions unrelated to Claude Code
  setup (e.g., "why is my app slow").
user-invocable: true
---

# Context Audit

Bloated context costs more and produces worse output. This skill finds
the waste and tells you what to cut.

## Step 1: Get the Context Breakdown

Check the conversation history for `/context` output. If the user already
ran `/context` in this session, use that data and proceed to Step 2.

If not, ask:

> Run `/context` in your terminal and paste or share the output. This
> gives me the real token breakdown so I can audit what's actually
> costing you. I'll wait for it before starting.

**Fallback**: If the user can't or doesn't want to run `/context`, proceed
without it. Audit everything in Step 2 except the MCP token overhead
numbers. Note in the final report that the MCP and system prompt token
counts are estimated, not measured, and recommend running `/context` for
precise numbers.

STOP HERE if waiting for `/context`. Do NOT proceed to Step 2 until the
user responds (either with `/context` output or saying to skip it).

## Step 2: Audit What's Bloated

Work through each category. If `/context` data is available, audit from
largest overhead to smallest. Otherwise, audit in the order below. Run
checks in parallel where possible.

### MCP Servers

Each MCP server loads its full tool definitions into context every turn.
This typically costs 15,000-20,000 tokens per server — whether or not
you use those tools in a given turn.

Read both settings files to find configured servers:
- **Project**: `.claude/settings.json`
- **User**: `~/.claude/settings.json`

For each server found:
- Note its name and where it's configured (project vs user)
- Flag servers that have CLI alternatives costing zero tokens when idle
  (common examples: Playwright, GitHub, Google Workspace, Slack, Notion
  all have CLIs or dedicated tools that only consume tokens when called)
- If `/context` data is available, report the actual MCP overhead
- If not, estimate: `(number of servers) x ~17,500 tokens`

### CLAUDE.md Rules

Read all CLAUDE.md files that Claude loads:
- Project root `CLAUDE.md`
- `.claude/CLAUDE.md`
- `~/.claude/CLAUDE.md`

Count total lines across all files. Then read every rule and test it
against these five filters:

| Filter | Flag when... |
|--------|-------------|
| **Default** | Claude already does this without being told ("write clean code", "handle errors", "follow best practices") |
| **Contradiction** | Conflicts with another rule in the same or a different file |
| **Redundancy** | Repeats something already covered by another rule |
| **Bandaid** | Added to fix one specific bad output rather than improve outputs generally |
| **Vague** | Could be interpreted differently every time ("be natural", "use good tone", "write quality code") |

If total lines across all files exceed 200, check for progressive
disclosure opportunities. Rules that only apply to specific tasks (API
conventions, deployment steps, testing guidelines, framework-specific
patterns) should live in reference files with one-line pointers from
CLAUDE.md. Only recommend splitting when the file is genuinely bloated —
a lean CLAUDE.md with universal context is fine as a single file.

### Skills

Find all skills by scanning:
- `.claude/skills/*/SKILL.md` (project-level)
- `~/.claude/skills/*/SKILL.md` (user-level)

For each skill:
- Count lines in the SKILL.md (flag if > 200, critical if > 500)
- Run the same five filters on the skill's instructions
- Flag restated goals, hedging language ("you may want to", "consider
  perhaps"), and synonymous instructions ("be concise" + "keep it short"
  + "don't be verbose" all say the same thing)

### Settings

Read both `.claude/settings.json` and `~/.claude/settings.json`. Check:

| Setting | Flag if | Recommended |
|---------|---------|-------------|
| `autocompact_percentage_override` | Missing or > 80 | 75 |
| `BASH_MAX_OUTPUT_LENGTH` (in env) | Missing or at default (30-50K) | 150000 |

### File Permissions

Check both settings files for `permissions.deny` rules. Then check
whether bloat-prone directories exist in the project:

| If this exists in the project... | Should deny reads/writes to... |
|----------------------------------|-------------------------------|
| `package.json` | `node_modules`, `dist`, `build`, `.next`, `coverage` |
| `Cargo.toml` | `target` |
| `go.mod` | `vendor` |
| `pyproject.toml` or `requirements.txt` | `__pycache__`, `.venv`, `*.egg-info` |

Flag missing deny rules only when the corresponding directories actually
exist in the project. No point flagging `node_modules` if there's no
`package.json`.

## Step 3: Score and Report

Start at 100. Deduct per issue found:

| Issue | Points |
|-------|--------|
| CLAUDE.md total > 200 lines | -10 |
| CLAUDE.md total > 500 lines | -20 (replaces the -10) |
| Per 5 rules flagged by filters | -5 |
| Contradictions between files | -10 |
| Missing `autocompact_percentage_override` | -10 |
| Missing `BASH_MAX_OUTPUT_LENGTH` override | -5 |
| Skill SKILL.md > 200 lines | -5 each |
| Skill SKILL.md > 500 lines | -10 each (replaces -5) |
| Per MCP server configured | -3 each |
| No deny rules + bloat directories exist | -10 |

Floor at 0. Score labels:
- **90-100**: CLEAN
- **70-89**: NEEDS WORK
- **50-69**: BLOATED
- **0-49**: CRITICAL

Issue severity: CRITICAL = costs > 10 points, WARNING = 5-10 points,
INFO = < 5 points.

Output this format:

```
# Context Audit

Score: {N}/100 [{CLEAN|NEEDS WORK|BLOATED|CRITICAL}]

## Context Breakdown
{If /context was available: paste the key numbers}
{If not: note estimates and recommend running /context}

## Issues Found

### [{CRITICAL|WARNING|INFO}] {Category}
{What's wrong}
**Fix:** {One-line actionable fix}

### Rules to Cut
{Each flagged rule with: the text, which filter caught it, one-line reason}

### Conflicts
{Contradictions between files, with file paths and line numbers}

## Top 3 Fixes
1. {Highest-impact fix — the one that saves the most tokens}
2. {Second highest impact}
3. {Third highest impact}
```

## Step 4: Apply Fixes

After presenting the report, offer to fix what was found:

> Want me to fix any of these? I can:
> - Show a cleaned-up CLAUDE.md with flagged rules removed
> - Add the missing settings.json configs
> - Add permissions.deny rules for build artifacts
> - Show which skills to trim and how

**Auto-apply** (safe, reversible — just do it):
- `autocompact_percentage_override` and `BASH_MAX_OUTPUT_LENGTH` in
  settings.json
- `permissions.deny` rules for detected build artifact directories

**Show diff first** (let the user confirm before writing):
- CLAUDE.md edits (removing or rewriting flagged rules)
- Skill SKILL.md edits (trimming bloated instructions)

These are the user's own instructions — don't modify them without
explicit approval. Show exactly what you'd remove and why, then wait
for a go-ahead.
