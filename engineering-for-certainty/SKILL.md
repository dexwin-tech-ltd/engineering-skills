---
name: engineering-for-certainty
description: Core certainty-first coding doctrine for software projects. Use by default for implementation, review, debugging, planning, and code generation; pair with companion skills when work touches observability, resilience, auth/security, or frontend testing.
---

# Engineering for Certainty

Use this skill by default for coding projects. It is the core doctrine: keep the defaults broadly applicable, preserve the repo's existing stack unless the user asks to migrate, and pull in companion skills only when the work touches those specialized areas.

## Priorities

- Always validate boundaries.
- Keep adapters thin and business logic explicit.
- Use `Result<T, E>` / `ResultAsync<T, E>` for expected failures.
- Write the right tests first.
- Apply specialized companion skills only when the work touches those areas.

## Companion Skills

Apply these only when relevant:

- Use `$engineering-observability` for centralized logging, Better Stack setup, correlation/request IDs, sanitization/redaction, and frontend log ingestion.
- Use `$engineering-resilience` for external-call timeouts/retries, mutation safety, concurrency, idempotency, queues, cron, webhooks, and async processing.
- Use `$engineering-auth-security` for cookies, sessions, CSRF, token handling, auth boundaries, and permission enforcement.
- Use `$engineering-frontend-testing` for web/mobile test-stack choices, accessibility quality bars, and web/mobile E2E doctrine.

## Preferred Defaults

- Language: TypeScript.
- Package manager: pnpm.
- Web frontend: React + Vite + React Router + Tailwind.
- Mobile: Expo React Native.
- Backend: Fastify.
- Validation: Zod.
- Result library: neverthrow.
- Path aliases: prefer `@` and `#` aliases over deep relative imports when the repo supports them; add aliases for new projects.

## First Move

Before editing code:

1. Read the relevant local instructions, README, docs, ADRs, plan files, and nearby implementation examples.
2. Identify the repo's equivalents for contracts, adapters/routes/controllers, services/domain logic, repositories/persistence, hooks, flows, views, and tests.
3. For non-trivial work, perform a gap review before implementation. Resolve missing scope, contracts, errors, persistence behavior, orchestration, UI states, and test strategy.
4. Do not implement from an inconsistent plan. Update the plan or state the unresolved gap first.

## Trust Boundaries

All external input is untrusted.

- Validate HTTP requests, query params, route params, forms, storage reads, environment-derived config, and remote API responses at the boundary with Zod.
- Use `.safeParse()` at user/API boundaries; never pass raw request bodies or unvalidated remote data into services/domain logic.
- Return user-facing validation messages, not raw Zod issue objects.
- Put shared request/response/domain schemas in the repo's shared contract package. If API and domain shapes diverge, define explicit API and domain variants and map at the boundary.
- Schemas that validate persisted database row/document shapes should be named with `Db` so the persistence boundary is visible at call sites.
- Normalize canonical values at the boundary before persistence or identity-critical lookup.
- Validate returned data at the boundary before sending it out. Prefer one reusable schema-validation helper so output validation stays consistent.

## Architecture

Keep adapters thin and domain logic explicit.

- Routes/controllers/adapters handle auth checks, parsing, validation, mapping, and response presentation only.
- Domain services contain business rules and return `Result<T, E>` or `ResultAsync<T, E>`.
- Repositories own persistence access and validate database records at the persistence boundary.
- Cross-app or cross-feature logic belongs in shared packages/modules, not copied across apps.
- Do not read `process.env` or global config inside business logic. Inject configuration explicitly.
- Avoid hidden global state; prefer pure functions and explicit dependencies.
- Model mutually exclusive state as discriminated unions, not scattered booleans.
- Exhaustively handle discriminated unions. Adding a variant should break compilation until every branch is handled.

## Errors and Results

- Domain services must not throw for expected failures.
- Do not use `try/catch` inside domain services. Restrict `try/catch` to adapter/client boundaries or explicit bridges to throwing libraries.
- Public service/repository signatures should use named top-level error aliases, not inline unions inside `Result`/`ResultAsync`.
- Every error variant should be tested and mapped exhaustively at the boundary.
- HTTP/API error responses should include actionable, sanitized `details` when the client can use them to fix or understand the failure.
- Unknown infrastructure failures should be normalized into explicit error variants such as `service_unavailable`.

## Frontend and Mobile Structure

Use the same certainty rules in UI code.

- Screens/pages/routes stay thin. They render one flow/container component and avoid branching orchestration logic.
- Flows own orchestration: data loading, transitions, reducer dispatch, navigation side effects, and exhaustive state matching.
- Views are presentational: explicit props in, callbacks out, minimal local logic.
- Prefer reducers over multiple related `useState` calls, especially for transition-heavy flows or 2+ related state values.
- API calls live in API/client adapter modules only. Components, screens, routes, stores, and hooks must not call `fetch`, `axios`, or raw clients directly.
- Hooks call API adapters and expose discriminated status unions such as `idle | loading | error | loaded`; avoid flat `isLoading`/`isError` state leaking through the app.
- Validate all form inputs with Zod schemas. Reuse field schemas across blur validation and submit/step validation.
- Extract and display user-facing validation messages; never expose raw Zod errors in UI.
- Use exhaustive matching for non-boolean discriminants. Avoid nested ternaries; use reducer transitions or pattern matching.
- Add a short comment above every `useEffect` explaining why the effect exists.

## Testing Doctrine

TDD is mandatory by default.

- For new features and behavior changes: write the failing test first, implement the minimum to pass, then refactor with tests green.
- If TDD is intentionally skipped, state the concrete reason before implementation.
- Cover success paths, failure paths, validation failures, error variants, and exhaustive mapping.
- For API endpoints, test status codes, response payloads, and actionable error details.
- API integration tests are mandatory for endpoint changes. Exercise the real app wiring end-to-end through route, service, and persistence layers; mock only true external systems at the boundary.
- When work touches observability, resilience, auth/security, or frontend testing/accessibility, apply the relevant companion skill and test those behaviors explicitly.
- Prefer test names that state given/when/then behavior. If the repo has a local test naming style, follow it.
- For debugging: first build a deterministic repro loop. Convert the minimized repro into a regression test before fixing when a valid seam exists.

## Backend Build Sequence

For endpoint or service work:

1. Plan/gap review.
2. Contracts: request, response, domain, and error schemas/types.
3. Boundary validation tests.
4. Service tests using `Result`/`ResultAsync` errors.
5. Repository/persistence tests when persistence behavior changes.
6. Implementation: repository, service, route/controller.
7. Exhaustive error mapping and response validation.
8. Mandatory integration tests for key behavior and failure cases.

For Fastify projects:

- Declare request/response schemas in route metadata for OpenAPI when the project supports it.
- Define API request/response schemas from canonical Zod schemas plus a transformer built on `zod-to-json-schema`.
- Do not hand-author JSON Schema for API schemas except for unavoidable framework gaps.
- Use one dedicated response schema per status code.
- Validate response payloads before sending. Prefer one reusable helper that validates against the Zod schema and prevents invalid output from leaving the boundary.
- For database migrations, edit schema source files and run the generator. Do not hand-edit generated migration metadata.

## Frontend Build Sequence

For web or mobile features:

1. Plan/gap review, including UI states and error states.
2. Shared contracts and API adapter types.
3. API adapter tests for request/response validation and error mapping.
4. Hook tests for query/mutation behavior and status unions.
5. Flow tests for reducer transitions and orchestration.
6. View/component tests for rendering, permissions, success states, and error states.
7. Implementation from API adapter inward to hook, flow, and view.
8. Apply `$engineering-frontend-testing` when the work touches platform-specific unit-test stacks, accessibility, or E2E coverage.

## Review Checklist

Before declaring work complete, verify:

- No unvalidated external data reaches domain logic.
- No business logic lives in routes/controllers/pages/views.
- Expected failures use `Result`/`ResultAsync`, not thrown exceptions.
- Error unions and UI states are discriminated and exhaustively handled.
- Config is injected rather than read from globals inside business logic.
- Returned data is validated at the boundary before it is sent back out.
- Tests cover the new behavior, validation, and error variants.
- Relevant companion skills were applied when work touched observability, resilience, auth/security, or frontend testing/accessibility.
- New shared contracts or logic live in shared packages/modules when used across boundaries.
- Imports follow repo aliases (`@`, `#`) instead of brittle deep relative paths where possible.
- Any skipped doctrine rule has a concrete, documented reason.
