# Backend Authentication (JWT) Specification

## Purpose
Defines how backend services authenticate and authorize requests using Spring
Security and JSON Web Tokens. The backend is the owner of authentication: it
issues JWTs on login and verifies them on every protected request, complementing
the frontend auth spec, which only consumes the tokens.

## Requirements

### Requirement: Spring Security as the security layer
Backend projects MUST enforce authentication and authorization through Spring
Security. Custom, hand-rolled security filters MUST NOT replace Spring Security
without a change proposal in this store.

#### Scenario: Securing endpoints
- GIVEN a service exposes protected endpoints
- WHEN security is configured
- THEN access control is enforced through Spring Security
- AND endpoints are denied by default unless explicitly permitted

### Requirement: Backend issues JWTs on login
The backend MUST issue a signed JWT when a user authenticates with valid
credentials. The token MUST be signed with a secret/key held only by the
backend, and MUST carry an expiration claim.

#### Scenario: Successful login
- GIVEN a user submits valid credentials to the login endpoint
- WHEN authentication succeeds
- THEN the backend returns a signed JWT with an expiration claim
- AND the token is signed with a key that is not exposed to clients

### Requirement: Verifying JWTs on protected requests
The backend MUST verify the JWT signature and expiration on every request to a
protected endpoint, and MUST reject invalid or expired tokens with 401
Unauthorized. The token is expected in the `Authorization` header using the
`Bearer` scheme.

#### Scenario: Expired or invalid token
- GIVEN a request to a protected endpoint
- WHEN the JWT is missing, malformed, or expired
- THEN the backend responds with 401 Unauthorized
- AND the protected operation does not run

#### Scenario: Valid token
- GIVEN a request with a valid `Bearer` JWT
- WHEN the backend verifies it
- THEN the request proceeds with the authenticated principal established

### Requirement: Authorization based on token claims
Authorization decisions MUST be derived from verified token claims (e.g. roles or
authorities) on the server, not from client-supplied flags outside the token.

#### Scenario: Insufficient permissions
- GIVEN an authenticated user without the required role
- WHEN they call an endpoint that requires that role
- THEN the backend responds with 403 Forbidden

### Requirement: Secrets and tokens kept out of logs and source
Signing keys and credentials MUST come from externalized configuration and MUST
NOT be committed to source. JWTs and secrets MUST NOT be written to logs or
included in error responses.

#### Scenario: Logging a failed request
- GIVEN a request fails
- WHEN the error is logged
- THEN the log output does not contain the JWT or the signing key

### Requirement: Password storage
When the backend stores user credentials, passwords MUST be stored using a
strong adaptive hashing algorithm (e.g. BCrypt) and MUST NOT be stored in
plaintext or with a fast/unsalted hash.

#### Scenario: Creating a user
- GIVEN a new user account is created with a password
- WHEN the password is persisted
- THEN only a salted, adaptively hashed value is stored
- AND the plaintext password is never persisted
