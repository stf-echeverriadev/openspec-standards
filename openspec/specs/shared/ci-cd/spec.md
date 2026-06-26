# CI/CD Specification

## Purpose
Defines the continuous integration and delivery standards shared by all
projects: what runs automatically on changes, what gates merges and releases,
and how builds are produced.

## Requirements

### Requirement: CI runs on every pull request
Every pull request MUST trigger an automated CI pipeline that installs
dependencies, lints, runs tests, and builds the project.

#### Scenario: Opening a pull request
- GIVEN a developer opens a pull request
- WHEN CI is triggered
- THEN it installs dependencies, lints, tests, and builds the project

### Requirement: Required checks gate merges
The lint, test, and build jobs MUST be required checks; a pull request MUST NOT
be mergeable while any required check is failing.

#### Scenario: Failing build
- GIVEN a pull request whose build fails
- WHEN a merge is attempted
- THEN the merge is blocked until the build passes

### Requirement: Reproducible installs in CI
CI MUST use the committed lockfile for deterministic, reproducible dependency
installation (e.g. `npm ci`) rather than unpinned installs.

#### Scenario: Installing dependencies in CI
- GIVEN the CI pipeline installs dependencies
- WHEN it runs
- THEN it uses the lockfile-based install command for reproducible versions

### Requirement: Environment configuration via CI secrets
Build- and deploy-time secrets MUST be provided through the CI platform's secret
store, never committed to the repository or printed in logs.

#### Scenario: Deploy needs a token
- GIVEN a deployment step requires a credential
- WHEN the pipeline runs
- THEN the credential comes from the CI secret store and is not printed in logs

### Requirement: Build artifacts match the deployed app
Production builds MUST be produced by the CI pipeline using the project's
standard build command, so the deployed artifact matches what CI verified.

#### Scenario: Releasing
- GIVEN a change is approved for release
- WHEN the release pipeline runs
- THEN it produces the artifact with the standard build command that CI validated
