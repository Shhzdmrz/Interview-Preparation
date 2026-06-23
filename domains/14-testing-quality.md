# Domain 14 — Testing & Quality

## Goal

Be able to explain how you test .NET applications at unit, integration, API, and performance levels.

## Topic Priority Legend

- **[High]** — Must know for interviews. Learn and practice first.
- **[Medium]** — Learn after the High-priority topics are comfortable.

## Core Testing Concepts

- [High] Unit tests vs integration tests vs end-to-end tests
- [High] Test pyramid
- [High] Arrange, Act, Assert pattern
- [High] What should and should not be unit tested
- [High] Deterministic and isolated tests
- [High] Testing error paths and edge cases
- [Medium] Code coverage and its limitations

## xUnit and .NET Testing

- [High] xUnit fundamentals
- [High] `[Fact]` vs `[Theory]`
- [High] Test data with `[InlineData]`
- [High] Setup and teardown using constructors and `IDisposable`
- [Medium] Shared fixtures with `IClassFixture<T>`
- [Medium] Async test methods

## Mocking

- [High] Mock, stub, and fake differences
- [High] Moq or NSubstitute fundamentals
- [High] Mocking dependencies through interfaces
- [High] Verifying calls and expected behavior
- [High] What not to mock
- [Medium] Risks of over-mocking implementation details

## ASP.NET Core Integration Testing

- [High] `WebApplicationFactory<TEntryPoint>`
- [High] Testing HTTP endpoints and status codes
- [High] Replacing production dependencies in tests
- [High] Testing authentication and authorization
- [Medium] Testing middleware and exception handling
- [Medium] Testing database-backed APIs

## Database and Infrastructure Testing

- [High] In-memory provider limitations
- [Medium] SQLite in-memory integration testing
- [Medium] Testcontainers fundamentals
- [Medium] Running SQL Server, PostgreSQL, Redis, or RabbitMQ in test containers
- [High] Test data isolation and cleanup

## Performance and Quality

- [Medium] Load testing vs stress testing vs spike testing
- [Medium] k6 or JMeter fundamentals
- [Medium] Performance baselines and bottleneck measurement
- [Medium] Static analysis with SonarQube
- [Medium] Quality gates in CI/CD
- [High] Code review fundamentals

---

Revision answers will be added here when an interview response is rated below 8/10.
