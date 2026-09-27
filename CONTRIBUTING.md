# Contributing to Payments Management System

First off, thank you for considering contributing to the Payments Management System! It's people like you that make this project better for everyone.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Getting Started

### Prerequisites

- Java 11 or higher
- Maven 3.6+
- MySQL 8.0+
- Git
- Your favorite IDE (IntelliJ IDEA recommended)

### Development Setup

1. **Fork the repository**

   Click the "Fork" button at the top right of the repository page.

2. **Clone your fork**

   ```bash
   git clone https://github.com/YOUR_USERNAME/Payments-Management-System.git
   cd Payments-Management-System
   ```

3. **Add upstream remote**

   ```bash
   git remote add upstream https://github.com/ayushmaanbhav/Payments-Management-System.git
   ```

4. **Set up the database**

   ```bash
   # Create a MySQL database
   mysql -u root -p -e "CREATE DATABASE payments_dev;"

   # Run migrations
   mvn -pl ayushmaanbhav-database spring-boot:run
   ```

5. **Build the project**

   ```bash
   mvn clean install
   ```

6. **Run tests**

   ```bash
   mvn test
   ```

## How to Contribute

### Reporting Bugs

Before creating bug reports, please check the [existing issues](https://github.com/ayushmaanbhav/Payments-Management-System/issues) to avoid duplicates.

When creating a bug report, include:

- A clear and descriptive title
- Steps to reproduce the behavior
- Expected behavior
- Actual behavior
- Java version, OS, and other relevant environment details
- Any relevant logs or error messages

### Suggesting Features

Feature requests are welcome! Please:

1. Check if the feature has already been requested
2. Clearly describe the use case
3. Explain why this feature would be useful to most users

### Contributing Code

1. **Find an issue to work on**
   - Look for issues labeled `good first issue` or `help wanted`
   - Comment on the issue to let others know you're working on it

2. **Create a branch**

   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

3. **Make your changes**
   - Follow the coding standards
   - Write tests for your changes
   - Update documentation as needed

4. **Commit your changes**

   ```bash
   git add .
   git commit -m "feat: add amazing feature"
   ```

   We follow [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat:` for new features
   - `fix:` for bug fixes
   - `docs:` for documentation changes
   - `test:` for test changes
   - `refactor:` for refactoring
   - `chore:` for maintenance tasks

5. **Push and create a Pull Request**

   ```bash
   git push origin feature/your-feature-name
   ```

## Pull Request Process

1. **Before submitting**
   - Ensure all tests pass: `mvn test`
   - Update documentation if needed
   - Add tests for new functionality
   - Rebase on the latest master: `git rebase upstream/master`

2. **PR Requirements**
   - Fill out the PR template completely
   - Link related issues
   - Provide a clear description of changes
   - Include screenshots for UI changes

3. **Review Process**
   - At least one maintainer approval required
   - All CI checks must pass
   - Address reviewer feedback promptly

4. **After Merge**
   - Delete your feature branch
   - Sync your fork with upstream

## Coding Standards

### Java Style Guide

We follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) with these additions:

- Use 4 spaces for indentation (not tabs)
- Maximum line length: 120 characters
- Use meaningful variable and method names
- Add Javadoc for public APIs

### Code Structure

```java
@Service
@RequiredArgsConstructor
@FieldDefaults(level = AccessLevel.PRIVATE, makeFinal = true)
public class MyService {

    // Dependencies (injected via constructor)
    MyRepository repository;
    MyMapper mapper;

    // Public methods first
    public MyDto doSomething(MyRequest request) {
        // Implementation
    }

    // Private methods last
    private void helperMethod() {
        // Implementation
    }
}
```

### Lombok Usage

We use Lombok to reduce boilerplate:

```java
@Data                           // For DTOs
@Builder                        // For builders
@RequiredArgsConstructor        // For constructor injection
@FieldDefaults(level = AccessLevel.PRIVATE, makeFinal = true)  // For final fields
```

### Exception Handling

- Use specific exception types
- Include meaningful error messages
- Log exceptions at appropriate levels

```java
public Account getAccount(String accountId) {
    return repository.findByExternalId(accountId)
        .orElseThrow(() -> new AccountNotFoundException(
            "Account not found: " + accountId
        ));
}
```

## Testing Guidelines

### Test Structure

```
src/test/java/
├── com/ayushmaanbhav/{module}/
│   ├── service/           # Service layer tests
│   ├── controller/        # Controller tests
│   ├── mapper/            # Mapper tests
│   ├── repository/        # Repository tests (integration)
│   └── testsetup/         # Test data builders
```

### Writing Tests

```java
@ExtendWith(MockitoExtension.class)
class MyServiceTest {

    @Mock
    MyRepository repository;

    @InjectMocks
    MyService service;

    @Test
    void shouldDoSomething_whenValidInput() {
        // Given
        var input = createTestInput();
        when(repository.findById(any())).thenReturn(Optional.of(createTestEntity()));

        // When
        var result = service.doSomething(input);

        // Then
        assertThat(result).isNotNull();
        assertThat(result.getField()).isEqualTo("expected");
        verify(repository).findById(input.getId());
    }
}
```

### Test Naming

Use descriptive names: `should{ExpectedBehavior}_when{Condition}`

- `shouldCreatePaymentOrder_whenValidRequest`
- `shouldThrowException_whenAccountNotFound`
- `shouldReturnEmpty_whenNoResultsFound`

### Test Coverage

- Minimum 80% coverage for new code
- Focus on business logic, not getters/setters
- Include edge cases and error scenarios

## Documentation

### Code Documentation

- Add Javadoc for public APIs
- Include `@param`, `@return`, and `@throws` tags
- Keep comments up to date with code changes

```java
/**
 * Creates a new payment order.
 *
 * @param request the payment order request containing order details
 * @return the created payment order response
 * @throws PaymentException if payment creation fails
 */
public PaymentOrderResponse createPayment(PaymentOrderRequest request) {
    // Implementation
}
```

### README Updates

- Update README for new features
- Include usage examples
- Document configuration options

### API Documentation

- OpenAPI/Swagger annotations for all endpoints
- Include request/response examples
- Document error responses

## Questions?

Feel free to:
- Open a [Discussion](https://github.com/ayushmaanbhav/Payments-Management-System/discussions)
- Ask in an issue
- Reach out to maintainers

Thank you for contributing!
