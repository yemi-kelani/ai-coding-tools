# Simplify & Optimize Code

Simplify, optimize, and refine code for clarity, performance, and maintainability while preserving exact functionality.

## Focus

$ARGUMENTS

If no focus is specified, analyze the current file or most recent code context and identify the highest-impact optimization targets before proceeding.

## Principles

**Preserve Functionality**
Never change what the code does—only how it does it. All original features, outputs, and behaviors must remain intact.

**Clarity Over Brevity**
Explicit, readable code beats clever one-liners. Avoid nested ternaries, dense chains, and "golf" solutions that sacrifice debuggability for fewer lines.

**Don't Repeat Yourself**
Extract repeated logic into reusable functions, constants, or modules. But don't over-abstract—if something is used twice and might diverge later, duplication can be the right call. Three or more usages is the threshold for extraction.

**Remove Before Refactoring**
Eliminate dead code, redundant abstractions, unused imports, and comments that describe obvious behavior before restructuring what remains.

**Optimize Deliberately**
Prefer algorithmic improvements (O(n²) → O(n)) over micro-optimizations. Cache expensive computations. Avoid unnecessary allocations in hot paths. For code without profiler data, optimize conservatively—favor readability unless the path is obviously hot (loops over large collections, request handlers, render cycles).

**Respect Existing Patterns**
Follow conventions already established in the codebase and CLAUDE.md. Don't impose new patterns unless the old ones are actively harmful.

**Stay in Scope**
Optimize the targeted code. If improvements require changes to code outside the focus area, flag them for a separate pass rather than expanding scope mid-task.

## Process

1. Identify what the code is trying to accomplish
2. Research language/framework best practices if unfamiliar with the codebase conventions
3. Flag unnecessary complexity, duplication, performance issues, or unclear naming
4. Consolidate repeated logic—extract shared functions, constants, or types
5. Apply algorithmic or structural optimizations where impactful
6. Simplify remaining code while preserving behavior
7. Verify the result:
   - Run existing tests if available
   - If no tests exist, flag untested behavioral assumptions
   - Confirm the result is actually better—not just shorter or "cleverer"

## Output Format

1. **Optimized code** (the actual result)

2. **Changelog** organized by category:
   - **Removed**: Dead code, unused imports, redundant abstractions
   - **Extracted**: Consolidated logic, new shared functions/constants/types
   - **Restructured**: Clarity and structural improvements
   - **Optimized**: Performance improvements with expected impact

3. **Tradeoffs & Flags**:
   - Behavioral edge cases or assumptions
   - Readability costs of optimizations
   - Out-of-scope issues discovered (candidates for separate passes)
   - New dependencies with justification (if any)
   - Untested code paths (if no test coverage exists)

## Constraints

- No new dependencies without explicit justification (document in Tradeoffs)
- No behavioral changes unless fixing an obvious bug (flag these explicitly)
- Don't extract abstractions for code used fewer than 3 times
- Don't expand scope beyond the focus area—flag adjacent issues instead
- If no meaningful improvements are possible, say so and explain why

## Resources

Use all available tools to do this well:

- Use subagents freely for parallel analysis
- Create scratch files in `/temp_scratchpad` for notes or intermediate work
- Take your time—repeat the optimization process as many times as necessary