# Server State Hooks

Read when changing query hooks, loaders, prefetch, cache identity, or server-state mapping.

## Hooks and Server State

- Prefer TanStack Query as the default server-state/query library for web and mobile projects unless the repo already standardizes on another tool.
- Treat server data required for a route or screen's initial render as navigation-owned work when the repository's router or framework provides a loader, server-data, or route-prefetch mechanism. Use that mechanism instead of initiating the fetch from a component or flow effect.
- Integrate navigation-owned loading with the repository's existing query or cache layer through its supported dependency boundary, such as typed router context. Reuse the same query definition and cache identity in the loader or prefetch boundary and the consuming hook. Await route-critical data; start optional prefetching without blocking navigation when the framework supports it.
- If no suitable navigation-owned data-loading API exists, preserve the repository's established query-hook or flow boundary rather than introducing a new router architecture or a hand-rolled prefetch effect merely to imitate loader behavior.
- Hooks call API adapters and expose the adapter result without reclassifying expected domain failures as thrown exceptions.
- Hooks and flows receive the adapter's exact result type; do not widen it to a convenient global error union.
- When a query or mutation awaits a `ResultAsync`, the resolved value is a `Result`; domain failures land in query/mutation `data`, not in Query's `error` state. Branch on `data.isOk()` / `data.isErr()` for domain outcomes.
- Use TanStack Query `isError` / `error` only for unexpected thrown failures that violate the adapter contract.

### Queries

- When flows or layouts consume a server-state hook, map library-specific flags into an application-owned discriminated union. Do not make consumers infer state from independent combinations of `isPending`, `isError`, `isFetching`, `data`, and `error`.
- Preserve TanStack Query's discriminated result when mapping it. Use the full result or a deliberately distributive projection; do not apply a plain `Pick` that erases relationships among `status`, `fetchStatus`, `data`, and `error`.
- At minimum, distinguish `initial-loading`, terminal `initial-failure`, `ready`, `refreshing`, and `refresh-failure`. Refresh states retain usable data. Add explicit `idle`, initial-paused, and refresh-paused variants whenever disabled, lazy, or network-mode behavior makes them reachable instead of collapsing those states into loading.
- Reuse a generic application-owned mapper when multiple hooks share the same lifecycle. Domain hooks supply their exact data and failure types, map unexpected query failures, and retain the last usable value by query identity when expected refresh failures must preserve stale data.
- Require flows and layouts to match the resulting union exhaustively. Prefer the repository's existing exhaustive-matching mechanism; do not introduce a pattern-matching dependency only to copy this example.
- Keep query keys short, stable, and domain-specific.

For a query that may be idle or paused, a shared mapper can preserve the
boundary without widening domain failures or relying on a runtime-only
fallback:

```ts
import { match } from "ts-pattern";

type ServerState<TData, TFailure> =
  | { status: "idle" }
  | { status: "initial-loading" }
  | { status: "initial-paused" }
  | { status: "initial-failure"; failure: TFailure }
  | { status: "ready"; data: TData }
  | { status: "refreshing"; data: TData }
  | { status: "refresh-paused"; data: TData }
  | { status: "refresh-failure"; data: TData; failure: TFailure };

type QueryStateInput<TData, TFailure> = UseQueryResult<
  Result<TData, TFailure>,
  unknown
>;

type RetainedData<TData> =
  | { status: "absent" }
  | { status: "present"; data: TData };

type UnexpectedErrorQuery<TData, TFailure> = Extract<
  QueryStateInput<TData, TFailure>,
  { status: "error" }
>;

type SuccessfulQuery<TData, TFailure> = Extract<
  QueryStateInput<TData, TFailure>,
  { status: "success" }
>;

function mapUnexpectedQueryState<TData, TFailure>(
  query: UnexpectedErrorQuery<TData, TFailure>,
  retainedData: RetainedData<TData>,
  mapUnexpectedFailure: (error: unknown) => TFailure,
): ServerState<TData, TFailure> {
  const currentData: RetainedData<TData> = query.data?.isOk()
    ? { status: "present", data: query.data.value }
    : { status: "absent" };
  const usableData =
    currentData.status === "present" ? currentData : retainedData;
  const failure = mapUnexpectedFailure(query.error);

  return usableData.status === "present"
    ? { status: "refresh-failure", data: usableData.data, failure }
    : { status: "initial-failure", failure };
}

function mapSuccessfulQueryState<TData, TFailure>(
  query: SuccessfulQuery<TData, TFailure>,
  retainedData: RetainedData<TData>,
): ServerState<TData, TFailure> {
  const result = query.data;

  if (result.isErr()) {
    if (retainedData.status === "absent") {
      return { status: "initial-failure", failure: result.error };
    }

    return match(query.fetchStatus)
      .returnType<ServerState<TData, TFailure>>()
      .with("fetching", () => ({
        status: "refreshing",
        data: retainedData.data,
      }))
      .with("paused", () => ({
        status: "refresh-paused",
        data: retainedData.data,
      }))
      .with("idle", () => ({
        status: "refresh-failure",
        data: retainedData.data,
        failure: result.error,
      }))
      .exhaustive();
  }

  return match(query.fetchStatus)
    .returnType<ServerState<TData, TFailure>>()
    .with("fetching", () => ({
      status: "refreshing",
      data: result.value,
    }))
    .with("paused", () => ({
      status: "refresh-paused",
      data: result.value,
    }))
    .with("idle", () => ({ status: "ready", data: result.value }))
    .exhaustive();
}

function mapQueryToServerState<TData, TFailure>(
  query: QueryStateInput<TData, TFailure>,
  retainedData: RetainedData<TData>,
  mapUnexpectedFailure: (error: unknown) => TFailure,
): ServerState<TData, TFailure> {
  return match(query)
    .returnType<ServerState<TData, TFailure>>()
    .with({ status: "pending", fetchStatus: "idle" }, () => ({
      status: "idle",
    }))
    .with({ status: "pending", fetchStatus: "fetching" }, () => ({
      status: "initial-loading",
    }))
    .with({ status: "pending", fetchStatus: "paused" }, () => ({
      status: "initial-paused",
    }))
    .with({ status: "error" }, (failedQuery) =>
      mapUnexpectedQueryState(
        failedQuery,
        retainedData,
        mapUnexpectedFailure,
      ),
    )
    .with({ status: "success" }, (successfulQuery) =>
      mapSuccessfulQueryState(successfulQuery, retainedData),
    )
    .exhaustive();
}
```

Keep retained data keyed to the query identity so data from one query cannot appear in another. A domain hook returns `ServerState<Profile, ProfileLoadFailure>`; a flow or layout consumes it without importing TanStack Query flags:

```tsx
import { match } from "ts-pattern";

return match(profileState)
  .with({ status: "idle" }, () => <ProfileNotReady />)
  .with({ status: "initial-loading" }, () => <ProfileSkeleton />)
  .with({ status: "initial-paused" }, () => <ProfileLoadPaused />)
  .with({ status: "initial-failure" }, ({ failure }) => (
    <ProfileLoadError failure={failure} />
  ))
  .with({ status: "ready" }, ({ data }) => <ProfileView profile={data} />)
  .with({ status: "refreshing" }, ({ data }) => (
    <ProfileView profile={data} refreshing />
  ))
  .with({ status: "refresh-paused" }, ({ data }) => (
    <ProfileView profile={data} refreshPaused />
  ))
  .with({ status: "refresh-failure" }, ({ data, failure }) => (
    <ProfileView profile={data} refreshFailure={failure} />
  ))
  .exhaustive();
```
