# Frontend State & Data Specification

## Purpose
Defines how application state is managed and how data is fetched from backend
APIs across frontend projects, including separation of server state from UI
state and consistent request handling.

## Requirements

### Requirement: Separate server state from UI state
Projects MUST distinguish **server state** (data fetched from APIs) from **UI
state** (local, ephemeral interface state). Server responses MUST NOT be copied
into ad-hoc global stores as the long-term source of truth.

#### Scenario: Displaying fetched data
- GIVEN a screen that lists data from an API
- WHEN the data is loaded
- THEN it is treated as server state (fetched, cached, and revalidated)
- AND not duplicated into unrelated global UI state

### Requirement: Centralized API client
HTTP access MUST go through a centralized API client/service layer under
`src/services/` rather than scattering raw fetch/axios calls throughout
components. The base URL MUST come from environment configuration.

#### Scenario: Calling an endpoint
- GIVEN a component needs data from the backend
- WHEN it requests the data
- THEN it calls a function in the service layer
- AND the service layer uses the configured base URL

### Requirement: Loading and error states
Every data-fetching view MUST handle loading and error states explicitly,
showing a loading indicator while pending and an error state on failure.

#### Scenario: Failed request
- GIVEN a screen fetching data
- WHEN the request fails
- THEN the screen shows an error state instead of a blank or broken view

#### Scenario: Pending request
- GIVEN a screen fetching data
- WHEN the request is in flight
- THEN a loading indicator is shown

### Requirement: Authenticated requests
Requests to protected endpoints MUST attach the authentication token (see the
frontend auth spec) through the centralized client, not per-call by hand.

#### Scenario: Protected endpoint
- GIVEN an authenticated user
- WHEN a request is made to a protected endpoint
- THEN the API client attaches the JWT automatically

### Requirement: Environment configuration
Environment-specific values (API base URL, feature flags) MUST be read from
Vite environment variables and MUST NOT be hard-coded in source.

#### Scenario: Switching environments
- GIVEN the app targets staging instead of production
- WHEN it is built for staging
- THEN the API base URL comes from environment variables, not edited source code
