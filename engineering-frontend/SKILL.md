---
name: engineering-frontend
description: Frontend engineering doctrine for web/mobile architecture, API integration, forms, accessibility, operational telemetry and tracing, and testing. Use with engineering-for-certainty when work touches frontend modules, routes/screens, API adapters, hooks, flows/views, TanStack Query/Form, accessibility, client telemetry, or web/mobile unit/E2E coverage.
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
- frontend operational logging, tracing, or telemetry emission
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
- Put non-trivial reducers in a `reducers/` folder inside the relevant domain module, flow, or feature module instead of colocating reducer transition logic inside route, screen, or view files.
- Cross-domain UI or app workflows should live in explicit flow or process modules rather than being buried inside a single domain component or screen.
- Use exhaustive matching for non-boolean discriminants. Avoid nested ternaries; use reducer transitions or pattern matching.
- In React and React Native hooks or components, add a brief comment above every `useEffect` explaining what that effect does and why it exists.

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
- Validate and parse the exact contract for the operation being called. Do not parse operation responses through a broader domain-wide or project-wide error schema.
- If response validation fails, map it to an explicit infrastructure failure such as `service_unavailable`; an unrecognized payload is not a domain error.
- Prefer combined per-endpoint error contracts when the repo supports them. Parse the error body once, then exhaustively match the parsed error discriminant instead of branching on HTTP status first with a broad fallback.
- Keep domain error unions as named top-level type aliases. Put domain error types and factories in the domain's frontend module, not inline inside adapter functions.
- Return the operation's exact error union plus only failures introduced by the adapter itself, such as client-side input validation, transport failure, SDK rejection, or malformed response data.
- Map transport, SDK, provider, and malformed-response failures to adapter-owned infrastructure variants such as `service_unavailable`; do not attribute them to the remote operation.
- No `try/catch` in adapters, hooks, or flows for expected failures. Use `Result`/`ResultAsync` and restrict `try/catch` to explicit bridges to throwing libraries.

## Hooks and Server State

- Prefer TanStack Query as the default server-state/query library for web and mobile projects unless the repo already standardizes on another tool.
- Hooks call API adapters and expose the adapter result without reclassifying expected domain failures as thrown exceptions.
- Hooks and flows receive the adapter's exact result type; do not widen it to a convenient global error union.
- When a query or mutation awaits a `ResultAsync`, the resolved value is a `Result`; domain failures land in query/mutation `data`, not in Query's `error` state. Branch on `data.isOk()` / `data.isErr()` for domain outcomes.
- Use TanStack Query `isError` / `error` only for unexpected thrown failures that violate the adapter contract.
- When flows or layouts consume a server-state hook, map library-specific flags into an application-owned discriminated union. Do not make consumers infer state from independent combinations of `isPending`, `isError`, `isFetching`, `data`, and `error`.
- At minimum, distinguish `initial-loading`, terminal `initial-failure`, `ready`, `refreshing`, and `refresh-failure`. Refresh states retain usable data. Add explicit variants such as `idle` or `paused` when the query lifecycle includes them instead of collapsing those states into loading.
- Reuse a generic application-owned mapper when multiple hooks share the same lifecycle. Domain hooks supply their exact data and failure types, map unexpected query failures, and retain the last usable value by query identity when expected refresh failures must preserve stale data.
- Require flows and layouts to match the resulting union exhaustively. Prefer the repository's existing exhaustive-matching mechanism; do not introduce a pattern-matching dependency only to copy this example.
- Keep query keys short, stable, and domain-specific.

For an enabled query, a shared mapper can preserve the boundary without widening domain failures:

```ts
type ServerState<TData, TFailure> =
  | { status: "initial-loading" }
  | { status: "initial-failure"; failure: TFailure }
  | { status: "ready"; data: TData }
  | { status: "refreshing"; data: TData }
  | { status: "refresh-failure"; data: TData; failure: TFailure };

type QueryStateInput<TData, TFailure> = Pick<
  UseQueryResult<Result<TData, TFailure>, unknown>,
  | "data"
  | "error"
  | "isLoading"
  | "isLoadingError"
  | "isRefetching"
  | "isRefetchError"
>;

type RetainedData<TData> =
  | { status: "absent" }
  | { status: "present"; data: TData };

function mapQueryToServerState<TData, TFailure>(
  query: QueryStateInput<TData, TFailure>,
  retainedData: RetainedData<TData>,
  mapUnexpectedFailure: (error: unknown) => TFailure,
): ServerState<TData, TFailure> {
  const currentData: RetainedData<TData> = query.data?.isOk()
    ? { status: "present", data: query.data.value }
    : { status: "absent" };
  const usableData =
    currentData.status === "present" ? currentData : retainedData;

  if (query.isLoadingError) {
    return {
      status: "initial-failure",
      failure: mapUnexpectedFailure(query.error),
    };
  }

  if (query.isRefetchError) {
    const failure = mapUnexpectedFailure(query.error);
    return usableData.status === "present"
      ? { status: "refresh-failure", data: usableData.data, failure }
      : { status: "initial-failure", failure };
  }

  if (query.isRefetching && usableData.status === "present") {
    return { status: "refreshing", data: usableData.data };
  }

  if (query.data?.isErr()) {
    return retainedData.status === "present"
      ? {
          status: "refresh-failure",
          data: retainedData.data,
          failure: query.data.error,
        }
      : { status: "initial-failure", failure: query.data.error };
  }

  if (currentData.status === "present") {
    return { status: "ready", data: currentData.data };
  }

  if (query.isLoading) {
    return { status: "initial-loading" };
  }

  throw new Error("Query lifecycle requires another ServerState variant");
}
```

Keep retained data keyed to the query identity so data from one query cannot appear in another. A domain hook returns `ServerState<Profile, ProfileLoadFailure>`; a flow or layout consumes it without importing TanStack Query flags:

```tsx
import { match } from "ts-pattern";

return match(profileState)
  .with({ status: "initial-loading" }, () => <ProfileSkeleton />)
  .with({ status: "initial-failure" }, ({ failure }) => (
    <ProfileLoadError failure={failure} />
  ))
  .with({ status: "ready" }, ({ data }) => <ProfileView profile={data} />)
  .with({ status: "refreshing" }, ({ data }) => (
    <ProfileView profile={data} refreshing />
  ))
  .with({ status: "refresh-failure" }, ({ data, failure }) => (
    <ProfileView profile={data} refreshFailure={failure} />
  ))
  .exhaustive();
```

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

## Client Observability

Apply `$engineering-observability` whenever frontend code emits operational telemetry. It owns signal contracts, propagation, ingestion, export, privacy, retention, and capacity; this skill owns where and when meaningful client events and spans begin.

### Operational Events

Emit a Frontend Operational Event only at an owned boundary or meaningful transition:

- an application error boundary or unhandled client failure
- an API adapter transport failure after its bounded retry policy
- an API response that violates its declared contract
- an unexpected auth or session transition
- an unexpected failure of a critical user workflow
- an offline or online transition that materially changes an active workflow
- an aggregate client-logger health signal such as a dropped-event count

Do not log component renders, routine effects, keystrokes, form values, every request or response, every navigation or click, expected validation or domain failures without a named operational requirement, product analytics, raw errors, stacks, console arguments, storage values, arbitrary objects, or URLs with query strings.

- Emit once per failure occurrence or meaningful state transition, not once per render, retry, or observer.
- Deduplicate repeated signatures in a bounded window and aggregate repeated low-value events into counts.
- Disable debug events in production and sample informational events when complete collection is unnecessary.
- Use a bounded in-memory client queue, explicit event-count or byte thresholds, a maximum flush interval, and asynchronous non-blocking delivery.
- Bound retries with backoff and jitter. Drop telemetry when the queue is full; never block user actions, grow memory without limit, or recursively log delivery failure.
- Treat every severity as rate-limited. A critical label must not permit an event storm.
- Keep product analytics separate and generate authoritative security or audit truth on the backend.

### Client Tracing

- Trace only a critical user journey that meets `$engineering-observability`'s risk trigger; do not trace every render, effect, click, navigation, or request.
- Use supported automatic instrumentation plus targeted manual spans at meaningful journey and adapter boundaries.
- Propagate W3C trace context only to an explicit allowlist of first-party or trusted origins. Context propagation is not telemetry export.
- Export client span data only through the controlled backend trace endpoint. Never send it directly to an OpenTelemetry Collector, Better Stack, or another provider, and never ship exporter credentials to the client.
- Never attach auth data, form values, user-entered content, raw URLs or query strings, direct identity, or arbitrary baggage to client spans.
- Apply client-specific sampling, queue, payload, and privacy limits. Keep product analytics separate from operational traces.

## Testing

- Prefer `@testing-library/react` for web unit/component tests.
- For mobile unit/component tests, prefer the repo's existing Expo/Jest integration, commonly `expo-jest`, instead of introducing a parallel test stack without a strong reason.
- Web E2E tests are mandatory for important user-visible flows when a web app exists. Prefer Playwright.
- Mobile E2E tests are mandatory for important user-visible flows when a mobile app exists. Prefer Maestro.
- Test one-event-per-transition telemetry, deduplication or aggregation, production sampling and debug-event policy, queue bounds and drops, and the absence of PII or raw error objects.
- When client tracing changes, test origin-allowlisted context propagation, controlled-backend-only span export, client sampling and limits, meaningful span ownership, and the absence of sensitive attributes or baggage.
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
- Endpoint error parsing and mapping use the exact operation contract, remain exhaustive, and do not hide new variants behind a broad domain schema or status-first fallback.
- Hooks branch on `Result` values for domain outcomes instead of treating TanStack Query `isError` as the domain failure path.
- Hooks consumed by flows or layouts expose an application-owned exhaustive server-state union rather than independent query flags; background refresh and refresh failure preserve usable data, and every real lifecycle state has an explicit variant.
- Forms render plain user-facing strings and preserve all field and general error messages.
- Web unit/component tests prefer `@testing-library/react`.
- Mobile unit/component tests follow the repo's existing Expo/Jest integration unless there is a documented reason to diverge.
- User-visible states and flows that affect navigation, form submission, authentication, checkout or payment, destructive actions, or error recovery include accessibility coverage.
- Frontend operational events and spans occur only at owned boundaries or meaningful transitions, satisfy `$engineering-observability`, export only through the controlled backend, and cannot form render, retry, ingestion, or provider-failure loops.
- Supported-platform flows that cover core business actions, high-traffic journeys, or failure recovery include mandatory E2E coverage.
- If a platform or E2E tool is unsupported in the repo, document the limitation and prioritize accessibility plus unit/component and flow coverage on supported platforms.
- Before completion, verify every triggered check or record its omission and alternative assurance in the `$engineering-for-certainty` handoff.
