# ADR Format

Keep ADRs in the relevant `docs/adr/` directory and use sequential filenames such as `0001-slug.md`. Create the directory lazily.

## Template

```md
# {Short title of the decision}

{One to three sentences stating the context, accepted decision, and reason.}
```

Most ADRs need only that paragraph. Add optional status, considered options, or consequences only when they preserve information a future reader will need.

Scan the target ADR directory for the highest existing number and increment it by one.

## Qualification

Create an ADR only when the decision is hard to reverse, surprising without context, and the result of a genuine trade-off. Typical qualifying decisions include architectural shape, integration patterns, lock-in technology choices, context ownership boundaries, deliberate deviations from the obvious path, constraints invisible in code, and non-obvious rejected alternatives.
