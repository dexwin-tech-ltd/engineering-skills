# Style and Clarity

Use these principles when writing new or materially changed code. Style and
clarity concern whether code communicates its purpose and safe change path,
not formatting preference or aesthetic uniformity.

## Apply to the changed concern

Identify the **changed concern**: the bounded behavior, contract, or
implementation pattern that the code owns. Inspect the enclosing callable, its
direct consumers, nearby equivalent code, applicable `CONTEXT.md`, and
activated doctrine. Follow an established local convention unless an accepted
user or repository rule deliberately improves it in scope.

Do not turn a scoped change into unrelated legacy cleanup. Do not introduce a
competing pattern for the same concern unless it makes that concern clearer or
safer to change.

## Authoring principles

### Value contracts

Prefer a declared schema for untrusted input and any cohesive internal value
contract when it replaces scattered manual checks with a clearer contract.
Prefer Zod when the repository already uses or explicitly selects it. Do not
require a schema for ordinary internal control flow, a proven performance
constraint, or a repository that deliberately uses another validator.

Keep policy that depends on live state, permissions, time, persistence, or
external calls outside the schema in a named business step.

### Transformations and effects

Prefer a functional core: named transformations and decisions whose inputs and
outputs can be followed without hidden mutation or I/O. Keep persistence,
network calls, logging, time, framework lifecycle work, and other effects in a
named imperative boundary.

When a sequence of transformations is the story of the code, prefer the
repository's established fluent pipeline form, for example:

```ts
return pipe(rawInput)
  .andThen(parseCreateOrder)
  .andThen(normalizeOrder)
  .andThen(priceOrder)
  .andThen(approveOrder)
```

Do not require a new pipe package. A generic fluent pipeline is for ordinary
transformations; fallible work retains native `Result` or
`ResultAsync` `.andThen(...)` semantics. Do not hide transaction ownership,
resource lifetime, retries, or meaningful I/O in a chain when an explicit
imperative form is clearer.

Prefer functions with one semantic purpose and named stages at meaningful
boundaries—validation, transformation, policy decision, or effect. Do not
enforce line counts, one-expression functions, or helpers whose names conceal
the work.

### Names and comments

Require names that express a value's domain meaning or a function's observable
operation. Prefer complete words over unexplained acronyms or abbreviations.
Allow a short form only when it is already clearer to the intended maintainers:
a canonical domain term, language or framework convention, familiar unit, or
tiny obvious local scope. Names such as `data`, `result`, `context`, `process`,
or `handle` need a meaningful qualifier when their role is not otherwise
obvious.

Let names and structure explain what the code does. Add or preserve a short
rationale comment only when it captures a non-obvious invariant,
ordering, compatibility or security constraint, trade-off, workaround removal
condition, or external-system fact that code cannot communicate. Remove or
avoid stale, contradictory, or narrating comments. When `$engineering-frontend`
applies, its requirement for a brief what-and-why comment above every React or
React Native `useEffect` remains in force.

### Control flow and state

Prefer guard clauses for exceptional exits when they leave the normal path
clear. Keep a short local conditional when it is clearer than an abstraction.
For several mutually exclusive domain or lifecycle states, prefer a named
discriminated union with exhaustive handling over related Boolean flags or
nested conditions that permit contradictory states. Extract a named decision
when a compound condition carries business meaning.
