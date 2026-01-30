# Parallel Patterns for Feature Implementation

Templates and patterns for effective subagent delegation during feature development.

## Exploration Phase Templates

Use during analysis to gather information in parallel:

### Architecture Discovery
```
Subagent A: "Find all files containing [related functionality]. 
List file paths and summarize each file's purpose in one sentence."

Subagent B: "Read [entry point file] and trace the request flow. 
Document the sequence: entry → processing → response."

Subagent C: "Search for existing utilities related to [functionality]. 
List function names, file locations, and what each does."

Subagent D: "Check project configuration files for relevant settings.
Note: build config, environment variables, feature flags."
```

### Pattern Recognition
```
Subagent: "Find 2-3 existing features similar to [new feature].
For each, document:
- File structure and organization
- Naming conventions used
- How they handle errors
- How they integrate with [shared system]"
```

## Implementation Phase Templates

### Foundation Layer (Parallel)
```
Subagent 1 - Data Models:
"Create data models for [feature] in [path].
Include: [list of models with fields]
Follow existing patterns from [reference file].
Do NOT implement business logic."

Subagent 2 - Type Definitions:
"Create type definitions/interfaces for [feature] in [path].
Include: [list of types]
Match existing type patterns from [reference file]."

Subagent 3 - Shared Utilities:
"Create utility functions for [feature] in [path].
Include: [list of utilities with signatures]
Reuse existing utilities from [reference file] where possible."
```

### Core Logic Layer (Parallel, after Foundation)
```
Subagent 4 - Main Logic:
"Implement [core function] in [path].
Tests to pass: [test_name_1], [test_name_2]
Use models from [foundation path].
Handle errors by [error strategy]."

Subagent 5 - Integration:
"Implement integration with [existing system] in [path].
Tests to pass: [test_name_3]
Use existing [utility] for [purpose].
Follow error handling pattern from [reference]."

Subagent 6 - API/Interface:
"Implement [endpoint/component] in [path].
Tests to pass: [test_name_4]
Connect to core logic via [interface].
Validate inputs using [validation approach]."
```

### UI Layer (if applicable)
```
Subagent 7 - Components:
"Create [component] in [path].
Tests to pass: [test_name_5]
Use existing design system components from [reference].
State management: [approach]."

Subagent 8 - Styling:
"Add styles for [component] following [style system].
Match existing patterns from [reference component]."
```

## Coordination Patterns

### Sequential Dependencies
When tasks depend on each other:
```
Phase 1: Foundation (parallel)
  → Checkpoint: Verify models and types compile
Phase 2: Core Logic (parallel)
  → Checkpoint: Verify unit tests pass
Phase 3: Integration (parallel)
  → Checkpoint: Verify integration tests pass
Phase 4: Polish (parallel)
  → Final: All tests pass, no warnings
```

### Shared State Communication
For complex features requiring coordination:
```
Create coordination file: [path]/feature-state.json

Subagent A writes after completing types:
{"types": "complete", "exports": ["TypeA", "TypeB"]}

Subagent B reads before implementing logic:
Check feature-state.json for available types before proceeding.

Subagent B writes after completing logic:
{"types": "complete", "logic": "complete", "main_function": "processFeature"}
```

### Conflict Prevention
When multiple subagents might touch related areas:
```
Option 1 - File Ownership:
"Subagent A owns [file1], Subagent B owns [file2].
Do not modify files outside your ownership."

Option 2 - Section Ownership:
"Subagent A: lines 1-100 of [file]
Subagent B: lines 150-250 of [file]
Do not modify outside your section."

Option 3 - Sequential Phases:
"Subagent A modifies [file] first.
After A completes, Subagent B modifies [file]."
```

## Test-First Implementation Pattern

When implementing to pass tests:
```
Subagent Instructions:
"Implement [function] to pass these tests: [list]

1. Read the test file first to understand expected behavior
2. Implement minimal code to pass tests
3. Do NOT modify the tests
4. Run tests after implementation
5. If tests fail, fix implementation, not tests
6. If tests seem wrong, report back without changes"
```

## Error Recovery

When a subagent reports failure:

### Retry Template
```
Previous attempt failed: [error description]

Revised instructions:
1. [Updated approach addressing failure]
2. Additional context: [what was learned]
3. Constraint: Avoid [what caused failure]

If this approach also fails, report back with:
- What was attempted
- Exact error message
- Suggested alternative approach
```

### Escalation Template
```
Subagent reported: [failure]

Assessment:
- Scope: Does this affect other subagents?
- Cause: [analysis]
- Options: [list alternatives]

Decision: [retry/reassign/pause all/escalate to user]
```

## Documentation Phase

After implementation completes:
```
Subagent - Inline Docs:
"Add documentation to [files]:
- Function docstrings explaining purpose and parameters
- Complex logic comments explaining 'why' not 'what'
- Type annotations where missing"

Subagent - External Docs:
"Update [documentation files]:
- README section for new feature
- API documentation with examples
- Migration notes if applicable"
```

## Checklist for Subagent Instructions

Every subagent task should include:
- [ ] Clear, specific task description
- [ ] Context about the feature being built
- [ ] Exact files and line numbers when applicable
- [ ] Tests to pass (for implementation tasks)
- [ ] Constraints on what NOT to do
- [ ] Verification step after completion
- [ ] What to do if blocked or confused
