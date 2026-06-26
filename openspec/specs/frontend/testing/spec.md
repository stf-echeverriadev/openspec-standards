# Frontend Testing Specification

## Purpose
Defines how frontend projects are tested using Vitest, including what must be
tested, how tests are organized, and the expectations for running tests in
development and CI.

## Requirements

### Requirement: Vitest as the test runner
Projects MUST use Vitest as the unit and component test runner. Jest and other
runners MUST NOT be introduced without a change proposal in this store.

#### Scenario: Running tests
- GIVEN a frontend project
- WHEN the test command (e.g. `npm run test`) runs
- THEN Vitest executes the test suite

### Requirement: Component testing approach
Component tests MUST exercise components through user-facing behavior (rendered
output and interactions) rather than internal implementation details, using a
React testing library compatible with Vitest.

#### Scenario: Testing a form component
- GIVEN a form component
- WHEN it is tested
- THEN the test interacts with rendered fields and asserts visible results
- AND it does not assert on internal component state directly

### Requirement: Test file location and naming
Test files MUST be co-located with the code they test and named with a
`.test.` (or `.spec.`) suffix matching the source file.

#### Scenario: Locating a component test
- GIVEN a component `UserCard.jsx`
- WHEN its test is created
- THEN it is `UserCard.test.jsx` in the same folder

### Requirement: Critical paths are covered
Authentication flows, form validation, and protected-route behavior MUST have
automated tests. Pure presentational components MAY rely on lighter coverage.

#### Scenario: Login flow coverage
- GIVEN the login feature
- WHEN the test suite runs
- THEN there are tests covering successful login and invalid credentials

### Requirement: Tests run in CI and must pass
The test suite MUST run in continuous integration on every pull request, and a
failing suite MUST block merging.

#### Scenario: Failing test in a pull request
- GIVEN a pull request that breaks a test
- WHEN CI runs
- THEN the test job fails
- AND the pull request is not mergeable until fixed
