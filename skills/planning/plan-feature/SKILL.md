---
name: plan-feature
description: Transform a feature request into a comprehensive, context-rich implementation plan that enables one-pass execution by an AI agent or developer. Use this skill whenever the user wants to plan a feature, design a task breakdown, create an implementation plan, scope out work, or prepare a coding task for execution. Also trigger when the user says "plan this feature", "break this down", "create an implementation plan", "scope this out", "how should we implement this", or describes a feature they want built and would benefit from structured planning before coding.
argument-hint: <feature description>
---

# Plan Feature: Create Implementation-Ready Plans

Transform a feature request into a comprehensive implementation plan through systematic codebase analysis, research, and strategic thinking.

## Core Principle

Do NOT write code in this phase. The goal is to create a plan so thorough and context-rich that an AI agent (or developer) can execute it in a single pass without needing additional research or clarification.

Context is king — the plan must contain everything needed for implementation: patterns to follow, files to read, documentation links, validation commands, and gotchas to avoid.

---

## Phase 1: Understand the Feature

Start by deeply understanding what's being asked.

**Analyze the request:**
- What core problem is being solved?
- Who benefits and how?
- What type of work is this? (New capability / Enhancement / Refactor / Bug fix)
- How complex is it? (Low / Medium / High)
- What systems and components are affected?

**Create or refine a user story:**
```
As a <type of user>
I want to <action/goal>
So that <benefit/value>
```

**Clarify ambiguities** — if the requirements are unclear, ask the user before continuing. Get specific about:
- Implementation preferences (libraries, approaches)
- Architectural constraints
- Scope boundaries (what's explicitly out of scope?)

---

## Phase 2: Gather Codebase Intelligence

Investigate the codebase systematically. Parallelize these where possible.

**1. Project Structure**
- Detect languages, frameworks, runtime versions
- Map directory structure and architectural patterns
- Identify service/component boundaries
- Locate config files and build processes
- Read `CLAUDE.md` for project-specific rules and conventions

**2. Pattern Recognition**
- Search for similar implementations already in the codebase — these are your best guide
- Identify coding conventions: naming, file organization, error handling, logging
- Extract common patterns for the feature's domain
- Note anti-patterns to avoid

**3. Dependency Analysis**
- Catalog external libraries relevant to the feature
- Understand how they're integrated (check imports, configs)
- Find relevant docs in `docs/`, `ai_docs/`, `.agents/reference/`, or similar
- Note version and compatibility requirements

**4. Testing Patterns**
- Identify test framework and structure
- Find similar test examples for reference
- Understand test organization (unit vs integration vs e2e)
- Note coverage requirements

**5. Integration Points**
- Identify existing files that need updates
- Determine new files needed and where they should live
- Map route/API registration patterns
- Understand database/model patterns if applicable
- Identify auth/authorization patterns if relevant

---

## Phase 3: Research and Documentation

Gather external context that the implementation agent will need.

- Research latest best practices for the relevant libraries/frameworks
- Find official documentation with specific section anchors (not just homepage links)
- Locate implementation examples and tutorials
- Identify common gotchas and known issues
- Check for breaking changes or migration guides

Compile references with context on why each is relevant:
```markdown
- [Library Docs - Auth Section](https://example.com/docs#auth)
  Why: Shows the pattern for token refresh we need to implement
```

---

## Phase 4: Strategic Thinking

Before writing the plan, think deeply about:

- How does this fit into the existing architecture?
- What are the critical dependencies and correct order of operations?
- What could go wrong? (Edge cases, race conditions, error scenarios)
- How will this be tested comprehensively?
- Are there performance implications?
- Are there security considerations?
- How maintainable is this approach long-term?

Choose between alternative approaches with clear rationale. Design for the project's actual needs, not hypothetical future ones.

---

## Phase 5: Generate the Plan

Write the plan to `.agents/plans/{kebab-case-name}.md` (create the directory if needed).

Use this structure — adapt sections based on what's relevant to the feature:

```markdown
# Feature: <feature-name>

Validate documentation and codebase patterns before implementing. Pay attention to naming of existing utils, types, and models. Import from the right files.

## Feature Description
<Detailed description — purpose, value, how it works>

## User Story
As a <user>
I want to <goal>
So that <benefit>

## Problem Statement
<The specific problem or opportunity>

## Solution Statement
<The proposed approach and how it solves the problem>

## Feature Metadata
- **Type**: [New Capability / Enhancement / Refactor / Bug Fix]
- **Complexity**: [Low / Medium / High]
- **Systems Affected**: [list]
- **Dependencies**: [external libraries or services]

---

## CONTEXT REFERENCES

### Files to Read Before Implementing
- `path/to/file.py` (lines 15-45) — Contains pattern for X to mirror
- `path/to/model.py` (lines 100-120) — Database model structure to follow
- `path/to/test.py` — Test pattern example

### New Files to Create
- `path/to/new_service.py` — Service implementation for X
- `tests/path/to/test_new_service.py` — Tests for new service

### Documentation to Read
- [Doc Link](url#section) — Why: needed for X implementation

### Patterns to Follow
<Actual code examples extracted from the codebase — naming, error handling, logging, etc.>

---

## IMPLEMENTATION PLAN

### Phase 1: Foundation
<Base structures, schemas, types, dependencies>

### Phase 2: Core Implementation
<Business logic, service layer, API endpoints, data models>

### Phase 3: Integration
<Connect to existing routers, update config, register components>

### Phase 4: Testing & Validation
<Unit tests, integration tests, edge cases>

---

## STEP-BY-STEP TASKS

Execute every task in order, top to bottom. Each task is atomic and independently testable.

Action keywords:
- **CREATE**: New files or components
- **UPDATE**: Modify existing files
- **ADD**: Insert new functionality
- **REMOVE**: Delete deprecated code
- **REFACTOR**: Restructure without behavior change
- **MIRROR**: Copy pattern from elsewhere in codebase

### {ACTION} {target_file}
- **IMPLEMENT**: Specific implementation detail
- **PATTERN**: Reference to existing pattern — file:line
- **IMPORTS**: Required imports and dependencies
- **GOTCHA**: Known issues or constraints
- **VALIDATE**: `executable validation command`

---

## TESTING STRATEGY

### Unit Tests
<Based on project's test framework and conventions>

### Integration Tests
<End-to-end workflow verification>

### Edge Cases
<Specific edge cases to test>

---

## VALIDATION COMMANDS

### Level 1: Syntax & Style
<Linting and formatting commands>

### Level 2: Unit Tests
<Test runner commands>

### Level 3: Integration Tests
<Integration test commands>

### Level 4: Manual Validation
<Feature-specific manual testing steps>

---

## ACCEPTANCE CRITERIA
- [ ] Feature implements all specified functionality
- [ ] All validation commands pass with zero errors
- [ ] Tests cover core paths and edge cases
- [ ] Code follows project conventions and patterns
- [ ] No regressions in existing functionality
- [ ] Documentation updated (if applicable)

---

## NOTES
<Design decisions, trade-offs, risks, alternatives considered>
```

---

## Output

After creating the plan, report back:

```markdown
## Plan Created

**File**: `.agents/plans/{name}.md`

### Summary
<What the feature does and the chosen approach>

### Complexity
<Low / Medium / High — with brief justification>

### Key Risks
<Top 2-3 implementation risks or considerations>

### Confidence Score
<X/10 that an agent can execute this plan in one pass>
```

---

## Quality Checks

Before finalizing, verify the plan passes these criteria:

**Context Completeness**
- All necessary codebase patterns identified with file:line references
- External library usage documented with links
- Integration points clearly mapped
- Gotchas captured
- Every task has a validation command

**Implementation Ready**
- Another developer (or AI agent) could execute without additional research
- Tasks ordered by dependency — executable top-to-bottom
- Each task is atomic and independently testable

**Pattern Consistency**
- Tasks follow existing codebase conventions
- No reinvention of existing utilities or patterns
- Testing approach matches project standards

**The "No Prior Knowledge" Test**
- Someone unfamiliar with the codebase can implement using only the plan's content
- If this test fails, the plan needs more context
