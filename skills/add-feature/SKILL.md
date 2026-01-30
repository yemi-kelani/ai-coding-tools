---
name: add-feature
description: >
  Systematic feature implementation for codebases. Use when asked to add new functionality, implement features, build capabilities, or extend existing systems. Emphasizes understanding before coding, TDD workflow via effective-testing skill, parallel subagent delegation, and seamless integration with existing architecture. Triggers on: implement, add feature, build, create functionality, extend, or any new capability request.
---

# Add Feature

Implement features systematically with test-driven development. **Critical principle: understand fully, design with tests, then implement.** Features built on specifications (tests) have fewer bugs and integrate cleanly.

## Workflow Overview

1. **Analyze** — Understand requirements and codebase completely
2. **Design** — Architecture decisions + test specifications (TDD)
3. **Implement** — Parallel subagents build to pass tests
4. **Verify** — Build, test, clean up

## Phase 1: Analyze (No Code Yet)

**Block all code generation until analysis completes.**

### 1.1 Understand Requirements

Before touching code, answer:
- What problem does this feature solve?
- What are the acceptance criteria?
- What are the edge cases and constraints?
- Who uses this and how?

If requirements are ambiguous, **ask ONE focused question** and wait for the answer.

### 1.2 Map Existing Architecture

Delegate exploration to subagents in parallel:
```
Subagent tasks (parallel):
- "Find similar/related features in the codebase and summarize their patterns"
- "List modules, services, and data structures this feature will interact with"
- "Identify existing utilities and components that can be reused"
- "Document current conventions: naming, file organization, error handling"
```

Do NOT write code—only read and report.

### 1.3 Research Dependencies and Impacts

Identify:
- What existing code will this interact with?
- What new dependencies might be needed?
- What side effects could this create?
- Are there performance considerations?
- Security implications?

Use extended thinking: include "think hard" for complex architectural decisions.

### 1.4 Confirm Understanding

Before proceeding, document:
- **Feature summary**: One paragraph describing what will be built
- **Integration points**: Where this connects to existing code
- **Reusable components**: Existing code to leverage (DRY)
- **New components needed**: What must be created
- **Risks**: Potential issues and mitigations

## Phase 2: Design with Tests (TDD)

**Tests are specifications, not afterthoughts.** Write tests before implementation code.

### 2.1 Architecture Decisions

Document:
- Component structure and organization
- Data models and schemas
- API contracts (if applicable)
- State management approach
- Integration strategy with existing code

### 2.2 Write Test Specifications

**Invoke the effective-testing skill workflow:**

1. **Gather test context** — Function signatures, expected behavior, edge cases
2. **Write tests first** — Tests define the contract
3. **Confirm tests fail** — For the right reason (missing implementation)
4. **Do NOT write implementation yet**

Test categories to cover:
- **Core functionality**: Main purpose, return values, realistic data
- **Input validation**: Invalid types, null/undefined, empty, boundaries
- **Error handling**: Expected exceptions, meaningful messages
- **Integration**: Interaction with existing systems

```
Test Specification Example:
- test_feature_handles_valid_input: Given [input], expect [output]
- test_feature_rejects_invalid_input: Given [bad input], expect [error]
- test_feature_integrates_with_existing: Given [context], expect [integration behavior]
```

### 2.3 Implementation Plan

After tests are written:
```
Implementation Plan:
1. New files to create:
   - [path]: [purpose]

2. Existing files to modify:
   - [path] line [N]: [change] because [reason]

3. Shared utilities to create/update:
   - [utility]: [purpose]

4. Documentation to add:
   - [location]: [content]
```

## Phase 3: Implement via Parallel Subagents

Now implement code that passes the tests.

### 3.1 Task Decomposition (SAOI)

Good task boundaries are:
- **Specific** — Agent knows exactly what to do
- **Achievable** — Within agent capabilities
- **Ordered** — Later steps can build on earlier ones
- **Independent** — Minimal dependencies between parallel tasks

See `references/parallel-patterns.md` for delegation templates.

### 3.2 Parallel Execution Pattern

```
Phase A (parallel, foundation):
- Subagent 1: Create data models, types, schemas
- Subagent 2: Set up core utilities and helpers
- Subagent 3: Create API contracts/interfaces

Phase B (parallel, core logic):
- Subagent 4: Implement main feature logic
- Subagent 5: Implement integration with existing systems
- Subagent 6: Build UI/interface components (if applicable)

Phase C (after implementation):
- Subagent 7: Run tests, fix failures
- Subagent 8: Add documentation and examples
```

### 3.3 Subagent Instructions Template

Every subagent task must include:
```
Task: [one-line description]

Context:
- Feature: [what we're building]
- Tests to pass: [relevant test names]

Specific instructions:
1. [Exact action with file and line numbers]
2. [Exact action]

Constraints:
- Do NOT modify tests to make them pass
- Follow existing project conventions
- Reuse [specific utilities] where applicable

Verification:
- After changes, run [test command] and confirm [expected result]
```

### 3.4 Implementation Requirements

- Follow existing project patterns and conventions
- Reuse existing utilities and components (DRY)
- Design for maintainability and extensibility
- Keep it simple—avoid over-engineering
- No technical debt or shortcuts
- Consider scalability and performance
- Implement proper error handling
- Add logging and observability
- Ensure type safety (if applicable)
- Address security considerations

## Phase 4: Verify

### 4.1 Build and Test

```bash
# Build the codebase
# Run full test suite
# Check for warnings/errors
```

Resolve ALL warnings and errors before completing.

### 4.2 Validate Against Requirements

- [ ] All acceptance criteria met
- [ ] Edge cases handled
- [ ] Error scenarios handled gracefully
- [ ] Integration points work correctly
- [ ] Performance acceptable under expected load

### 4.3 Clean Up

- Remove debug code and commented-out code
- Remove unused imports and dependencies
- Ensure consistent formatting
- Verify all files follow project conventions

### 4.4 Documentation

- Update README if needed
- Add inline documentation for complex logic
- Update API documentation (if applicable)
- Add usage examples

## Anti-Patterns to Avoid

1. **Skipping analysis** — Jumping to code without understanding requirements
2. **Tests after implementation** — Validates bugs as features
3. **Over-engineering** — Building for hypothetical future needs
4. **Ignoring existing patterns** — Creating inconsistency in codebase
5. **Mocking the wrong things** — Mock external dependencies only
6. **Breaking existing functionality** — Run full test suite to catch regressions

## Integration with effective-testing Skill

This skill delegates test creation to the **effective-testing** skill. When reaching Phase 2.2:

1. Invoke effective-testing workflow
2. Provide gathered context (signatures, behavior, edge cases)
3. Write tests following TDD principles
4. Confirm tests fail before implementation
5. Implementation phase makes tests pass

**Never modify tests to make them pass.** If tests seem wrong, discuss with user first.

## Resources

- `references/parallel-patterns.md` — Subagent delegation templates and coordination patterns
- `references/design-checklist.md` — Architecture decision checklist and common patterns
