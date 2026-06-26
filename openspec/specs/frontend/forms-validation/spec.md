# Frontend Forms & Validation Specification

## Purpose
Defines how forms are built and validated across frontend projects using Formik
for form state and Yup for schema validation, ensuring consistent validation,
error handling, and submission behavior.

## Requirements

### Requirement: Formik for form state
Forms with user input MUST manage their state with Formik. Ad-hoc form state
built from scattered `useState` calls MUST NOT be used for non-trivial forms.

#### Scenario: Building a form
- GIVEN a developer builds a form with multiple fields
- WHEN the form is implemented
- THEN Formik manages values, touched state, errors, and submission

### Requirement: Yup for validation schemas
Form validation MUST be defined with Yup schemas and wired into Formik. Inline,
imperative validation logic MUST NOT replace a Yup schema for field validation.

#### Scenario: Validating input
- GIVEN a form with validation rules
- WHEN the rules are implemented
- THEN they are expressed as a Yup schema passed to Formik

### Requirement: Visible, accessible validation feedback
Validation errors MUST be displayed next to the affected field once the field
has been touched or the form has been submitted, and MUST be associated with the
input for assistive technologies.

#### Scenario: Invalid field
- GIVEN a required field is left empty
- WHEN the user blurs the field or submits the form
- THEN an error message is shown for that field
- AND the message is programmatically associated with the input

### Requirement: Submission state handling
On submit, forms MUST reflect their submission state: the submit control is
disabled while submitting, and a server-side error is surfaced to the user
without losing entered values.

#### Scenario: Submitting
- GIVEN a valid form
- WHEN the user submits it
- THEN the submit button is disabled while the request is in flight

#### Scenario: Server error on submit
- GIVEN a form submission that fails on the server
- WHEN the error response is received
- THEN the user sees an error message
- AND the previously entered values are preserved

### Requirement: Reusable validation schemas
Shared validation rules (e.g. email, password) MUST NOT be duplicated per form;
they SHOULD be defined once and reused across the forms that need them.

#### Scenario: Reusing an email rule
- GIVEN two forms that both validate an email field
- WHEN the second form is built
- THEN it reuses the shared email validation rule
