# Frontend Review Checks

Use during independent review of changed frontend behavior, selecting checks applicable to the affected surface. Follow `$code-review` for review independence, evidence, and findings.

## Review

- Routes/pages/screens are thin and render flow/container components.
- Flows own UI orchestration; `views/` contains only full-screen presentational surfaces for complete navigable screens, while smaller presentational UI lives in `components/` at its narrowest owning boundary.
- React and React Native JSX passes meaningful callbacks through named handlers close to `return`; only short, single-expression forwarding, binding, or minor UI event/value adaptation stays inline. Direct references remain unchanged, `useCallback` follows an independent stability contract, and flows retain orchestration ownership.
- API calls go through the adapter boundary. No direct `fetch`, `axios`, SDK, or raw client calls leak into components, screens, routes, flows, views, or stores.
- Requests, success responses, and error responses are validated at the adapter boundary with Effect Schema in Effect projects or `.safeParse()` in established Zod projects.
- Endpoint error parsing and mapping use the exact operation contract, remain exhaustive, and do not hide new variants behind a broad domain schema or status-first fallback.
- In frontend-first work, only backend-calling adapter implementations are mocked. Production and mock implementations satisfy the same exact operation contracts, while every consuming frontend layer follows its production architecture.
- The default mock seam uses a pure domain-owned `create[Domain]Api` factory and an environment-aware domain API entrypoint, unless the repository has a stronger existing adapter seam. Mock scenarios do not pollute production contracts, consuming layers remain mock-unaware, and production rejects mock mode.
- Mock scenarios are deterministic, runtime-valid where schemas exist, and cover every UI-relevant contract outcome without simulating transport internals inside the mock adapter.
- For a UI-first staging review, every planned named scenario is reachable by its URL and the floating selector. Direct loading, refresh, and back/forward navigation restore it; switching scenarios does not leak stale requests, caches, or flow state into the next setup. The selector remains accessible and does not cover the screen being reviewed.
- A URL cannot enable mock mode in production. The production deployment uses the production artifact and configuration, rejects mock mode, cannot run review controls, and prevents direct access to unfinished feature routes and operations when their code is deployed behind a default-off gate. Exclude review controls from production bundles when the toolchain permits.
- Effect hooks expose exact expected failures while keeping defects and interruption distinct; apply `$engineering-effect`. Established neverthrow hooks branch on Result values for domain outcomes instead of treating TanStack Query `isError` as the domain failure path.
- Established neverthrow/TanStack mutation hooks call `ResultAsync`-returning adapters, including `ResultAsync<TData, never>` when there is no expected failure; they resolve `Result` in `data`, never reject an expected domain outcome, and expose `state` and `run` from one shared mutation instance and settlement helper.
- Routes and screens use the repository's router or framework data-loading boundary for initial-render server data when one exists; the loader or prefetch boundary and consuming hook share the query definition and cache identity, critical data is awaited, optional prefetching does not block navigation, and component or flow effects do not initiate route-critical fetches.
- Hooks consumed by flows or layouts expose an application-owned exhaustive server-state union rather than independent query flags; background refresh and refresh failure preserve usable data, and every real lifecycle state has an explicit variant.
- Flows/hooks own form and application state; views and presentational components are stateless. Forms render plain strings and preserve all field and general error messages.
- Web unit/component tests prefer `@testing-library/react`.
- Mobile unit/component tests follow the repo's existing Expo/Jest integration unless there is a documented reason to diverge.
- User-visible states and flows that affect navigation, form submission, authentication, checkout or payment, destructive actions, or error recovery include accessibility coverage.
- Frontend operational events and spans occur only at owned boundaries or meaningful transitions, satisfy `$engineering-observability`, export only through the controlled backend, and cannot form render, retry, ingestion, or provider-failure loops.
- Supported-platform flows that cover core business actions, high-traffic journeys, or failure recovery include mandatory E2E coverage.
- If a platform or E2E tool is unsupported in the repo, document the limitation and prioritize accessibility plus unit/component and flow coverage on supported platforms.
- A mock-backed frontend slice is not reported as an end-to-end complete feature; production adapter integration and live contract/integration evidence remain explicit until verified.
- Observable frontend issues have current browser, computer, emulator, or
  device Runtime Acceptance evidence. Task-level Quick changes have focused
  direct UI evidence. When an issue uses an authoritative design, it also has
  a current Design Reference Manifest and Evidence Bundle, a source-drift
  result, a complete classified Design Audit Matrix, and the required
  independent Design Audit.
- Before completion, verify every triggered check or record its omission and alternative assurance in the `$engineering-for-certainty` handoff.
