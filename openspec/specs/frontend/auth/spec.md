# Frontend Authentication (JWT) Specification

## Purpose
Defines how frontend projects authenticate users and manage JWT-based sessions,
including token storage, attaching tokens to requests, session expiration, and
logout. This spec covers the client side only; token issuance and verification
are owned by the backend.

## Requirements

### Requirement: JWT-based sessions
Frontend projects MUST authenticate users with JWT issued by the backend. The
frontend MUST treat the token as an opaque credential and MUST NOT trust
client-side data for authorization decisions beyond UX hints.

#### Scenario: Successful login
- GIVEN a user submits valid credentials
- WHEN the backend returns a JWT
- THEN the frontend stores the token and marks the user as authenticated

### Requirement: Attaching the token to requests
The JWT MUST be attached to requests to protected endpoints through the
centralized API client, typically as an `Authorization: ****** header.
Tokens MUST NOT be passed in URLs or query strings.

#### Scenario: Calling a protected endpoint
- GIVEN an authenticated user
- WHEN the app calls a protected endpoint
- THEN the API client adds the `Authorization: ****** header
- AND the token is never placed in the URL

### Requirement: Token storage and exposure
The token MUST be stored in a single, well-defined place accessed only through a
dedicated auth module. Tokens MUST NOT be logged, embedded in markup, or exposed
through error messages.

#### Scenario: Logging
- GIVEN a request fails
- WHEN the error is logged
- THEN the log output does not contain the JWT

### Requirement: Handling expiration and 401 responses
The frontend MUST clear the session and redirect the user to the login route
when a request returns 401 Unauthorized or the token is known to be expired.

#### Scenario: Expired token
- GIVEN an authenticated user whose token has expired
- WHEN a request returns 401 Unauthorized
- THEN the session is cleared
- AND the user is redirected to the login route

### Requirement: Logout
Logging out MUST clear the stored token and any authenticated UI state so that
protected routes are no longer accessible without re-authenticating.

#### Scenario: User logs out
- GIVEN an authenticated user
- WHEN they log out
- THEN the token is removed
- AND protected routes redirect to login on the next navigation

### Requirement: Centralized auth state
Authentication state MUST be exposed through a single source (e.g. an auth
context/hook) that components and route guards consume. Components MUST NOT read
the raw token directly to decide what to render.

#### Scenario: Conditional UI
- GIVEN a component shows different content for authenticated users
- WHEN it determines auth status
- THEN it reads from the shared auth state, not the raw token store
