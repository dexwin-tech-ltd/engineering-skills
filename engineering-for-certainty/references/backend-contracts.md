# Boundaries, Architecture, and Errors

For new Effect projects, apply `$engineering-effect`: Effect Schema replaces Zod at value boundaries; explicit Effect contracts replace internal Result/ResultAsync; Context services and Layers provide dependency injection; application/request scopes own lifetimes; and the Effect HTTP stack is preferred. The common ownership and exact-error rules below still apply. References to Zod, neverthrow, Fastify factories, and `withTransaction(tx)` describe established non-Effect projects. Preserve those paths unless migration is separately authorized.

Read the relevant sections when work changes external input, backend modules, services, persistence, public contracts, or error behavior.

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
- Domain services contain business rules and expose explicit Effect signatures in Effect projects, or `Result<T, E>` / `ResultAsync<T, E>` in established Result projects.
- Repositories own persistence access and validate database records at the persistence boundary.
- Prefer Drizzle ORM and Kit unless the repo already standardizes on another persistence stack. In Effect projects prefer a verified native Effect driver; direct Effect SQL may serve queries that benefit from it. Keep connection and transaction ownership coherent, validate database records, and preserve isolated migration proof.
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
- In Effect projects use typed Context services and Layer assembly. In established non-Effect projects use strict constructor or factory dependency injection.
- In non-Effect projects, build one typed `deps` object at startup and inject dependencies into module factories.
- Do not use Fastify decorate as a dependency container.
- Keep request-scoped context explicit in parameters or typed Effect service requirements.
- Do not read global config or environment inside services or repositories.
- Assemble live repositories at startup. In non-Effect projects instantiate them with `db` and expose `withTransaction(tx)` for transaction-scoped instances; Effect projects use the verified SQL transaction context described in `$engineering-effect`.
- Services own transaction boundaries.
- Keep cross-domain orchestration explicit and deterministic.

### Auth Boundaries

- Keep authentication and authorization at explicit, typed boundaries.
- Pass actor context explicitly into services; do not hide it in globals.
- Keep routes thin, enforce permissions on the backend, and map auth outcomes exhaustively.
- For framework-specific extraction/guard wiring, cookie policy, CSRF, and permission or policy registries, apply `$engineering-auth-security`.
- Map unauthorized (401) and forbidden (403) outcomes separately from service-unavailable infrastructure failures.

## Errors and Results

In Effect projects the expected-failure channel carries producer-owned failures; defects and interruption remain distinct and are handled at designated execution boundaries. Do not wrap every Effect in Result or relabel a defect as an expected domain failure. The Result-specific total-contract and thrown-error rules below apply to established Result projects. Read `$engineering-effect` for the Effect boundary policy.

- Treat expected application failures as values. Domain services and exported callable operations must not throw them.
- Expose the narrowest truthful closed error type from each producer: include every error variant it can emit and no variant it cannot emit.
- Apply producer-owned truthfulness recursively. Each error code owns its exact
  required or forbidden details shape, and every nested issue discriminant is
  coupled only to payloads a real producer branch can emit. Reject
  optional-property bags, Cartesian products, and nested combinations that no
  producer path can construct.
- Use named top-level error aliases in public operation, service, and repository signatures, not inline unions inside Effect or Result/ResultAsync signatures.
- Reuse domain, infrastructure, authorization, and provider error variants as atomic types and factories. Compose them into a producer-specific error union at each operation boundary.
- Do not use a global, project-wide, or domain-wide umbrella error union as an operation's return type unless that operation can genuinely emit every variant in it.
- Expose a success-only contract or `Result<T, never>` when an operation cannot emit an application-level error under its declared boundary policy; do not invent defensive variants.
- Restrict `try/catch` to exported callable boundaries, adapters/clients, and explicit bridges to throwing libraries.
- Keep expected application failures as exact `Result` / `ResultAsync` values
  through internal helper graphs. Helper callers propagate those variants
  exhaustively; they do not throw a typed application error for an outer catch
  block to recover.
- Normalize unexpected execution failures observable by a boundary that promises a total `Result` contract into a sanitized, boundary-owned variant such as `internal_error`.
- Treat a typed application error that is thrown instead of returned as an
  unexpected transport defect and normalize it to the boundary-owned internal
  failure. Do not inspect caught values to restore an expected application
  code.
- Treat network, process, provider, SDK, and platform failures outside an operation's execution as failures introduced by the calling boundary, not emitted by the called operation.
- Widen an error union only with variants that the consuming boundary or adapter can itself introduce. Otherwise preserve the producer's exact error union.
- Test and map every error variant exhaustively at the consuming boundary. Adding a producer variant should break affected consumers until they handle it.
- Include actionable, sanitized `details` in HTTP/API error responses when the client can use them to understand or correct the failure.
- Keep reusable error atoms and factories with their owning domain or infrastructure module; shared ownership does not make an error variant part of every operation contract.
- Prefer discriminated `_tag` variants in Effect projects or established `type` variants in Result projects, with small factory helpers; do not scatter inline `{ type: "..." as const }` shapes across the codebase.

## Backend Build Sequence

For endpoint or service work:

1. Plan/gap review.
2. Contracts: request, response, domain, and error schemas/types.
3. Boundary validation tests.
4. Service tests covering the selected Effect or Result failure contract.
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

For all Drizzle projects, edit schema source and run the generator; do not hand-edit generated SQL or metadata. Use an explicit custom migration for authored SQL. Preserve the Migration Proof Harness regardless of runtime integration.

For Fastify projects:

- Declare request/response schemas in route metadata for OpenAPI when the project supports it.
- Define API request/response schemas from canonical Zod schemas plus a transformer built on `zod-to-json-schema`.
- Do not hand-author JSON Schema for API schemas except for unavoidable framework gaps.
- Use one dedicated response schema per status code.
- Validate response payloads before sending. Prefer one reusable helper that validates against the Zod schema and prevents invalid output from leaving the boundary.
- For protected routes, use the shared auth extractor or guard wiring instead of duplicating token parsing in handlers.
- Keep actor context flow explicit from request boundary to service call; do not fetch auth context from hidden globals inside services.
