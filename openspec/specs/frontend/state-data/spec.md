# Frontend State & Data Specification

## Purpose
Defines how application state is managed and how data is fetched from backend
APIs across frontend projects, including the approved state management approaches
(Context API and Redux), separation of server state from UI state, and consistent
request handling.

## Requirements

### Requirement: Approved UI state management approaches
For global UI state, projects MUST use either the React Context API or Redux
(with Redux Toolkit). Other global state libraries (e.g. MobX, Zustand, Recoil)
MUST NOT be introduced without a change proposal in this store.

#### Scenario: Choosing a state tool
- GIVEN a project needs to share UI state across components
- WHEN the developer adds global state
- THEN they use either the Context API or Redux Toolkit
- AND no alternative global state library is introduced without a change
  proposal

### Requirement: Prefer Redux for complex state
Projects MUST use Redux (Redux Toolkit) when the global UI state is complex —
large objects, many interrelated fields, or frequent updates from multiple parts
of the app — and SHOULD reserve the Context API for simple, low-frequency state
(e.g. theme, current user, feature toggles).

#### Scenario: Large, complex shared state
- GIVEN global state is a large object with many interrelated fields updated
  from several features
- WHEN the developer selects a state approach
- THEN they use Redux Toolkit
- AND they do not manage that complex state through ad-hoc nested contexts

#### Scenario: Simple shared state
- GIVEN a small, infrequently changing value such as the UI theme
- WHEN the developer shares it across the app
- THEN the Context API is an acceptable choice
- AND Redux is not required for that value

### Requirement: Redux state stays serializable and out of server state
When Redux is used, its store MUST hold serializable UI state and MUST NOT be
used as the long-term source of truth for server data (see the server vs UI
state requirement). Side effects and API calls MUST go through the service layer,
not raw fetches inside reducers.

#### Scenario: Storing fetched data in Redux
- GIVEN data fetched from an API
- WHEN a developer considers placing it in the Redux store
- THEN the store is not treated as the canonical cache for that server data
- AND non-serializable values are not placed in the store

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
