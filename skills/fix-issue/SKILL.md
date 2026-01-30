---
name: fix-issue
description: >
  Systematic issue diagnosis and resolution for codebases. Use when asked to fix bugs, resolve errors, debug issues, or troubleshoot code problems. Emphasizes root cause identification before code changes, parallel subagent delegation, and leaving the codebase better than found. Triggers on fix, debug, resolve, troubleshoot, why is this broken, this doesn't work, or any error/bug investigation request.
---

# Fix Issue

Diagnose and fix codebase issues systematically. **Critical principle: diagnosis must complete before any code changes.** LLM debugging effectiveness decays 60-80% after 2-3 failed attempts—accurate first-pass diagnosis is essential.

## Workflow Overview

1. **Diagnose** — Understand the issue completely (NO CODE YET)
2. **Plan** — Design minimal, targeted fix
3. **Execute** — Implement via parallel subagents
4. **Verify** — Build, test, clean up

## Phase 1: Diagnosis (No Code Changes)

**Block all code generation until diagnosis completes.**

### 1.1 Gather Context

Collect before analyzing:
- Full error message and stack trace
- Recent changes (`git diff`, `git log -5`)
- Expected vs actual behavior
- Reproduction steps if available

### 1.2 Map Affected Code

Delegate exploration to subagents in parallel:
```
Subagent tasks (parallel):
- "Read all files in [module] and list functions related to [symptom]"
- "Trace the call chain from [entry point] to [error location]"
- "Find all usages of [function/variable] across codebase"
```

Do NOT write code—only read and report.

### 1.3 Root Cause Analysis

Apply the **5 Whys with Evidence** technique:

```
Why did [symptom] occur?
→ Because [cause 1]. Evidence: [specific line/log/trace]

Why did [cause 1] occur?
→ Because [cause 2]. Evidence: [specific line/log/trace]

Continue until root cause identified. Each "why" requires concrete evidence.
```

Generate **3-5 hypotheses ranked by probability**. For each:
- State the hypothesis
- Identify what evidence would confirm it
- Identify what evidence would reject it

Use extended thinking: include "think hard" or "ultrathink" in complex cases.

**Distinguishing symptoms from root causes:**
- Symptoms cluster together and appear downstream
- Root cause is a single point explaining multiple symptoms
- Ask: "If I fix X, which symptoms disappear?"

See `references/root-cause-analysis.md` for detailed techniques.

### 1.4 Confirm Understanding

Before proceeding, document:
- **Root cause**: Single sentence identifying the actual bug
- **Evidence**: Specific code/logs proving this is the cause
- **Affected files**: Complete list with line numbers
- **Current behavior**: What happens now
- **Expected behavior**: What should happen

If multiple attempts have failed to identify root cause, **reset completely**:
```
"Forget previous hypotheses. Restart with fresh observation of symptoms.
Generate entirely new hypotheses independent of prior attempts."
```

## Phase 2: Fix Plan

### 2.1 Design Minimal Changes

Requirements:
- Modify existing files (avoid creating new ones unless clearly better)
- Preserve existing functionality and interfaces
- Follow project conventions and patterns
- Apply DRY—reuse/update existing methods before creating new ones
- Remove dead code (research usage first)
- No backward compatibility unless explicitly requested
- No mocked/fake implementations

### 2.2 Document the Plan

```
Fix Plan:
1. File: [path] Line [N]: Change [X] to [Y] because [reason]
2. File: [path] Line [N]: Add [code] because [reason]
...

Tests to add/modify:
- [test case]: Verifies [behavior]

Potential side effects:
- [risk]: Mitigated by [safeguard]
```

## Phase 3: Execute via Parallel Subagents

Decompose into independent tasks. Good task boundaries are **SAOI**:
- **Specific** — Clear enough the agent knows exactly what to do
- **Achievable** — Within agent capabilities
- **Ordered** — Later steps can build on earlier ones
- **Independent** — Minimal dependencies between parallel tasks

See `references/parallel-decomposition.md` for delegation patterns.

### Parallel Execution Pattern

```
Phase A (parallel):
- Subagent 1: Implement fix in [file1]
- Subagent 2: Implement fix in [file2]
- Subagent 3: Write/update test cases

Phase B (after A completes):
- Subagent 4: Integration verification
- Subagent 5: Documentation updates
```

**Subagent instructions must include:**
- Specific file and line numbers
- Exact change to make
- Constraint: "Do not modify any other code"
- Verification: "After changes, confirm [expected behavior]"

## Phase 4: Verify

### 4.1 Build and Test

```bash
# Run project build
# Run test suite
# Check for warnings/errors
```

Resolve ALL warnings and errors before completing.

### 4.2 Clean Up

- Remove any dead code introduced or exposed
- Verify no debugging artifacts remain (console.log, print statements)
- Ensure consistent formatting

### 4.3 Final Verification

- Confirm original issue is resolved
- Confirm no regressions in related functionality
- Run full test suite if available

## Anti-Patterns to Avoid

1. **Treating symptoms** — Fix addresses visible error, not underlying cause
2. **Overly broad changes** — Keep PRs atomic, under 200 lines when possible
3. **Breaking existing functionality** — Test thoroughly, preserve interfaces
4. **Context drift** — If stuck after 2-3 attempts, reset completely
5. **Hallucinated fixes** — Every change must trace to diagnosed root cause

## Resources

- `references/root-cause-analysis.md` — Detailed RCA techniques (5 Whys, scientific debugging, delta debugging)
- `references/parallel-decomposition.md` — Subagent delegation patterns and task boundary guidelines
