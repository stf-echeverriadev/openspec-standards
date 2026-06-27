# Backend Testing Specification

## Purpose
Defines how backend services are tested. The goal is the simplest setup that
gives confidence: the default Spring Boot test tooling (JUnit 5), with no extra
testing infrastructure unless a real need justifies it.

## Requirements

### Requirement: JUnit 5 with the Spring Boot test starter
Projects MUST write tests with JUnit 5 using the `spring-boot-starter-test`
dependency that ships with Spring Boot. Additional test frameworks or runners
MUST NOT be introduced without a change proposal in this store.

#### Scenario: Adding a test
- GIVEN a developer writes a test for a backend service
- WHEN the test is created
- THEN it is a JUnit 5 test using the Spring Boot test starter
- AND no alternative test framework is added

### Requirement: Keep the test setup simple
The testing setup MUST stay as simple as possible: prefer plain unit tests for
business logic and use the Spring context only where the behavior under test
genuinely requires it. Heavier infrastructure (e.g. containerized databases)
MUST NOT be added unless a real need justifies it.

#### Scenario: Testing business logic
- GIVEN a service method with business rules
- WHEN it is tested
- THEN the test exercises it as a plain unit test without loading the full
  Spring context where the context is not needed

#### Scenario: Avoiding unnecessary infrastructure
- GIVEN a new test is needed
- WHEN the developer chooses an approach
- THEN they use the simplest approach that covers the behavior
- AND they do not introduce extra test infrastructure without a clear need

### Requirement: Critical paths are covered
Each endpoint MUST have automated tests covering authentication/authorization
behavior, request validation, and its main success and failure paths.

#### Scenario: Endpoint coverage
- GIVEN an endpoint that requires authentication and validates its input
- WHEN the test suite runs
- THEN there are tests for the success path, an unauthorized request, and an
  invalid payload

### Requirement: Tests run in CI and must pass
The test suite MUST run in continuous integration on every pull request via the
build (e.g. `./mvnw verify`), and a failing suite MUST block merging.

#### Scenario: Failing test in a pull request
- GIVEN a pull request that breaks a test
- WHEN CI runs the build
- THEN the test phase fails
- AND the pull request is not mergeable until fixed
