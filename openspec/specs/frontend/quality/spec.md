# Frontend Code Quality Specification

## Purpose
Defines the code quality standards shared by every frontend project: linting,
formatting, naming, and accessibility. These rules keep code readable and
consistent across all frontend repositories.

## Requirements

### Requirement: Linting is enforced
Projects MUST use ESLint with a shared configuration and MUST fail the build/CI
on lint errors. Disabling rules inline MUST be justified with a comment.

#### Scenario: Lint error in CI
- GIVEN code that violates an ESLint rule
- WHEN CI runs the lint job
- THEN the job fails until the violation is fixed or explicitly justified

### Requirement: Consistent formatting
Projects MUST use an automated formatter (e.g. Prettier) with a shared
configuration so formatting is not a matter of personal preference or review
debate.

#### Scenario: Formatting on commit
- GIVEN a developer changes a file
- WHEN they format it
- THEN the result matches the shared formatter configuration

### Requirement: Naming conventions
Code MUST follow consistent naming: components in **PascalCase**, hooks with a
`use` prefix, variables and functions in **camelCase**, constants in
**UPPER_SNAKE_CASE**, and folders/spec domains in **kebab-case**.

#### Scenario: Naming a hook
- GIVEN a new custom hook
- WHEN it is named
- THEN it starts with `use` and is camelCase (e.g. `useUserSession`)

### Requirement: No unused or dead code
Pull requests MUST NOT introduce unused variables, imports, or unreachable code.
Linting SHOULD flag these automatically.

#### Scenario: Unused import
- GIVEN a file with an unused import
- WHEN linting runs
- THEN the unused import is flagged and removed before merge

### Requirement: Accessibility baseline
Interactive UI MUST meet a baseline of accessibility: images have alt text, form
inputs have associated labels, and interactive elements are keyboard reachable.

#### Scenario: Adding an input
- GIVEN a new form input
- WHEN it is created
- THEN it has an associated, programmatically linked label

### Requirement: Small, focused modules
A component or module that grows too large or mixes unrelated concerns MUST be
split, and business logic SHOULD live in hooks/services rather than inside
presentational components.

#### Scenario: Oversized component
- GIVEN a component handling rendering, data fetching, and complex logic
- WHEN it becomes hard to follow
- THEN its logic is extracted into hooks/services and the component is simplified
