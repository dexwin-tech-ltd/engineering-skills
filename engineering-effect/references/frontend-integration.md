# Frontend Integration

Read for Effect-backed web/mobile queries, mutations, forms, runtime setup, and subscriptions. Apply `$engineering-frontend` for layer ownership, navigation, accessibility, and platform proof.

## Ownership

Use `contract -> adapter/service -> hook -> flow -> view`. Only adapters call network/SDK clients. Hooks own reactive integration, expose application-owned state and actions, and preserve exact producer failure types. Flows own orchestration and application/form state; views and presentational components receive plain values and callbacks and remain stateless. Simple flow-local React state is valid; Effect is not required for a text field or Boolean disclosure state.

Prefer Atom for server state in new Effect web projects. A project-owned application/provider module assembles its Layer-backed runtime and registry once. Use supported Atom React hooks in domain hooks. Keep Atom, AsyncResult, Cause, and library flags out of presentation contracts. React Native adoption requires the platform proof in the entrypoint.

Navigation loaders/prefetch use the same atom/query identity and registry as the consuming hook. Do not duplicate network calls or create a second cache. Keep identity domain-specific, stable, and partitioned by actor when data is actor-specific.

Share server-query caches deliberately; keep editable UI state in its owning flow/reducer. Give each independent flow its own mutation instance, including pending state, failure state, attempt settlement, and cancellation. One flow's teardown must not cancel another flow's work. Share an operation only when it is intentionally one coordinated operation with a named concurrency and lifetime owner.

Keep writable atoms and registry writes within the owning domain hook/integration module. Expose named actions and plain state to flows; views remain stateless. Avoid business orchestration through subscriptions that silently write other atoms. Keep transitions explicit so callers can trace an action through the effect and resulting query refresh. A shared registry does not itself establish write ownership.

## Queries

Expose a closed application-owned lifecycle: initial loading, initial failure, ready, refreshing, and refresh failure with retained usable data. Include idle, paused/offline, cancellation, or defect variants when reachable rather than disguising them as loading. Retain data by query identity, and clear/partition it on session changes. Translate typed failures into safe messages and defects into separate safe unexpected-failure state at the hook boundary.

## Operations

Provide reactive `state` plus `run(input)` returning the outcome of that exact attempt. Both derive from one owned operation instance. Define single-flight, latest-wins, or concurrent policy; late completion must not overwrite newer state, navigate after teardown, or apply another attempt's result. Disable or otherwise guard duplicate unsafe writes. Failed refresh after a successful mutation must not tell the user that the mutation failed.

An application-owned total outcome can have this shape:

```ts
type OperationOutcome<A, E> =
  | { status: 'ok'; value: A }
  | { status: 'err'; error: E }
  | { status: 'cancelled' }
  | { status: 'defect' };
```

`useEffectOperation` or an equivalent helper is a project-owned abstraction, not an assumed Effect built-in. Use the selected Atom/runtime API and scope cancellation; expose `cancel`/`reset` when the flow needs them. Do not swallow an unhandled rejected Promise from an event handler.

Flows exhaustively handle attempt results when coordinating further work:

```ts
import { Match } from 'effect';

Match.value(outcome).pipe(
  Match.when({ status: 'ok' }, () => setConfirmationOpen(true)),
  Match.when({ status: 'err' }, ({ error }) => showExpectedFailure(error)),
  Match.when({ status: 'cancelled' }, () => handleCancellation()),
  Match.when({ status: 'defect' }, () => showUnexpectedFailure()),
  Match.exhaustive,
);
```

Use the matched branch argument to access narrowed fields. Expected errors and raw causes do not go to views. A state mapper, message translation, and attempt settlement should be shared only where their policies actually agree.

## Forms and Promise Bridges

Retain TanStack Form. Its owner is the flow/hook, with controlled inputs and plain messages passed to presentation. Use the selected Effect Schema Standard Schema adapter after checking installed compatibility. Validation may preserve editable input values rather than return decoded transformations; explicitly decode on submission before side effects. Preserve all field and banner messages. Async business checks remain in services.

For TanStack Query fallback or another Promise consumer, execute an Effect at one owned bridge. Preserve expected failures as an explicit resolved outcome where the consumer contract promises that behavior; classify defects and interruption separately. Pass cancellation through the selected runtime. Do not await an Effect object expecting Promise execution. Avoid duplicate Atom and Query ownership for the same server state.

## Subscriptions

Atom can consume Effect Streams, but it does not automatically watch a remote database. A live backend subscription needs an explicit transport such as SSE, WebSocket, or RPC; validate received messages at the adapter. Scope subscription cleanup to its owner, and define bounded buffering, reconnect, missed-event recovery/resnapshot, and failure states. Publish committed state only. In-memory PubSub is neither durable nor a multi-process event bus.

## Verification

Prove query identity/prefetch reuse; typed failures versus defects/interruption; attempt isolation; independent-flow mutation state and cancellation isolation; unmount/session cleanup; retained refresh data; mutation success despite refresh failure; form decoding and all messages; and any supported connectivity or persistence behavior. Keep RTL, Expo/Jest, Playwright, and Maestro for the boundaries they prove.
