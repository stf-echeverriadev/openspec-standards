# Git Workflow Specification

## Purpose
Defines the version-control conventions shared by all projects: branching,
commit messages, and pull requests. These rules keep history readable and
reviews consistent across every repository.

## Requirements

### Requirement: Branch naming
Work MUST be done on branches named with a `type/short-description` pattern
(e.g. `feat/login-form`, `fix/token-refresh`). Committing feature work directly
to the default branch MUST NOT happen.

#### Scenario: Starting new work
- GIVEN a developer begins a new feature
- WHEN they create a branch
- THEN the branch is named like `feat/<short-description>`

### Requirement: Conventional commit messages
Commit messages MUST follow Conventional Commits: a `type(scope): subject`
summary line (e.g. `feat(auth): add login form`). Allowed types include `feat`,
`fix`, `docs`, `refactor`, `test`, `chore`, and `ci`.

#### Scenario: Committing a fix
- GIVEN a developer fixes a bug
- WHEN they commit
- THEN the message follows `fix(scope): subject`

### Requirement: Pull requests for all changes
Changes MUST be merged via pull request, never pushed directly to the default
branch. Each pull request MUST describe what changed and why.

#### Scenario: Merging work
- GIVEN a completed branch
- WHEN it is ready to merge
- THEN it is merged through a reviewed pull request

### Requirement: Green CI before merge
A pull request MUST pass required CI checks (lint, tests, build) before it can be
merged.

#### Scenario: Failing checks
- GIVEN a pull request with a failing CI check
- WHEN a merge is attempted
- THEN the merge is blocked until checks pass

### Requirement: No secrets in history
Secrets (tokens, keys, credentials) MUST NOT be committed. If a secret is
committed, it MUST be revoked and rotated, not merely removed in a later commit.

#### Scenario: Accidental secret
- GIVEN a secret is accidentally committed
- WHEN it is discovered
- THEN the secret is revoked and rotated
- AND the offending value is removed from the codebase
