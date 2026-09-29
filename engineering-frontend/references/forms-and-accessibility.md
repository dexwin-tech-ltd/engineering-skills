# Forms and Accessibility

Read when changing forms, validation messages, focus, keyboard interaction, or accessible states.

## Forms and Validation

- Prefer TanStack Form as the default form-state and form-UI orchestration library for web and mobile projects unless the repo already standardizes on another tool.
- Validate all form inputs with Zod schemas. Reuse field schemas across blur validation and submit/step validation.
- Use `.safeParse()` for form validation. Do not pass raw Zod errors into JSX.
- Return and render user-facing strings. Never expose raw Zod issues, client errors, or `Result` objects to views.
- Preserve all messages for a field. Do not collapse validation output to `issues[0]` or `fields[name][0]`.
- Keep general/banner errors as a list of messages, not a nullable single string, when a flow can surface multiple independent problems.
- Flows translate domain errors into plain submit outcomes, field errors, and banner messages. Views apply field errors to their form state after callbacks resolve.

## Accessibility

- Accessibility is a default quality bar. Use semantic roles/labels, keyboard or equivalent navigation support where applicable, and screen-reader-friendly loading, error, and success states.
- Preserve accessibility coverage for states that affect navigation, form submission, authentication, checkout or payment, destructive actions, or error recovery.
- Add accessibility-aware assertions for critical user-visible flows when the local test stack supports them.
