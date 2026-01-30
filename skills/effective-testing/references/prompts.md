# Example Test Prompts

Effective prompts for common testing scenarios. Use these as templates.

## Starting TDD for a New Feature

```
I want to implement [feature description]. Let's do TDD.

First, write tests for these behaviors:
- [expected behavior 1]
- [expected behavior 2]
- [edge case 1]

Use [framework]. Don't write implementation yet.
```

## Adding Tests to Existing Code

```
Write tests for `[function_name]` in [file].

Expected behaviors:
- [what it should do with valid input]
- [what it should do with invalid input]
- [edge cases: empty, null, boundary values]

Match the testing patterns in [existing_test_file].
```

## Comprehensive Edge Case Coverage

```
Generate boundary value tests for [function/validator]:
- Test at min/max boundaries ([specific values])
- Test immediately outside boundaries
- Test type mismatches (string instead of number, etc.)
- Test null, undefined, empty string, empty array
- Test extreme values (0, -999, MAX_INT)
```

## Testing Error Handling

```
Write tests for error handling in [function]:
- What exception is thrown for [invalid input 1]?
- What exception is thrown for [invalid input 2]?
- Does the error message contain [expected info]?
- Is the error recoverable or fatal?
```

## Testing Async Operations

```
Write tests for the async function [name]:
- Test successful resolution with [expected data]
- Test timeout behavior after [N] seconds
- Test rejection with [error type]
- Test retry logic: fails twice, succeeds third time
- Test concurrent calls don't interfere
```

## API/Integration Tests

```
Write integration tests for [endpoint]:
- GET with valid params returns [expected structure]
- GET with invalid params returns 400
- Unauthorized request returns 401
- Not found returns 404
- Malformed request body returns 400 with [error format]
```

## Requesting Test Review

```
Review these tests for [function]:
- Which tests would actually catch a regression?
- Are any assertions too shallow?
- What edge cases am I missing?
- Are any tests testing implementation instead of behavior?
```

## After AI Generates Tests

```
For each test you just generated:
1. What specific bug would this test catch?
2. Would this test fail if I [specific code change]?
3. Is the assertion verifying behavior or just execution?
```

## Requesting Mutation Testing Setup

```
Set up mutation testing for [project/module]:
- What tool should I use for [language]?
- Which files should be targeted?
- What mutation score should I aim for?
- Show me how to run it and interpret results.
```

## Testing State Machines

```
Write tests for [state machine/workflow]:
- Test each valid state transition
- Test invalid transitions are rejected
- Test initial state is correct
- Test terminal states cannot transition
- Test idempotency of transitions
```

## Testing Data Transformations

```
Write tests for [transformation function]:
- Input [example 1] → Output [expected 1]
- Input [example 2] → Output [expected 2]
- Preserves [invariant] during transformation
- Handles missing fields by [expected behavior]
- Round-trip: transform then reverse equals original
```

## Prompt Improvements That Help AI

| Instead of | Say |
|-----------|-----|
| "add tests" | "Write tests for [specific behaviors]" |
| "comprehensive tests" | "Tests covering [list specific cases]" |
| "good tests" | "Tests that would fail if [specific bug]" |
| "test this" | "Test that [function] returns [X] when [Y]" |
| "fix the tests" | "Update tests to expect [new behavior]" |

## Questions to Ask After Generation

1. "Is there anything I'm not testing?"
2. "Which of these tests is most valuable?"
3. "Would any of these tests pass even if the code was broken?"
4. "Are we testing behavior or implementation details?"
5. "What's the minimal set of tests that would catch real bugs?"
