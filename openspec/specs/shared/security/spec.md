# Security Specification

## Purpose
Defines the baseline security standards shared by all projects: secret handling,
input validation, dependency hygiene, and safe handling of authentication
tokens. These apply to both frontend and backend projects.

## Requirements

### Requirement: No hard-coded secrets
Secrets MUST NOT be hard-coded in source or committed to the repository. They
MUST be provided through environment variables or a secrets manager.

#### Scenario: Configuring an API key
- GIVEN a project needs an API key
- WHEN it is configured
- THEN the key is read from environment configuration, not committed to source

### Requirement: Validate untrusted input
Input crossing a trust boundary (user input, API responses, URL params) MUST be
validated before use. Frontend forms MUST validate with the agreed schema
library (Yup); backends MUST validate request payloads independently.

#### Scenario: Submitting a form
- GIVEN a user submits data through a form
- WHEN the data is processed
- THEN it is validated before being used or sent onward

### Requirement: Safe token handling
Authentication tokens MUST NOT be logged, exposed in URLs, or rendered into
markup. Tokens MUST be transmitted only over HTTPS.

#### Scenario: Logging a request
- GIVEN a request carries an auth token
- WHEN it is logged for debugging
- THEN the token is omitted or redacted from the log output

### Requirement: Dependency vulnerability checks
Projects MUST run dependency vulnerability checks (e.g. `npm audit` or an
equivalent) in CI and MUST address high/critical advisories before release.

#### Scenario: New high-severity advisory
- GIVEN a dependency has a high-severity advisory
- WHEN CI runs the audit
- THEN the issue is surfaced and resolved before release

### Requirement: Least-privilege configuration
Access and permissions (API scopes, CORS origins, tokens) MUST be scoped to the
minimum needed. Wildcard or overly broad permissions MUST be avoided.

#### Scenario: Configuring CORS
- GIVEN a backend exposes an API to a known frontend origin
- WHEN CORS is configured
- THEN it allows the specific known origin rather than `*`
