# Frontend UI Components & Theming Specification

## Purpose
Defines how user interface components are built and styled across frontend
projects using Material UI (MUI) as the component library and react-icons for
iconography, with a shared theme for consistent look and feel.

## Requirements

### Requirement: MUI as the component library
UI components MUST be built on Material UI (MUI). Introducing a second general
component library (e.g. Ant Design, Chakra) MUST NOT happen without a change
proposal in this store.

#### Scenario: Adding a styled button
- GIVEN a developer needs a button
- WHEN they implement it
- THEN they use the MUI button component (optionally wrapped) rather than a
  hand-rolled or third-party-library button

### Requirement: Centralized theme
Visual design tokens (palette, typography, spacing, breakpoints) MUST be defined
in a single MUI theme under `src/theme/` and provided at the application root.
Hard-coded colors and font sizes in individual components MUST be avoided in
favor of theme values.

#### Scenario: Applying brand colors
- GIVEN the app has a brand palette
- WHEN a component needs the primary color
- THEN it reads the color from the MUI theme rather than hard-coding a hex value

### Requirement: react-icons for iconography
Icons MUST be sourced from react-icons. Mixing multiple unrelated icon delivery
mechanisms (e.g. raw SVG imports plus an icon font plus react-icons) SHOULD be
avoided for consistency.

#### Scenario: Adding an icon
- GIVEN a developer needs an icon in a component
- WHEN they add it
- THEN the icon comes from react-icons

### Requirement: Styling approach
Component styling MUST use MUI's styling system (e.g. the `sx` prop or
`styled`). Global CSS files that override MUI internals SHOULD be avoided.

#### Scenario: Styling a component
- GIVEN a developer styles an MUI-based component
- WHEN they apply custom styles
- THEN they use the `sx` prop or `styled` rather than ad-hoc global CSS

### Requirement: Responsive and accessible components
Components MUST be responsive using the theme breakpoints and MUST preserve the
accessibility semantics provided by MUI (labels, roles, keyboard support).

#### Scenario: Small viewport
- GIVEN a layout built with MUI
- WHEN it is viewed on a small screen
- THEN it adapts using theme breakpoints without horizontal overflow
