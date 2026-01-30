# Parallel Task Decomposition for Subagents

Patterns for effective subagent delegation in issue fixing.

## Task Boundary Principles (SAOI)

Good tasks for parallel execution are:

**Specific** — Agent knows exactly what to do without interpretation
- Bad: "Fix the authentication bug"
- Good: "In auth.js line 45, change `TZ_OFFSET = 0` to `TZ_OFFSET = user.timezone`"

**Achievable** — Within agent capabilities and available tools
- Bad: "Refactor the entire auth system"
- Good: "Update the token validation function to use dynamic timezone"

**Ordered** — Later steps can build on earlier ones when needed
- Phase 1 tasks should not depend on Phase 2 results
- Order phases logically: read → analyze → implement → test

**Independent** — Minimal dependencies between parallel tasks
- Bad: Two agents editing the same file section
- Good: Each agent owns distinct files or non-overlapping sections

## Exploration Phase Pattern

During diagnosis, maximize parallel discovery:

```
Parallel exploration tasks:
- Agent A: "List all files importing [module]. Report file paths only."
- Agent B: "Read [file] and summarize the control flow for [function]."
- Agent C: "Find all calls to [function] and note the arguments passed."
- Agent D: "Check git log for recent changes to [directory]."
```

**Key:** Exploration agents should ONLY read and report. No code changes.

## Implementation Phase Pattern

After diagnosis and planning:

```
Phase A (parallel, independent files):
- Agent 1: "In [file1] at line [N], make this change: [exact diff]"
- Agent 2: "In [file2] at line [N], make this change: [exact diff]"
- Agent 3: "Create test case in [test_file] that verifies [behavior]"

Phase B (after A completes):
- Agent 4: "Run test suite and report failures"
- Agent 5: "Update [docs] to reflect the API change"
```

## Subagent Instruction Template

Every subagent task should include:

```
Task: [one-line description]

Context:
- We are fixing [issue description]
- Root cause: [identified root cause]

Specific instructions:
1. Open [file path]
2. Navigate to line [N]
3. Change [old code] to [new code]

Constraints:
- Do NOT modify any other code
- Do NOT refactor unrelated sections
- Preserve existing interfaces

Verification:
- After changes, [expected outcome]
- If [unexpected outcome], report back without further changes
```

## Handling Shared File Edits

When multiple changes are needed in one file:

**Option 1: Sequential phases**
```
Phase 1: Agent A edits lines 10-50
Phase 2: Agent B edits lines 100-150 (after A completes)
```

**Option 2: Section ownership**
```
Agent A owns function foo() (lines 10-50)
Agent B owns function bar() (lines 100-150)
Both can run in parallel if no overlap
```

**Option 3: Single agent, multiple tasks**
```
One agent handles all changes to [file]:
- Change 1: line 10, [diff]
- Change 2: line 45, [diff]
- Change 3: line 100, [diff]
```

## Communication Patterns

For complex fixes requiring coordination:

**Scratchpad pattern:**
```
Create shared state file: /tmp/fix-state.json
- Agent A writes: {"step1": "complete", "found_X": true}
- Agent B reads state before proceeding
- Agent B writes: {"step2": "complete", "changed_Y": true}
```

**Blocking dependencies:**
```
Phase 1: Independent tasks (parallel)
Checkpoint: Verify phase 1 outputs before proceeding
Phase 2: Dependent tasks (may be parallel within phase)
```

## Test Writing Delegation

Delegate test creation alongside implementation:

```
Test agent instructions:
- Create test file: [path]
- Test case 1: Verify [original bug] is fixed
  - Input: [reproduction input]
  - Expected: [correct behavior]
- Test case 2: Verify no regression in [related feature]
  - Input: [normal usage input]
  - Expected: [existing correct behavior]
```

## Error Recovery

When a subagent reports failure:

1. **Collect failure details** — What specific error occurred?
2. **Assess scope** — Does this affect other parallel tasks?
3. **Decide: retry or escalate**
   - Simple error → Retry with clarified instructions
   - Systemic issue → Pause all agents, reassess plan

**Retry template:**
```
Previous attempt failed because: [error]

Revised instructions:
- [Updated approach addressing the failure]
- Additional context: [what was learned from failure]
```

## Anti-Patterns

**Too granular:**
- Bad: 20 agents each making 1-line changes
- Good: Group related changes into coherent tasks

**Too broad:**
- Bad: "Fix all issues in auth module"
- Good: "Fix timezone handling in token validation"

**Missing constraints:**
- Bad: "Update the config file"
- Good: "In config.json, change timeout from 30 to 60, do not modify other values"

**Unclear verification:**
- Bad: "Make sure it works"
- Good: "After changes, run `npm test auth` and confirm 0 failures"
