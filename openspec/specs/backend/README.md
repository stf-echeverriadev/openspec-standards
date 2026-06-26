# Backend Standards — Pending

The backend standards are **not defined yet**. This folder is intentionally
reserved so that, once the shared backend stack is decided, its specs can be
added here following the same structure as the frontend standards.

All backend projects are expected to share a single common stack (language,
framework, database, auth, testing), so these specs will describe that shared
stack once, the same way `specs/frontend/` does for frontend.

## Planned spec domains

When the backend stack is defined, create the following (mirroring the frontend
layout):

- `stack/` — language, framework, runtime
- `api/` — API style (REST/GraphQL), versioning, error format
- `data/` — database, migrations, ORM/data access
- `auth/` — authentication and authorization (JWT issuance/verification)
- `testing/` — backend testing standard
- `observability/` — logging, metrics, error handling

Cross-cutting concerns (git, security, CI/CD) already live in
`openspec/specs/shared/` and apply to backend projects too.

## How to fill this in

For each domain above, create `<domain>/spec.md` using the OpenSpec format:
`## Purpose`, then `## Requirements` with `### Requirement:` entries and
`#### Scenario:` GIVEN/WHEN/THEN examples (see `openspec/project.md`).
