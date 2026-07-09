---
name: engineering-frontend
description: Frontend engineering doctrine for web/mobile architecture, API integration, forms, accessibility, and testing. Use with engineering-for-certainty when work touches frontend modules, routes/screens, API adapters, hooks, flows/views, TanStack Query/Form, accessibility requirements, or web/mobile unit/E2E coverage.
---

# Engineering Frontend

Use this companion skill with `$engineering-for-certainty` when work touches frontend architecture, API consumption, forms, accessibility, or web/mobile tests. Preserve repo conventions first; these rules define the default shape when the repo does not already have a stronger local convention.

## Scope

Apply this skill when the work includes:

- web routes, pages, components, flows, views, or hooks
- mobile screens, components, flows, views, or hooks
- frontend API adapter or client integration
- TanStack Query, TanStack Form, or equivalent server-state/form orchestration
- web component, flow, or hook testing
- mobile component, screen, or hook testing
- frontend accessibility requirements
- Playwright or Maestro test coverage
- frontend build-sequence changes related to testing

## Doctrine

Always follow priority 1. When priorities conflict, preserve correctness and accessibility before adding broader E2E coverage.

1. Always follow the repo's existing frontend architecture, libraries, and test stack unless the user explicitly requests a change.
2. Keep route/screen, flow, view, hook, adapter, and test boundaries explicit.
3. Validate every external boundary before data reaches UI or domain code.
4. Preserve accessibility coverage for important user-visible states.
5. Add E2E coverage for important supported-platform flows.

## Structure

- Organize frontend code by domain modules, not broad file-type buckets, where the repo structure allows it.
- Keep routes/pages/screens thin. They render one flow/container component and avoid branching orchestration logic.
- Flows own orchestration: data loading, transitions, reducer dispatch, navigation side effects, and exhaustive state matching.
- Views are presentational: explicit props in, callbacks out, minimal local logic.
- Prefer reducers over multiple related `useState` calls, especially for transition-heavy flows or two or more related state values.
- Cross-domain UI or app workflows should live in explicit flow or process modules rather than being buried inside a single domain component or screen.
- Use exhaustive matching for non-boolean discriminants. Avoid nested ternaries; use reducer transitions or pattern matching.
- Add a short comment above every `useEffect` explaining why the effect exists.

## API Integration

Default layer chain:

```text
contract -> API/client adapter -> hook -> flow -> view
```

Each arrow is a boundary. Nothing skips a layer.

- API calls live in API/client adapter modules only. Components, screens, routes, stores, flows, views, and hooks must not call `fetch`, `axios`, SDK clients, or raw clients directly unless the repo explicitly makes that hook the adapter boundary.
- Preserve the repo's adapter location. If no convention exists, use domain-owned adapters and one adapter file per domain surface.
- Validate adapter input with `.safeParse()` before making the request. Return a typed validation error and do not call the API when request validation fails.
- Validate success responses against the endpoint success contract before returning data.
- Validate error responses against the endpoint error contract before mapping them into domain errors.
- If response validation fails, map it to an explicit infrastructure failure such as `service_unavailable`; an unrecognized payload is not a domain error.
- Prefer combined per-endpoint error contracts when the repo supports them. Parse the error body once, then exhaustively match the parsed error discriminant instead of branching on HTTP status first with a broad fallback.
- Keep domain error unions as named top-level type aliases. Put domain error types and factories in the domain's frontend module, not inline inside adapter functions.
- No `try/catch` in adapters, hooks, or flows for expected failures. Use `Result`/`ResultAsync` and restrict `try/catch` to explicit bridges to throwing libraries.

## Hooks and Server State

- Prefer TanStack Query as the default server-state/query library for web and mobile projects unless the repo already standardizes on another tool.
- Hooks call API adapters and expose the adapter result without reclassifying expected domain failures as thrown exceptions.
- When a query or mutation awaits a `ResultAsync`, the resolved value is a `Result`; domain failures land in query/mutation `data`, not in Query's `error` state. Branch on `data.isOk()` / `data.isErr()` for domain outcomes.
- Use TanStack Query `isError` / `error` only for unexpected thrown failures that violate the adapter contract.
- Keep query keys short, stable, and domain-specific.

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

## Testing

- Prefer `@testing-library/react` for web unit/component tests.
- For mobile unit/component tests, prefer the repo's existing Expo/Jest integration, commonly `expo-jest`, instead of introducing a parallel test stack without a strong reason.
- Web E2E tests are mandatory for important user-visible flows when a web app exists. Prefer Playwright.
- Mobile E2E tests are mandatory for important user-visible flows when a mobile app exists. Prefer Maestro.
- Mirror production file naming. Follow the repo's naming style; otherwise prefer `test()` and given/when/then test bodies.

## Build Sequence

For web or mobile features:

1. Plan/gap review, including UI states, error states, accessibility states, and route/screen ownership.
2. Shared contracts and API adapter types.
3. API adapter tests for request validation, response validation, and error mapping.
4. Hook tests for query/mutation behavior and `Result` branches.
5. Flow tests for reducer transitions, orchestration, navigation, and submit outcomes.
6. View/component tests for rendering, permissions, validation, success states, and error states.
7. Implementation from API adapter inward to hook, flow, and view.
8. Accessibility checks for critical user-visible states and flows.
9. E2E coverage for important happy-path and failure-path user flows on supported platforms.

## Review

- Routes/pages/screens are thin and render flow/container components.
- Flows own orchestration; views stay presentational.
- API calls go through the adapter boundary. No direct `fetch`, `axios`, SDK, or raw client calls leak into components, screens, routes, flows, views, or stores.
- Requests, success responses, and error responses are validated at the adapter boundary with `.safeParse()`.
- Endpoint error mapping is exhaustive and does not hide new typed variants behind a broad status-first fallback.
- Hooks branch on `Result` values for domain outcomes instead of treating TanStack Query `isError` as the domain failure path.
- Forms render plain user-facing strings and preserve all field and general error messages.
- Web unit/component tests prefer `@testing-library/react`.
- Mobile unit/component tests follow the repo's existing Expo/Jest integration unless there is a documented reason to diverge.
- User-visible states and flows that affect navigation, form submission, authentication, checkout or payment, destructive actions, or error recovery include accessibility coverage.
- Supported-platform flows that cover core business actions, high-traffic journeys, or failure recovery include mandatory E2E coverage.
- If a platform or E2E tool is unsupported in the repo, document the limitation and prioritize accessibility plus unit/component and flow coverage on supported platforms.
