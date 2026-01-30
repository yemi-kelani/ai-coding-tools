# Fix Issue: $ARGUMENTS

Take an extensive look at how the codebase works with regard to this issue. Look for bugs, logic mismatches, potential issues and document them in detail. Delegate fixes to several subagents to be fixed in parallel.

## ANALYZE FIRST

1. Find all files related to this issue. You may delegate this task to as many subagents as you deem necessary.
2. Trace the code flow and dependencies  
3. Identify root cause (not just symptoms)
4. Document current vs expected behavior
5. Attepmt to deeply understand the issue and existing codebase before devising a plan.
6. Take as much time as you deem necessary to plan.

## CREATE FIX PLAN

Requirements:
- Modify existing files (avoid creating new ones unless necessary or preferable)
- Simplify logic where possible, leave the codebase in better shape than you encountered it in.
- No technical debt
- Do not fake/mock your implementation. It must work as intended with the existing codebase
- Preserve existing functionality
- Follow project conventions
- Follow best practices. If there is room for improvement in the codebase, improve the code
- NEVER create plans to support backwards compatibility unless explicitly told by the user. Instead implement improvements directly.
- Remember to remove dead and outdated code, and clean things up where possible. Before removing code, research extensively to understand how its used first.
- Make sure to use DRY principles, and reuse/update existing methods before trying to create new ones. Centralize logic.
- Code in a manner that is best practice for the languages and tools used in this codebase.

Plan must include:
- Specific file changes with line numbers
- Test cases to add/modify
- Potential side effects and safeguards

## EXECUTE

Break into subtasks and deligate separate tasks to subagents in parallel:
1. [Analysis] Map affected code
2. [Design] Choose best approach
3. [Implement] Make minimal changes
4. [Test] Verify fix works

Reiterate this process as needed.

## TEST

Build to code base and ensure it builds once your done. Make sure to resolve ALL warnings and errors that occurr.