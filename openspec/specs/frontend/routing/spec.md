# Frontend Routing Specification

## Purpose
Defines how client-side routing is implemented across frontend projects using
React Router, including route organization, protected routes, and navigation.

## Requirements

### Requirement: React Router for navigation
Projects MUST use React Router for all client-side navigation. Full-page reloads
for in-app navigation MUST NOT be used.

#### Scenario: Navigating between screens
- GIVEN a user is on an in-app screen
- WHEN they trigger navigation to another in-app screen
- THEN React Router updates the view without a full page reload

### Requirement: Centralized route definitions
Route definitions MUST be declared centrally (e.g. under `src/routes/`) rather
than scattered inline across components, so the full route map is discoverable in
one place.

#### Scenario: Reviewing the route map
- GIVEN a developer needs to see all available routes
- WHEN they open the routing configuration
- THEN every top-level route is listed in one place

### Requirement: Protected routes
Routes that require authentication MUST be wrapped by a route guard that checks
the session. Unauthenticated access to a protected route MUST redirect the user
to the login route.

#### Scenario: Unauthenticated access
- GIVEN a user without a valid session
- WHEN they navigate to a protected route
- THEN they are redirected to the login route
- AND the originally requested location MAY be preserved for post-login redirect

#### Scenario: Authenticated access
- GIVEN a user with a valid session
- WHEN they navigate to a protected route
- THEN the protected screen is rendered

### Requirement: Lazy loading of routes
A loading fallback MUST be shown while a screen loads, and route-level screens
SHOULD be code-split with lazy loading to keep the initial bundle small.

#### Scenario: Loading a heavy screen
- GIVEN a route-level screen is code-split
- WHEN the user navigates to it for the first time
- THEN a loading fallback is shown until the screen chunk has loaded
