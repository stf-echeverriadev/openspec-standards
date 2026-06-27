# Backend API Specification

## Purpose
Defines the API style shared by every backend service. All services expose
RESTful HTTP APIs with consistent resource naming, status codes, validation, and
a single error response format, so clients (including the shared frontend) can
integrate with any service the same way.

## Requirements

### Requirement: REST over HTTP as the API style
Backend services MUST expose their public API as REST over HTTP using JSON
payloads. GraphQL and RPC styles MUST NOT be introduced without a change
proposal in this store.

#### Scenario: Exposing a capability
- GIVEN a service needs to expose a capability to clients
- WHEN the API is designed
- THEN it is modeled as REST resources over HTTP with JSON
- AND no GraphQL or RPC endpoint is added without a change proposal

### Requirement: Resource-oriented, consistent routes
Endpoints MUST be organized around resources using plural nouns and MUST use HTTP
methods according to their semantics (GET to read, POST to create, PUT/PATCH to
update, DELETE to remove). Routes MUST NOT encode actions as verbs in the path.

#### Scenario: Designing a route
- GIVEN a resource named "invoices"
- WHEN routes are defined
- THEN the collection is `/invoices` and an item is `/invoices/{id}`
- AND actions are expressed through HTTP methods, not verbs like
  `/getInvoices`

### Requirement: Meaningful HTTP status codes
Responses MUST use HTTP status codes that match the outcome: 2xx for success,
4xx for client errors (e.g. 400 validation, 401 unauthenticated, 403 forbidden,
404 not found), and 5xx for server errors. A failed request MUST NOT return 200.

#### Scenario: Requesting a missing resource
- GIVEN a client requests a resource that does not exist
- WHEN the service responds
- THEN it returns 404 Not Found
- AND it does not return 200 with an empty or error body

### Requirement: Request validation
Incoming request payloads MUST be validated at the API boundary, and invalid
requests MUST be rejected with 400 Bad Request before any business logic runs.

#### Scenario: Invalid payload
- GIVEN a client sends a request that violates the field constraints
- WHEN the service processes it
- THEN it responds with 400 Bad Request
- AND the business operation does not run

### Requirement: Consistent error response format
Error responses MUST use a single, consistent JSON structure across all
endpoints so clients can handle errors uniformly. Internal details such as stack
traces MUST NOT be exposed in error responses.

#### Scenario: Returning an error
- GIVEN any endpoint produces an error
- WHEN the error response is sent
- THEN it follows the shared error structure
- AND it does not leak stack traces or internal implementation details

### Requirement: Layered controllers
Controllers MUST stay thin and delegate business logic to a service layer rather
than embedding business rules or data access directly in the controller.

#### Scenario: Handling a request
- GIVEN a controller receives a request
- WHEN it handles the request
- THEN it delegates the work to a service
- AND it does not perform data access or business rules inline
