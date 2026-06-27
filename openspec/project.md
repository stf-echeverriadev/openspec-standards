# Project Conventions — openspec-standards

## What this repo is

`openspec-standards` is a centralized **OpenSpec store** that holds the shared
specs and standards for **all** of my projects. It is the single source of truth
for **architecture, file-system layout, and code conventions**.

- **Frontend projects** share one common stack and therefore one set of frontend
  specs (`openspec/specs/frontend/`).
- **Backend projects** share another common stack and therefore one set of
  backend specs (`openspec/specs/backend/`). The shared backend stack is Java 21,
  Spring Boot, Maven, and SQL Server.
- **Cross-cutting standards** that apply to both live in
  `openspec/specs/shared/`.

This repo is consumed as a **reference**: other repos cite it read-only. It does
not contain any application code.

## How specs are written here

Every `spec.md` follows the OpenSpec format:

- `## Purpose` — one paragraph describing the domain.
- `## Requirements` — a list of `### Requirement:` entries.
- Each requirement states a behavior using RFC 2119 keywords
  (**MUST / SHALL / SHOULD / MAY**).
- Each requirement has at least one `#### Scenario:` with `GIVEN / WHEN / THEN`
  steps, so it is concrete and verifiable.

Specs describe **what** must be true (a contract), not step-by-step
implementation. They are intentionally tech-specific here because every project
of a given layer shares the same stack.

## Global conventions

### Language

- Specs and documentation in this repo are written in **English**.
- Code identifiers (variables, functions, files) are written in English.

### Naming

- Directories and spec domains use **kebab-case** (e.g. `state-data`,
  `ui-components`).
- One domain per folder, one `spec.md` per domain.

### Source of truth, not a copy

- When a project needs to know "how do we structure a feature?" or "which form
  library do we use?", the answer lives here, not duplicated in each repo.
- Changes to standards go through `openspec/changes/` and are archived into
  `openspec/specs/` once agreed.

## How a project consumes these standards

A project repo declares this store as a reference in its own
`openspec/config.yaml`:

```yaml
references:
  - { id: openspec-standards, remote: "git@github.com:stf-echeverriadev/openspec-standards.git" }
```

Frontend projects rely on `specs/frontend/` + `specs/shared/`; backend projects
rely on `specs/backend/` + `specs/shared/`.
