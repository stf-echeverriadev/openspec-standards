# Frontend Stack Specification

## Purpose
Defines the baseline technology stack shared by every frontend project. All
frontend applications are single-page React apps built with Vite and use the
same core libraries so that knowledge, tooling, and components transfer between
projects.

## Requirements

### Requirement: React as the UI library
Frontend applications MUST be built with React using function components and
hooks. Class components MUST NOT be introduced for new code.

#### Scenario: New component
- GIVEN a developer creates a new UI component
- WHEN the component is written
- THEN it is a function component
- AND it uses React hooks for state and side effects
- AND it does not use a class component

### Requirement: Vite as the build tool
Projects MUST use Vite for development server and production builds. Create
React App (CRA) and Webpack-based custom setups MUST NOT be used.

#### Scenario: Local development
- GIVEN a developer clones a frontend project
- WHEN they run the documented dev command (e.g. `npm run dev`)
- THEN Vite serves the app with hot module replacement

#### Scenario: Production build
- GIVEN a project is ready to deploy
- WHEN the build command (e.g. `npm run build`) runs
- THEN Vite produces an optimized static bundle in the build output directory

### Requirement: Standard core libraries
Frontend projects MUST use the agreed shared libraries for their respective
concerns and MUST NOT substitute equivalent alternatives without a change
proposal in this store:

- Routing: **React Router**
- UI components and theming: **Material UI (MUI)**
- Icons: **react-icons**
- Forms: **Formik**
- Schema validation: **Yup**
- Testing: **Vitest**

#### Scenario: Choosing a routing library
- GIVEN a project needs client-side routing
- WHEN the developer adds routing
- THEN React Router is used
- AND no alternative router (e.g. TanStack Router) is introduced without a
  change proposal

#### Scenario: Choosing a form library
- GIVEN a project needs form state management
- WHEN the developer builds a form
- THEN Formik is used with Yup for validation
- AND no alternative form library is introduced without a change proposal

### Requirement: Package management and Node version
Projects MUST pin a Node.js version supported by the current Vite major release
and MUST commit a lockfile so installs are reproducible.

#### Scenario: Reproducible install
- GIVEN a fresh checkout of a frontend project
- WHEN dependencies are installed
- THEN a committed lockfile determines the exact versions installed
