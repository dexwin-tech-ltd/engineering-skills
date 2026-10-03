# Forms and Accessibility

Read when changing forms, validation messages, focus, keyboard interaction, or accessible states.

## Forms and Validation

- Prefer TanStack Form as the default form-state and form-UI orchestration library for web and mobile projects unless the repo already standardizes on another tool.
- Use Effect Schema in new Effect projects and preserve Zod in established projects. Reuse field contracts across blur and submit/step validation; keep live-state business policy outside schemas.
- Decode with the selected schema library; use `.safeParse()` for Zod. TanStack Form may use a supported Standard Schema adapter, but validation does not necessarily replace editable form values with decoded/transformed output. Explicitly decode at submission before calling the adapter.
- Return and render plain user-facing strings. Never expose raw Schema issues, client errors, Effect causes, or Result objects to views.
- Preserve all messages for a field. Do not collapse validation output to `issues[0]` or `fields[name][0]`.
- Keep general/banner errors as a list of messages, not a nullable single string, when a flow can surface multiple independent problems.
- Flows/hooks own TanStack Form and translate failures into plain submit outcomes, field errors, and banner messages. Views receive controlled values, all messages, and callbacks; they do not own form state or apply asynchronous outcomes themselves.

## Accessibility

- Accessibility is a default quality bar. Use semantic roles/labels, keyboard or equivalent navigation support where applicable, and screen-reader-friendly loading, error, and success states.
- Preserve accessibility coverage for states that affect navigation, form submission, authentication, checkout or payment, destructive actions, or error recovery.
- Add accessibility-aware assertions for critical user-visible flows when the local test stack supports them.
