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
- Use `$engineering-auth-security` for cookies, sessions, CSRF, token handling,
  auth boundaries, permission enforcement, and shared permission-registry
  conventions.
- Use `$engineering-frontend-testing` for web/mobile test-stack choices, accessibility quality bars, and web/mobile E2E doctrine.

## Preferred Defaults

- Language: TypeScript.
- Package manager: pnpm.
- Web frontend: React + Vite + TanStack Router + Tailwind.
- Mobile: Expo React Native.
- Form UI library (web/mobile): TanStack Form.
- Server state and query orchestration (web/mobile): TanStack Query.
- Backend: Fastify.
- Database queries: Drizzle.
- Validation: Zod.
- Result library: neverthrow.
- Path aliases: prefer `@` and `#` aliases over deep relative imports when the repo supports them; add aliases for new projects.

## Linting and Formatting

- For TypeScript/JavaScript projects that do not already standardize on another tool, prefer ESLint for linting and Prettier for formatting.
- Preserve the repo's existing toolchain when one already exists; do not migrate a repo from Biome, dprint, Rome, Standard, or another established formatter/linter unless the user explicitly asks.
- Use ESLint for correctness, maintainability, and bug-prevention rules. Use Prettier for layout and whitespace only. Do not duplicate formatting rules in ESLint.
- Prefer ESLint flat config for new projects.
- For TypeScript projects, prefer `typescript-eslint` for parsing and TypeScript-aware rules.
- In monorepos, ESLint and Prettier should be wired to work consistently across apps and packages. Prefer shared root config or shared config packages over drifting per-package defaults.
- Prefer repo scripts that make the distinction explicit: `lint`, `lint:fix`, `format`, and `format:check`.

### Recommended ESLint Defaults

- `@typescript-eslint/no-unused-vars` with `_`-prefixed unused parameters/variables ignored when intentionally unused.
- `@typescript-eslint/no-floating-promises`.
- `@typescript-eslint/no-misused-promises`.
- `@typescript-eslint/consistent-type-imports`.
- `@typescript-eslint/switch-exhaustiveness-check`.
- `eqeqeq`.
- `curly`.
- `prefer-const`.

### Recommended Prettier Defaults

- Keep Prettier opinionated and minimal; avoid style bikeshedding.
- For new projects without an existing convention, default to:
  - `printWidth: 100`
  - `singleQuote: true`
  - `trailingComma: "all"`
  - `semi: true`
  - `arrowParens: "always"`
- If a repo already has an established formatting style, keep that style instead of reformatting unrelated code.

### Recommended Git Hook Defaults

- Prefer local git hooks or equivalent local automation for fast feedback when the repo supports them.
- Pre-commit should run formatting and linting against staged files, plus the unit-test command when the repo's unit suite is reliably fast enough for commit-time feedback.
- Pre-push should run the broader test gates: full unit tests, integration tests, and E2E tests when those commands already exist in the repo.
- Prefer explicit repo scripts for hook entry points, such as `test:unit`, `test:integration`, and `test:e2e`.
- Do not silently skip missing commands. Either wire hooks only to commands that exist or fail with a clear message so the repo's guarantees stay trustworthy.

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
- Prefer Drizzle as the default database query package for new work unless the repo already standardizes on another persistence stack.
- Cross-app or cross-feature logic belongs in shared packages/modules, not copied across apps.
- Do not read `process.env` or global config inside business logic. Inject configuration explicitly.
- Avoid hidden global state; prefer pure functions and explicit dependencies.
- Model mutually exclusive state as discriminated unions, not scattered booleans.
- For closed sets of values, prefer enums or const-backed literal unions over
  booleans and unconstrained strings. Use discriminated literal variants where
  exhaustive matching is required.
- Exhaustively handle discriminated unions. Adding a variant should break compilation until every branch is handled.
- Organize backend, frontend, and shared packages by domain ownership rather than broad technical buckets.
- Shared packages such as contracts should mirror domain ownership rather than becoming global dumping grounds.
- Default to kebab-case for non-component filenames. React component files should use the component name as the filename.

### Backend Module Structure

- For API backends, organize code by domain modules under
  `apps/api/src/modules`.
- Domain directory names should be plural and kebab-case where applicable.
- Do not default to top-level file-type buckets inside modules such as
  `routes`, `services`, or `repositories`. Prefer domain-local files such as:
  - `[domain].route.ts`
  - `[domain].service.ts`
  - `[domain].repository.ts`
  - `[domain].errors.ts`
- Normalize non-conforming filenames during structural refactors, such as
  snake_case to kebab-case.
- Keep access-control or identity modules focused on authentication,
  authorization, session, and identity-lifecycle concerns.
- Business-domain behavior should be owned by its business module even when
  exposed through access-control-adjacent routes or entrypoints.
- Cross-domain workflows belong in first-class process modules under
  `modules`; do not force cross-domain orchestration into a single business
  domain module.
- Allow helper files only when necessary. Cross-module helpers belong in shared
  `src/lib`, module-specific helpers may live in module-local `lib` folders,
  and tiny one-off helpers should stay in the owning domain file.
- Keep wiring in a dedicated bootstrap layer. `app.ts` stays thin and focused
  on server setup and module route registration.
- Register one route entrypoint per module in app wiring.
- Use strict constructor or factory dependency injection only.
- Build one typed `deps` object at startup and inject dependencies into module
  factories.
- Do not use Fastify decorate as a dependency container.
- Keep request-scoped context explicit in function parameters.
- Do not read global config or environment inside services or repositories.
- Instantiate repositories once with `db` at startup.
- Repositories should expose `withTransaction(tx)` to produce transaction-scoped
  instances with the same interface.
- Services own transaction boundaries.
- Keep cross-domain orchestration explicit and deterministic.

## Errors and Results

- Domain services must not throw for expected failures.
- Do not use `try/catch` inside domain services. Restrict `try/catch` to adapter/client boundaries or explicit bridges to throwing libraries.
- Public service/repository signatures should use named top-level error aliases, not inline unions inside `Result`/`ResultAsync`.
- Every error variant should be tested and mapped exhaustively at the boundary.
- HTTP/API error responses should include actionable, sanitized `details` when the client can use them to fix or understand the failure.
- Unknown infrastructure failures should be normalized into explicit error variants such as `service_unavailable`.
- Each module owns its domain errors in `[domain].errors.ts`.
- Cross-cutting errors belong in shared infrastructure errors.
- Continue using shared infrastructure error variants for technical failures,
  such as `service_unavailable`.
- Cross-module authorization or permission failures belong in shared
  infrastructure or authorization error layers, not a single business module.
- Define module errors with exported variant constants, named error types, and
  small factory helpers in `[domain].errors.ts`.
- Prefer discriminated `type` variants for service and repository errors; do not
  scatter inline `{ type: "..." as const }` shapes across the codebase.

## Frontend and Mobile Structure

Use the same certainty rules in UI code.

- Organize frontend code by domain modules as well, not broad file-type buckets
  as the default. Screens, flows, views, hooks, client adapters, and tests
  should stay aligned to the same domain ownership model where the repo
  structure allows it.
- Shared packages such as contracts should use domain folders with per-endpoint,
  per-feature, or per-workflow files plus domain-level exports.
- Cross-domain UI or app workflows should live in explicit flow or process
  modules rather than being buried inside a single domain component or screen.
- Screens/pages/routes stay thin. They render one flow/container component and avoid branching orchestration logic.
- Flows own orchestration: data loading, transitions, reducer dispatch, navigation side effects, and exhaustive state matching.
- Views are presentational: explicit props in, callbacks out, minimal local logic.
- Prefer reducers over multiple related `useState` calls, especially for transition-heavy flows or 2+ related state values.
- API calls live in API/client adapter modules only. Components, screens, routes, stores, and hooks must not call `fetch`, `axios`, or raw clients directly.
- Prefer TanStack Query as the default server-state/query library for both web and mobile projects.
- Hooks call API adapters and expose discriminated status unions such as `idle | loading | error | loaded`; avoid flat `isLoading`/`isError` state leaking through the app.
- Prefer TanStack Form as the default form-state and form-UI orchestration library for both web and mobile projects.
- Validate all form inputs with Zod schemas. Reuse field schemas across blur validation and submit/step validation.
- Extract and display user-facing validation messages; never expose raw Zod errors in UI.
- Use exhaustive matching for non-boolean discriminants. Avoid nested ternaries; use reducer transitions or pattern matching.
- Add a short comment above every `useEffect` explaining why the effect exists.

## Testing Doctrine

TDD is mandatory by default.

- For new features and behavior changes: write the failing test first, implement the minimum to pass, then refactor with tests green.
- If TDD is intentionally skipped, state the concrete reason before implementation.
- When setting up repo automation, prefer commit-time hooks for fast checks and push-time hooks for broader suites, while keeping CI as the authoritative full-environment validation.
- Cover success paths, failure paths, validation failures, error variants, and exhaustive mapping.
- For API endpoints, test status codes, response payloads, and actionable error details.
- API integration tests are mandatory for endpoint changes. Exercise the real app wiring end-to-end through route, service, and persistence layers; mock only true external systems at the boundary.
- When work touches observability, resilience, auth/security, or frontend testing/accessibility, apply the relevant companion skill and test those behaviors explicitly.
- Prefer test names that state given/when/then behavior. If the repo has a local test naming style, follow it.
- For debugging: first build a deterministic repro loop. Convert the minimized repro into a regression test before fixing when a valid seam exists.
- Test file naming should mirror production file naming, such as
  `[domain].route.test.ts`, `[domain].service.test.ts`, and
  `[domain].repository.test.ts`.
- Structural migrations must preserve behavior and prove that with tests.
- Keep test structure aligned with the real module structure.

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

- Contracts in `packages/contracts` should align with domain ownership.
- Prefer per-endpoint contract files within a domain folder plus domain-level
  exports.
- Keep existing endpoint paths stable during structural migrations.
- Keep behavior stable during structural migrations: no response, status-code,
  or message drift.
- For large structural refactors, prefer codemod-style file moves and import
  rewrites first, then targeted manual cleanup.

For Fastify projects:

- Declare request/response schemas in route metadata for OpenAPI when the project supports it.
- Define API request/response schemas from canonical Zod schemas plus a transformer built on `zod-to-json-schema`.
- Do not hand-author JSON Schema for API schemas except for unavoidable framework gaps.
- Use one dedicated response schema per status code.
- Validate response payloads before sending. Prefer one reusable helper that validates against the Zod schema and prevents invalid output from leaving the boundary.
- For Drizzle-backed database migrations, edit schema source files and run the generator. Never hand-edit generated migration files or generated migration metadata.

## Frontend Build Sequence

For web or mobile features:

1. Plan/gap review, including UI states and error states.
2. Shared contracts and API adapter types.
3. API adapter tests for request/response validation and error mapping.
4. Hook tests for query/mutation behavior and status unions.
5. Flow tests for reducer transitions and orchestration.
6. View/component tests for rendering, permissions, success states, and error states.
7. Implementation from API adapter inward to TanStack Query hook, flow, and view.
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
- Formatter and linter expectations are satisfied for the changed scope; ESLint owns code-quality rules and Prettier owns formatting.
- Hook-driven local validation matches repo policy for the changed scope: pre-commit runs staged format/lint plus unit tests, and pre-push runs unit, integration, and E2E tests when the repo defines those commands.
- Any skipped doctrine rule has a concrete, documented reason.
- Relevant unit and integration tests pass for the changed scope.
- No stale imports remain to old module paths or legacy naming.
- App wiring imports only module route entrypoints.
- Module files and directories follow the agreed naming conventions.
- Domain ownership stays consistent across backend, frontend, and shared
  packages for the changed scope.
