# openspec-standards

The single **source of truth** for architecture, file-system layout, and code
conventions across all of my projects. It is an [OpenSpec](https://github.com/Fission-AI/OpenSpec)
**store**: a standalone repository whose only job is to hold shared specs, with
no application code.

- **Frontend** projects share one stack, so they share `openspec/specs/frontend/`.
- **Backend** projects share another stack, so they will share
  `openspec/specs/backend/` _(pending — see that folder's README)_.
- **Cross-cutting** standards live in `openspec/specs/shared/`.

Other repos consume this store **by reference** (read-only). They cite it; they
do not copy it.

## Repository layout

```
openspec-standards/
├── .openspec-store/
│   └── store.yaml              # store identity (id: openspec-standards)
└── openspec/
    ├── project.md              # global conventions for this store
    ├── specs/
    │   ├── frontend/           # shared standards for all frontend projects
    │   │   ├── stack/          # React, Vite, React Router, MUI, Formik, Yup, Vitest
    │   │   ├── architecture/   # src/ layout & file-system conventions
    │   │   ├── routing/        # React Router conventions
    │   │   ├── forms-validation/ # Formik + Yup
    │   │   ├── ui-components/  # MUI + react-icons + theming
    │   │   ├── state-data/     # state & API data access
    │   │   ├── auth/           # JWT handling on the client
    │   │   ├── testing/        # Vitest
    │   │   └── quality/        # lint, format, naming, accessibility
    │   ├── backend/            # pending — reserved for the shared backend stack
    │   └── shared/             # cross-cutting: git, security, ci-cd
    └── changes/
        └── archive/            # completed change proposals
```

## The current frontend stack

These standards assume every frontend project uses the same stack:

| Concern            | Technology        |
| ------------------ | ----------------- |
| UI library         | React             |
| Build tool         | Vite              |
| Routing            | React Router      |
| Forms              | Formik            |
| Schema validation  | Yup               |
| Component library  | Material UI (MUI) |
| Icons              | react-icons       |
| Testing            | Vitest            |
| Auth               | JWT               |

## How specs are written

Each `spec.md` uses the OpenSpec format:

- `## Purpose` — what the domain covers.
- `## Requirements` — `### Requirement:` entries using RFC 2119 keywords
  (**MUST / SHALL / SHOULD / MAY**).
- Each requirement has `#### Scenario:` blocks with `GIVEN / WHEN / THEN` steps,
  so every rule is concrete and verifiable.

See `openspec/project.md` for the full conventions.

## Using these standards in a project (reference model)

This repo is consumed as a **reference**, so projects keep their own specs and
cite these standards read-only.

1. Install the OpenSpec CLI (optional, for tooling): `npm install -g @fission-ai/openspec@latest`.
2. Clone this store and register it locally:

   ```bash
   git clone git@github.com:stf-echeverriadev/openspec-standards.git
   openspec context-store register ./openspec-standards
   ```

3. In each project's `openspec/config.yaml`, declare the reference:

   ```yaml
   references:
     - { id: openspec-standards, remote: "git@github.com:stf-echeverriadev/openspec-standards.git" }
   ```

Frontend projects rely on `specs/frontend/` + `specs/shared/`; backend projects
will rely on `specs/backend/` + `specs/shared/`.

> Even without the CLI, every `spec.md` is plain Markdown and can be read
> directly by people and AI assistants.

## Changing a standard

Standards evolve through `openspec/changes/`: propose a change, agree on it, then
archive it so the deltas fold into `openspec/specs/`. Archived changes move to
`openspec/changes/archive/`.
