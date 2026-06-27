# Backend Standards

The backend standards describe the **shared backend stack** that all backend
projects use. They follow the same OpenSpec structure as the frontend standards:
one `spec.md` per domain, using `## Purpose` and `## Requirements` with
`### Requirement:` entries and `#### Scenario:` GIVEN/WHEN/THEN examples (see
`openspec/project.md`).

All backend projects share a single common stack so it is described once here,
the same way `specs/frontend/` does for frontend.

## The current backend stack

| Concern           | Technology              |
| ----------------- | ----------------------- |
| Language / runtime| Java 21 (LTS)           |
| Framework         | Spring Boot             |
| Build tool        | Maven                   |
| Database          | Microsoft SQL Server    |
| Data access       | Spring Data JPA         |
| Migrations        | Flyway                  |
| Auth              | Spring Security + JWT   |
| API style         | REST over HTTP (JSON)   |
| Testing           | JUnit 5 (Spring Boot test starter) |

## Spec domains

Defined so far (a subset of the planned domains):

- `stack/` — Java 21, Spring Boot, Maven
- `api/` — REST conventions, status codes, validation, error format
- `data/` — SQL Server, Spring Data JPA, Flyway migrations
- `auth/` — Spring Security with JWT issuance and verification
- `testing/` — JUnit 5 with the simplest practical setup

Planned but not defined yet:

- `observability/` — logging, metrics, error handling

Cross-cutting concerns (git, security, CI/CD) live in
`openspec/specs/shared/` and apply to backend projects too.
