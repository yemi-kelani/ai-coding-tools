# Design Checklist

Architecture decision guidance and common patterns for feature implementation.

## Pre-Design Checklist

Before designing, confirm you have:

### Requirements Understanding
- [ ] Clear problem statement
- [ ] Acceptance criteria defined
- [ ] Edge cases identified
- [ ] User workflows understood
- [ ] Performance requirements known
- [ ] Security requirements known

### Codebase Understanding
- [ ] Similar features reviewed for patterns
- [ ] Integration points identified
- [ ] Reusable components catalogued
- [ ] Naming conventions documented
- [ ] Error handling patterns understood
- [ ] Testing patterns understood

## Architecture Decision Framework

For each major decision, document:

```
Decision: [What are you deciding?]

Context: [Why is this decision needed?]

Options:
1. [Option A]: [Pros] / [Cons]
2. [Option B]: [Pros] / [Cons]
3. [Option C]: [Pros] / [Cons]

Decision: [Which option and why]

Consequences: [What this means for implementation]
```

## Common Design Patterns

### Component Organization

**By Feature (Recommended for most cases):**
```
features/
  user-auth/
    components/
    services/
    types/
    tests/
  payments/
    components/
    services/
    types/
    tests/
```

**By Layer (For highly shared components):**
```
components/     # UI components
services/       # Business logic
repositories/   # Data access
types/          # Type definitions
```

### Data Flow Patterns

**Unidirectional (Recommended):**
```
User Action → Handler → Service → State → UI Update
```

**Request/Response:**
```
Client → Validate → Process → Persist → Response
```

### Error Handling Strategies

**Fail Fast:**
```
Validate at boundaries, throw immediately on invalid input.
Use when: Data integrity is critical, early feedback preferred.
```

**Graceful Degradation:**
```
Catch errors, provide fallback behavior, log for debugging.
Use when: Availability is critical, partial functionality acceptable.
```

**Result Types:**
```
Return Result<T, Error> instead of throwing.
Use when: Errors are expected part of flow, caller must handle.
```

## Integration Patterns

### With Existing Services

**Adapter Pattern:**
```
Create adapter to translate between new feature and existing service.
Keeps new code clean, isolates changes.
```

**Facade Pattern:**
```
Create simplified interface to complex subsystem.
Hides complexity, provides clean API.
```

### With External APIs

**Anti-Corruption Layer:**
```
Create boundary that translates external concepts to internal.
Protects codebase from external changes.
```

**Circuit Breaker:**
```
Detect failures, fail fast, recover gracefully.
Use for unreliable external dependencies.
```

## Quality Checklist

### Before Implementation

- [ ] Design follows existing codebase patterns
- [ ] DRY: Reusing existing components where possible
- [ ] KISS: Simplest solution that meets requirements
- [ ] YAGNI: Not building for hypothetical future needs
- [ ] Tests written as specifications
- [ ] Edge cases covered in test specifications

### During Implementation

- [ ] Consistent naming with existing code
- [ ] Proper error handling at all boundaries
- [ ] Input validation where data enters system
- [ ] Logging at key decision points
- [ ] Type safety maintained (if applicable)
- [ ] No hardcoded values that should be configurable

### After Implementation

- [ ] All tests pass
- [ ] No new warnings
- [ ] Code review checklist passed
- [ ] Documentation updated
- [ ] No debug code remaining
- [ ] Performance acceptable

## Common Pitfalls

### Over-Engineering
**Signs:** Abstract base classes for single implementations, plugin systems for static features, generic solutions for specific problems.
**Fix:** Implement for current requirements. Refactor when patterns emerge.

### Under-Engineering
**Signs:** Copy-paste code, inline magic values, no error handling, no tests.
**Fix:** Apply DRY, extract constants, add error boundaries, write tests first.

### Inconsistency
**Signs:** Multiple patterns for same problem, different naming conventions, mixed error strategies.
**Fix:** Follow existing patterns. When in doubt, match nearby code.

### Premature Optimization
**Signs:** Complex caching before measuring, micro-optimizations, sacrificing readability.
**Fix:** Measure first. Optimize only proven bottlenecks. Keep code readable.

## Scalability Considerations

Only address when requirements demand:

### Data Volume
- Pagination for lists
- Lazy loading for large datasets
- Caching strategies

### Request Volume
- Rate limiting
- Queue-based processing
- Horizontal scaling approach

### Complexity Growth
- Clean interfaces for future extension
- Configuration over hardcoding
- Feature flags for gradual rollout

## Security Checklist

Review before finalizing design:

- [ ] Authentication: Who can access?
- [ ] Authorization: What can they do?
- [ ] Input validation: All external input sanitized?
- [ ] Output encoding: XSS prevention?
- [ ] Data protection: Sensitive data handled properly?
- [ ] Logging: No sensitive data in logs?
- [ ] Dependencies: Known vulnerabilities checked?
