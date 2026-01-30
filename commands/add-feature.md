# Implement Feature: $ARGUMENTS

Thoroughly analyze the codebase to understand existing patterns, architecture, and conventions. Design the feature to integrate seamlessly with the current system. Delegate implementation tasks to subagents to work in parallel.

## ANALYZE FIRST

1. Understand the feature requirements completely
   - What problem does this solve?
   - What are the acceptance criteria?
   - What are the edge cases and constraints?

2. Map the existing codebase architecture
   - Find similar/related features for reference
   - Identify relevant modules, services, and data structures
   - Understand current patterns and conventions
   - Locate where this feature should integrate

3. Research dependencies and impacts
   - What existing code will this interact with?
   - What new dependencies might be needed?
   - What side effects could this create?
   - Are there performance considerations?

4. Take as much time as necessary to deeply understand the codebase before designing the solution

## DESIGN THE FEATURE

Create a comprehensive design that includes:

**Architecture Decisions:**
- Component structure and organization
- Data models and schemas
- API contracts (if applicable)
- Integration points with existing code
- State management approach

**Requirements:**
- Follow existing project patterns and conventions
- Reuse existing utilities and components (DRY principle)
- Design for maintainability and extensibility
- Keep it simple - avoid over-engineering
- No technical debt or shortcuts, implement real solutions
- Must integrate properly with existing codebase
- Consider scalability and performance
- Follow best practices for the language/framework

**Implementation Plan must include:**
- New files to create (if necessary) with purpose
- Existing files to modify with specific changes
- Shared utilities to create or update
- Test files and test cases to add
- Documentation to add or update
- Migration steps (if applicable)

## EXECUTE

Break the feature into logical subtasks and delegate to subagents in parallel:

1. [Foundation] Set up data models, types, and core utilities
2. [Core Logic] Implement main feature logic and business rules
3. [Integration] Connect feature to existing systems and APIs
4. [UI/Interface] Build user-facing components (if applicable)
5. [Testing] Write comprehensive tests (unit, integration, e2e)
6. [Documentation] Add inline docs, README updates, examples

Coordinate between subtasks to ensure:
- Consistent naming conventions
- Proper error handling
- Logging and observability
- Type safety (if applicable)
- Security considerations

Iterate and refine as needed based on discoveries during implementation.

## TEST & VALIDATE

1. Build the codebase and resolve ALL errors and warnings
2. Run existing tests to ensure no regressions
3. Run new tests to verify feature works as intended
4. Test edge cases and error scenarios
5. Verify integration points work correctly
6. Check performance under expected load (if relevant)
7. Validate against original requirements

## CLEANUP

Before finishing:
- Remove any debug code or commented-out code
- Ensure consistent formatting
- Verify all files follow project conventions
- Update any affected documentation
- Remove unused imports or dependencies
- Ensure code is production-ready

## FINAL REVIEW

- Does the feature meet all requirements?
- Is the code maintainable and readable?
- Are there adequate tests?
- Does it follow project conventions?
- Is the codebase left in better shape than before?