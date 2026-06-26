# Frontend Architecture & File System Specification

## Purpose
Defines the folder structure and module organization shared by every frontend
project. A consistent file system lets any developer (or agent) navigate any
frontend project the same way and place new code in a predictable location.

## Requirements

### Requirement: Source root layout
Projects MUST keep application source under a top-level `src/` directory and MUST
organize it using the following baseline folders. Additional folders MAY be
added when justified, but these names are reserved for their stated purpose:

- `src/pages/` — route-level screens, one folder per page
- `src/components/` — reusable presentational/UI components
- `src/features/` — feature modules grouping components, hooks, and logic by
  business domain
- `src/hooks/` — shared custom hooks
- `src/services/` — API clients and external integrations
- `src/lib/` or `src/utils/` — framework-agnostic helpers
- `src/routes/` — route definitions and router configuration
- `src/theme/` — MUI theme configuration
- `src/assets/` — static assets imported by the app

#### Scenario: Placing a new screen
- GIVEN a developer adds a new route-level screen
- WHEN they create its files
- THEN the screen lives under `src/pages/<page-name>/`

#### Scenario: Placing a reusable button
- GIVEN a developer creates a button used across the app
- WHEN they create its files
- THEN the component lives under `src/components/`

### Requirement: Feature-based grouping
Business logic MUST be grouped by feature inside `src/features/<feature>/` rather
than scattered by technical type across the whole app. A feature folder SHOULD
co-locate its components, hooks, services, and validation schemas.

#### Scenario: Adding a feature
- GIVEN a new business capability (e.g. "invoices")
- WHEN it is implemented
- THEN its components, hooks, and logic live under `src/features/invoices/`
- AND cross-feature reusable pieces are promoted to the shared folders

### Requirement: Component file conventions
Each component MUST live in its own folder when it has more than one file
(component, styles, tests). Component files MUST use **PascalCase**
(`UserCard.jsx`); hooks MUST use the `use` prefix in **camelCase**
(`useAuth.js`); non-component modules MUST use **camelCase**.

#### Scenario: Component with a test
- GIVEN a component `UserCard` with a unit test
- WHEN its files are created
- THEN they live in `src/components/UserCard/` as `UserCard.jsx` and
  `UserCard.test.jsx`

### Requirement: Import boundaries
Shared folders (`components`, `hooks`, `lib`/`utils`) MUST NOT import from
`pages` or `features`, to keep shared code free of feature-specific coupling.
Pages MAY import from features, components, and shared folders.

#### Scenario: Invalid import
- GIVEN a shared component in `src/components/`
- WHEN it imports from `src/features/invoices/`
- THEN this violates the import boundary and MUST be refactored
