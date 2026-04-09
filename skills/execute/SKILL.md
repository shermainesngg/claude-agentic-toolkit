---
name: execute
description: Execute an implementation plan by working through each task in order, writing code, running validations, and reporting results. Use this skill whenever the user wants to execute a plan, implement from a plan file, build a feature from a spec, or carry out a previously created implementation plan. Also trigger when the user says "execute this plan", "implement the plan", "build this from the plan", "run the plan", or provides a path to a plan file and wants it implemented. This is the counterpart to plan-feature — plan-feature creates the plan, execute carries it out.
argument-hint: <path-to-plan-file>
---

# Execute: Implement from Plan

Take an implementation plan and execute it methodically — task by task, with validation at every step. The goal is disciplined, one-pass implementation with zero surprises at the end.

## Why a separate execution skill?

Planning and execution are different modes of thinking. Planning is about research, analysis, and decision-making. Execution is about precision, discipline, and following through. Separating them means the plan gets full attention during planning, and implementation gets full focus during execution — no temptation to cut corners or redesign mid-build.

---

## Step 1: Read and Internalize the Plan

Read the plan file provided by the user (the `$ARGUMENTS` path, or ask for it if not provided). Check `.agents/plans/` if the user references a feature by name without a full path.

As you read, build a mental model of:
- **The big picture** — what's being built and why
- **The task sequence** — what depends on what, and the intended order
- **The patterns** — what existing code to mirror, what conventions to follow
- **The validation strategy** — how to verify each step and the final result
- **The gotchas** — anything flagged as risky or tricky

Read every file listed in the plan's "Files to Read Before Implementing" section. These contain the patterns and context you need. Don't skip this — the plan author put them there for a reason.

---

## Step 2: Execute Tasks in Order

Work through the "Step-by-Step Tasks" section top to bottom. For each task:

### a. Prepare
- Read the target file (if modifying an existing file)
- Read any pattern reference files mentioned in the task
- Understand what the task is asking and why

### b. Implement
- Follow the task specifications precisely
- Mirror the patterns identified in the plan — naming, structure, error handling, logging
- Use the imports specified in the task
- Watch for the gotchas flagged in the task

### c. Validate immediately
- Run the task's validation command (if one is specified)
- Check that imports resolve correctly
- Verify types/syntax are correct
- If validation fails, fix the issue before moving to the next task

Do not batch tasks and validate later. Each task should be green before moving on. This catches problems early when they're cheap to fix.

---

## Step 3: Implement Tests

After the implementation tasks are complete, create all tests specified in the plan:

- Follow the testing strategy and test structure outlined in the plan
- Mirror existing test patterns in the project (test framework, naming, fixtures, assertions)
- Cover the edge cases listed in the plan
- Make sure tests actually test behavior, not just that code runs without errors

Run the test suite after writing tests. If tests fail, fix the implementation (not the tests) unless the test expectation is wrong.

---

## Step 4: Run All Validation Commands

Execute every validation command from the plan, in order:

1. **Syntax & Style** — linting, formatting, type checking
2. **Unit Tests** — the project's unit test suite
3. **Integration Tests** — end-to-end or integration tests
4. **Manual Validation** — any manual verification steps

If any command fails:
1. Read the error carefully
2. Fix the root cause (not just the symptom)
3. Re-run the command to confirm it passes
4. Continue to the next command

Do not skip validation steps even if you're confident the code is correct.

---

## Step 5: Final Verification

Before reporting completion, verify:

- [ ] All tasks from the plan are completed
- [ ] All new files are created in the correct locations
- [ ] All modified files have the intended changes
- [ ] All tests are created and passing
- [ ] All validation commands pass
- [ ] Code follows the project's conventions and patterns
- [ ] No regressions in existing functionality

---

## Output Report

After execution is complete, provide a summary:

```markdown
## Execution Complete

### Plan
<Plan file path and feature name>

### Completed Tasks
<List each task completed, grouped by phase>

### Files Created
- `path/to/new_file` — what it does

### Files Modified
- `path/to/existing_file` — what changed

### Tests
- Test files created: <list>
- Test results: <pass/fail summary>

### Validation Results
<Output or pass/fail status for each validation command>

### Deviations from Plan
<Any places where you had to deviate from the plan, and why.
If none, say "None — plan executed as written.">

### Issues Encountered
<Any problems hit during execution and how they were resolved.
If none, say "None.">

### Ready for Review
All changes are complete and validated. Ready for review and commit.
```

---

## When Things Go Wrong

- **Validation fails and you can't figure out why**: Report the failure clearly to the user with the error output. Don't silently skip it.
- **The plan has a gap or contradiction**: Note it, make your best judgment call, implement it, and flag the deviation in your report.
- **A task doesn't make sense given what you've seen in the code**: Pause and tell the user. It's better to clarify than to implement something wrong.
- **Tests reveal a bug in the implementation**: Fix the implementation. Tests are the source of truth for expected behavior.
- **You need to deviate from the plan**: That's fine — just document what you changed and why in the output report. Small tactical adjustments are normal; large architectural changes should be discussed with the user first.
