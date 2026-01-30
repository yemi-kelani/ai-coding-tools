---
name: effective-testing
description: Write meaningful tests that catch real bugs. Use when writing tests, adding test coverage, creating unit/integration tests, or when asked to "add tests" without specific guidance. Enforces TDD-first workflow, requires context before generation, focuses on behavioral contracts over implementation details, and validates tests catch actual bugs. Triggers on any test-related request including ambiguous ones like "write tests for this."
---

# Effective Testing

## Core Principle

**Tests are specifications, not afterthoughts.**

Every test must answer: "What bug would this catch?" If you cannot articulate a specific failure mode the test guards against, the test has no value. Tests validate *contracts*, not *code paths*.

Research shows AI-generated tests have **45% pass rate with proper context** vs **7.5% without**. Context provision is the single most important factor in test quality.

## TDD Workflow (Required)

Follow this workflow for all test generation. Do not skip steps.

### Step 1: Gather Context

Before writing any test, ensure you have:
- [ ] Function signatures with type hints
- [ ] Docstrings explaining intended behavior (what it *should* do, not just what it does)
- [ ] Existing test files in the project (patterns to follow)
- [ ] Expected inputs and outputs for complex logic
- [ ] Edge cases the user cares about

**If context is missing, ask.** See "When to Ask for Clarification" below.

### Step 2: Write Tests First

Write tests based on expected behavior. Be explicit:
- State you are doing TDD
- Do NOT create mock implementations
- Do NOT write implementation code yet

```
# Example test-first approach
def test_validates_age_rejects_negative():
    """Age validator should reject negative values."""
    with pytest.raises(ValidationError):
        validate_age(-1)
```

### Step 3: Confirm Tests Fail

Run tests and verify they fail *for the right reason*:
- Missing function → expected
- Wrong assertion → fix the test
- Do NOT write implementation yet

### Step 4: Implementation (Separate Step)

Only after tests are committed, write implementation that passes tests.
- Do NOT modify the tests to make them pass
- If tests seem wrong, discuss with user first

### Step 5: Verify Against Overfitting

Check that implementation doesn't "game" the tests:
- Would tests catch off-by-one errors?
- Would tests catch null handling bugs?
- Consider running mutation testing

## When to Ask for Clarification

**Always ask when:**
- The function's contract is unclear from code/comments
- Multiple valid interpretations of "correct" exist
- Error handling expectations are unspecified
- Business rules aren't documented
- Edge case behavior is ambiguous

**Frame questions around behavior:**
- "Should this throw or return null for empty input?"
- "What's the expected behavior for duplicate entries?"
- "Should validation happen before or after transformation?"

**Ask ONE focused question, not a list.** Get the answer, then proceed.

**Never generate tests from ambiguous requests** like "add tests" or "write comprehensive tests" without understanding what behaviors matter.

## Context That Improves Test Quality

### Effective Prompt Structure

```
❌ Poor: "add tests for foo.py"

✅ Good: "Write tests for validate_payment(amount, currency) covering:
- Valid inputs with realistic data
- Negative amounts (should reject)
- Zero values (edge case)
- Empty/null currency strings
- Currency case sensitivity
Use pytest with AAA pattern. Avoid mocks."
```

### Four-Tier Test Strategy

For each function, consider:

1. **Core functionality**: Main purpose, return values, realistic data
2. **Input validation**: Invalid types, null/undefined, empty, boundaries
3. **Error handling**: Expected exceptions, meaningful error messages
4. **Side effects**: External calls, state changes, dependency interactions

## Anti-Patterns to Avoid

### Shallow Assertions (Coverage Theater)

```python
# ❌ Passes even when code is broken
def test_process_data():
    result = process_data(input)
    assert result is not None
    assert isinstance(result, dict)

# ✅ Validates actual behavior
def test_process_data_preserves_keys():
    result = process_data({"user": "alice", "age": 30})
    assert result["user"] == "alice"
    assert result["age"] == 30
    assert "processed_at" in result
```

### Testing Implementation Instead of Behavior

```python
# ❌ Breaks on refactoring even when behavior unchanged
def test_uses_internal_cache():
    obj = MyClass()
    obj.process()
    assert obj._internal_cache is not None  # Private state

# ✅ Tests observable behavior
def test_second_call_returns_same_result():
    obj = MyClass()
    first = obj.process()
    second = obj.process()
    assert first == second
```

### Over-Mocking

Mock only external dependencies (APIs, databases, third-party services). Never mock:
- The code being tested
- Simple value objects
- Internal methods of the class under test

### The Verification-Validation Trap

AI tests verify what code *does*, not what it *should* do. If you test after implementation, you may validate bugs as features.

**Solution:** Write tests first, or have user specify expected behavior before looking at implementation.

## Assertion Responsibility

**AI generates test structure. Humans must validate assertions.**

Before finalizing any test:
- [ ] Does assertion validate actual behavior, not just types?
- [ ] Would this test fail if core logic broke?
- [ ] Is test name describing expected behavior clearly?
- [ ] Would this test survive refactoring?

When uncertain about what to assert, ask: "What specific value or state change should I verify here?"

## Test Validation with Mutation Testing

Code coverage shows where tests *aren't*. Mutation testing shows if tests catch bugs.

Suggest mutation testing when:
- User asks for "comprehensive" or "thorough" tests
- High-value code (payments, auth, data integrity)
- Refactoring legacy code

Tools by language:
- **JavaScript/TypeScript**: Stryker
- **Python**: mutmut
- **Java**: PIT/Pitest
- **Rust**: cargo-mutants

## Test Organization

```
describe('[Component/Module]', () => {
  describe('[Behavior being tested]', () => {
    it('[expected outcome] when [condition]', () => {})
  })
})
```

Group by behavior, not by method. One assertion per test when possible.

## CLAUDE.md Integration

Recommend users add testing instructions to CLAUDE.md:

```markdown
# Testing
- Framework: pytest (Python) / Jest (JS) / flutter_test (Dart)
- Pattern: AAA (Arrange, Act, Assert)
- Mocking: Only external dependencies
- Coverage: Run with --coverage flag
- Naming: test_[behavior]_[condition]_[expected]
```

Add any test anti-patterns discovered during the project to prevent repetition.

## References

- `references/edge-cases.md` - Comprehensive edge case patterns by data type
- `references/language-specific.md` - Framework-specific guidance for Python, TypeScript, Rust, Dart
- `references/prompts.md` - Example prompts for common testing scenarios
