# Language-Specific Testing Guidance

Framework-specific patterns, tools, and AI considerations by language.

## Python

### Framework: pytest (preferred)

```python
# Fixtures for shared setup
@pytest.fixture
def sample_user():
    return User(name="Alice", age=30)

# Parametrized tests for multiple inputs
@pytest.mark.parametrize("input,expected", [
    (0, "zero"),
    (1, "one"),
    (-1, "negative"),
])
def test_describe_number(input, expected):
    assert describe_number(input) == expected

# AAA Pattern
def test_user_can_update_email():
    # Arrange
    user = User(email="old@example.com")
    
    # Act
    user.update_email("new@example.com")
    
    # Assert
    assert user.email == "new@example.com"
```

### AI Considerations
- AI often generates hardcoded values instead of fixtures—explicitly request `@pytest.fixture`
- Ask for `@pytest.mark.parametrize` for multiple test cases
- AI handles pytest well due to abundant training data

### Tools
- Coverage: `pytest-cov`
- Mutation: `mutmut`
- Mocking: `pytest-mock`, `unittest.mock`

### Common Mistakes
- Using `assert True` after code runs (shallow)
- Not using `pytest.raises` for exception testing
- Mocking too much internal state

---

## TypeScript / JavaScript

### Framework: Jest or Vitest

```typescript
// Descriptive test structure
describe('PaymentProcessor', () => {
  describe('processPayment', () => {
    it('should return success for valid payment', async () => {
      const processor = new PaymentProcessor();
      const result = await processor.processPayment({
        amount: 100,
        currency: 'USD'
      });
      expect(result.status).toBe('success');
      expect(result.transactionId).toBeDefined();
    });

    it('should reject negative amounts', async () => {
      const processor = new PaymentProcessor();
      await expect(
        processor.processPayment({ amount: -50, currency: 'USD' })
      ).rejects.toThrow('Invalid amount');
    });
  });
});
```

### AI Considerations
- Best AI support of any language—Copilot's strongest language
- TypeScript types significantly improve AI test accuracy
- AI leverages types to reduce null/undefined test requirements
- For React: explicitly mention React Testing Library patterns

### Tools
- Coverage: built into Jest/Vitest
- Mutation: Stryker
- Mocking: Jest mocks, `msw` for API mocking

### Common Mistakes
- Using `toBeTruthy()` instead of specific value assertions
- Not awaiting async operations
- Over-mocking with `jest.mock()` for everything

---

## Rust

### Framework: Built-in `#[test]`

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_valid_input_returns_ok() {
        let result = validate_input("valid");
        assert!(result.is_ok());
        assert_eq!(result.unwrap(), "valid");
    }

    #[test]
    #[should_panic(expected = "empty input")]
    fn test_empty_input_panics() {
        validate_input("");
    }

    #[test]
    fn test_error_contains_message() {
        let result = validate_input("");
        assert!(result.is_err());
        assert!(result.unwrap_err().to_string().contains("empty"));
    }
}
```

### AI Considerations
- More limited training data than Python/JS
- Compiler catches many AI errors via type system and borrow checker
- AI struggles with lifetime annotations—may need manual fixes
- Property-based testing with `proptest` works well with AI

### Tools
- Coverage: `cargo-tarpaulin`
- Mutation: `cargo-mutants`
- Linting: `cargo clippy` (run as guardrail on AI code)
- Property testing: `proptest`, `quickcheck`

### Common Mistakes
- AI may generate code that doesn't compile (lifetimes, borrows)
- Using `unwrap()` in tests without checking `is_ok()` first
- Not testing `Result` and `Option` exhaustively

### Rust-Specific Patterns
```rust
// Test both Ok and Err paths
#[test]
fn test_result_handling() {
    // Success case
    assert!(parse_config("valid").is_ok());
    
    // Error case - check error type
    let err = parse_config("invalid").unwrap_err();
    assert!(matches!(err, ConfigError::InvalidFormat(_)));
}
```

---

## Dart / Flutter

### Framework: `flutter_test` / `test`

```dart
// Unit test
void main() {
  group('Counter', () {
    test('initial value is 0', () {
      final counter = Counter();
      expect(counter.value, 0);
    });

    test('increment increases value', () {
      final counter = Counter();
      counter.increment();
      expect(counter.value, 1);
    });
  });
}

// Widget test
void main() {
  testWidgets('MyWidget displays text', (tester) async {
    await tester.pumpWidget(const MyApp());
    
    expect(find.text('Hello'), findsOneWidget);
    
    await tester.tap(find.byType(ElevatedButton));
    await tester.pump();
    
    expect(find.text('Tapped'), findsOneWidget);
  });
}
```

### AI Considerations
- Moderate AI support, growing training data
- Widget tests generate well once patterns are established
- AI knowledge limited to training cutoffs—newer packages have reduced accuracy
- Use custom instructions file for Flutter-specific rules

### Tools
- Coverage: `flutter test --coverage`
- Mocking: `mocktail`, `mockito`
- Widget testing: `flutter_test` (built-in)
- Integration: `integration_test` package

### Common Mistakes
- Forgetting `pump()` or `pumpAndSettle()` after state changes
- Not wrapping widgets in `MaterialApp` for widget tests
- Using real HTTP in unit tests instead of mocking

### Flutter-Specific Patterns
```dart
// Provider testing
testWidgets('uses provider value', (tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        myProvider.overrideWithValue(MockValue()),
      ],
      child: const MyApp(),
    ),
  );
  // assertions...
});

// Golden tests for UI
testWidgets('matches golden', (tester) async {
  await tester.pumpWidget(const MyWidget());
  await expectLater(
    find.byType(MyWidget),
    matchesGoldenFile('goldens/my_widget.png'),
  );
});
```

---

## Cross-Language Principles

Regardless of language:

1. **One behavior per test** - easier to diagnose failures
2. **Descriptive names** - `test_rejects_negative_amount` not `test1`
3. **AAA pattern** - Arrange, Act, Assert (or Given/When/Then)
4. **Minimal mocking** - only external dependencies
5. **Fast execution** - unit tests should run in milliseconds
6. **Deterministic** - no flaky tests from timing or randomness
