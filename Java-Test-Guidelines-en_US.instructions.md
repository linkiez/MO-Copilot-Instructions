---
applyTo: '**/test/**/*.java'
---
# Optimized Test Generation Guidelines for Copilot

## Core Principles

### 1. Test Structure (AAA Pattern)

- **Arrange**: Set up test data and mocks
- **Act**: Execute the method under test
- **Assert**: Verify expected outcomes with descriptive messages
- Use `@DisplayName` for clear test documentation

### 2. JUnit 5 Best Practices

```java
@ExtendWith(MockitoExtension.class)
class ServiceTest {
    @Mock
    private DependencyService dependencyService;
    
    @InjectMocks
    private ServiceUnderTest serviceUnderTest;
    
    @BeforeEach
    void setUp() {
        // Common setup
    }
    
    @AfterEach
    void tearDown() {
        SecurityContextHolder.clearContext(); // Prevent test contamination
    }
}
```

### 3. Mockito Guidelines

- Use specific argument matchers instead of `anyString()`
- Place `@Mock` annotations on separate lines
- Use static imports: `import static org.mockito.Mockito.*;`
- Extract complex mock setup into helper methods

### 4. Test Data Management

- Create test data builders/factories for complex objects
- Use descriptive variable names (`documentOrNull` vs `doc`)
- Avoid magic numbers - use constants
- Extract test data creation into helper methods

## Testing Patterns

### 5. Parameterized Testing

```java
@ParameterizedTest
@DisplayName("Should validate various input scenarios")
@NullAndEmptySource
@ValueSource(strings = {"", " ", "invalid-format"})
void shouldValidateInputs(String input) {
    // Test implementation
}

@ParameterizedTest
@MethodSource("provideTestScenarios")
void shouldHandleComplexScenarios(TestScenario scenario) {
    // Test implementation
}

private static Stream<TestScenario> provideTestScenarios() {
    return Stream.of(/* test scenarios */);
}
```

### 6. Essential Test Categories

- **Happy Path**: Valid inputs, successful execution
- **Validation**: Input validation, business rules
- **Edge Cases**: Boundary values, null/empty inputs
- **Error Handling**: Exception scenarios with `assertThrows()`
- **Integration**: Component interactions

### 7. Exception Testing

```java
@Test
@DisplayName("Should throw IllegalArgumentException for null input")
void shouldThrowExceptionForNullInput() {
    IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class,
        () -> service.process(null),
        "Expected IllegalArgumentException for null input"
    );
    assertThat(exception.getMessage()).contains("Input cannot be null");
}
```

## Code Quality Standards

### 8. Method Naming

Use descriptive test names that explain the scenario:

- ✅ `shouldReturnUserWhenValidIdProvided()`
- ✅ `shouldThrowExceptionWhenUserNotFound()`
- ❌ `testGetUser()` or `test1()`

### 9. Test Organization

- Single responsibility per test method
- Group related tests with nested classes
- Extract common setup into `@BeforeEach`
- Keep test classes focused and manageable

### 10. Assertions Best Practices

```java
// Use descriptive assertions
assertThat(result)
    .as("User should be active after registration")
    .isNotNull()
    .extracting(User::getStatus)
    .isEqualTo(UserStatus.ACTIVE);

// Verify mock interactions
verify(notificationService, times(1))
    .sendNotification(eq(userId), any(NotificationRequest.class));
```

## Anti-Patterns to Avoid

### ❌ Don't Do

- Test multiple scenarios in one method
- Use generic assertion messages
- Test implementation details
- Create overly complex setups
- Use production data
- Test framework code instead of business logic
- Suppress warnings without addressing root cause
- Use magic numbers or hardcoded values

### ✅ Do

- Write focused, single-purpose tests
- Test behavior and outcomes
- Use descriptive names and documentation
- Create isolated, independent tests
- Mock external dependencies appropriately
- Verify both positive and negative scenarios
- Make utility classes static with private constructors

## Test Generation Template

```java
/**
 * Test class for {@link ServiceUnderTest}.
 * Validates [specific functionality/behavior].
 */
@ExtendWith(MockitoExtension.class)
@DisplayName("ServiceUnderTest")
class ServiceUnderTestTest {

    @Mock
    private ExternalService externalService;

    @InjectMocks
    private ServiceUnderTest serviceUnderTest;

    @Test
    @DisplayName("Should successfully process valid request")
    void shouldProcessValidRequest() {
        // Arrange
        var request = createValidRequest();
        when(externalService.call(any())).thenReturn(createSuccessResponse());

        // Act
        var result = serviceUnderTest.process(request);

        // Assert
        assertThat(result)
            .as("Result should contain expected data")
            .isNotNull()
            .satisfies(r -> {
                assertThat(r.getStatus()).isEqualTo(Status.SUCCESS);
                assertThat(r.getData()).isNotEmpty();
            });
        
        verify(externalService).call(request);
    }

    @ParameterizedTest
    @DisplayName("Should throw exception for invalid inputs")
    @NullAndEmptySource
    @ValueSource(strings = {"invalid", ""})
    void shouldThrowExceptionForInvalidInputs(String invalidInput) {
        // Arrange & Act & Assert
        assertThatThrownBy(() -> serviceUnderTest.process(invalidInput))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessageContaining("Invalid input");
    }

    // Helper methods
    private RequestType createValidRequest() {
        return RequestType.builder()
            .field1("valid-value")
            .field2(123)
            .build();
    }

    private ResponseType createSuccessResponse() {
        return ResponseType.builder()
            .status(Status.SUCCESS)
            .data(List.of("item1", "item2"))
            .build();
    }
}
```

## Quick Reference Checklist

When generating tests, ensure:

- [ ] Clear test method names with `@DisplayName`
- [ ] Proper mock setup with specific matchers
- [ ] Both positive and negative test cases
- [ ] Edge cases and boundary conditions
- [ ] Exception scenarios with proper assertions
- [ ] Descriptive variable names
- [ ] Helper methods for test data creation
- [ ] Proper cleanup in `@AfterEach`
- [ ] Static imports for common Mockito methods
- [ ] Builder patterns for complex objects

This optimized guide focuses on practical, actionable guidelines that improve test quality while maintaining readability and maintainability.
