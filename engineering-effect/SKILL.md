---
name: engineering-effect
description: Effect integration doctrine for TypeScript services, adapters, explicit failure contracts, Schema, Match, runtime and Layer ownership, HTTP/SQL, Atom, and testing. Use with engineering-for-certainty in Effect projects; preserve existing stacks unless migration is separately chosen.
---

# Engineering Effect

Use with `$engineering-for-certainty` and the behavior-specific frontend, resilience, security, and observability companions. Prefer Effect for services, adapters, and orchestration in new TypeScript backend, web, and mobile projects. Keep straightforward pure functions in TypeScript. Existing-project migration is a separate decision.

## Compatibility and Adoption

- Select a compatible set of Effect, runtime, React/Atom, database, and test packages. Lock exact versions and retain the lockfile. Do not mix examples or imports from different major versions.
- Selected unstable modules are allowed with an owned integration boundary, supported-platform proof, and explicit reviewed upgrades. Do not infer ecosystem compatibility from core Effect's release status.
- Check installed types and current primary documentation. Prove HTTP startup/shutdown, SQL transactions/cleanup, schema/form integration, and frontend lifecycle behavior relevant to the project.
- For React Native, prove the selected Atom integration on the supported Expo/native runtime, including connectivity, app background/foreground behavior, cleanup, and persistence when required. Use TanStack Query as the fallback if that proof fails; do not label web proof as native proof.

## Explicit Operation Contracts

Public services, repositories, and adapters expose named success and producer-owned failure types and explicit `Effect.Effect<Success, ExactExpectedFailure, RequiredServices>` signatures. The third parameter identifies services the caller must provide; it is not a runtime argument or an error union.

```ts
import { Context, Effect } from 'effect';

type UploadPhotoFailure = UploadUnavailable | PhotoRejected;
interface PhotoUploadsApi {
  readonly upload: (photo: Photo) => Effect.Effect<UploadedPhoto, UploadPhotoFailure>;
}
class PhotoUploads extends Context.Service<PhotoUploads, PhotoUploadsApi>()('PhotoUploads') {}

function uploadChosenPhoto(photo: Photo): Effect.Effect<
  UploadedPhoto, UploadPhotoFailure, PhotoUploads
> {
  return Effect.gen(function* () {
    const uploads = yield* PhotoUploads;
    return yield* uploads.upload(photo);
  });
}
```

The example uses Effect v4's service API; adapt to the project's locked major. Each failure variant owns its exact details. Reuse atomic variants, but do not widen operations to a project-wide error union. Effects with no expected failure use `never`; do not invent defensive domain failures. `Effect.gen`, named `flatMap`/`map` pipelines, and small ordinary functions are valid; choose readable dependency and ordering visibility.

## Failures, Defects, and Interruption

- Expected failures travel through the typed failure channel. Map known transport/SQL/provider failures at the owning adapter, preserving producer truthfulness.
- A defect is unexpected execution failure; interruption is cancellation of work. Neither silently becomes an expected domain error. A typed failure thrown as a JavaScript exception is still a defect.
- Handle all reachable outcomes at designated HTTP, React, job, CLI, or third-party execution boundaries. At a total UI boundary an application-owned attempt outcome may distinguish `ok`, `err`, `cancelled`, and `defect`; only `err` contains the exact expected-failure type. Expose safe presentation state instead of raw Cause/Exit to views.
- An Exit failure can contain multiple causes, including typed failures, defects, and interruption. Define precedence deliberately; do not extract the first typed failure and discard a coexisting defect or cancellation. Verify the chosen policy with mixed-cause tests when concurrency makes them reachable.
- Do not wrap internal Effects in neverthrow. Preserve existing Result/Promise/external APIs through an explicit boundary bridge when compatibility requires it.
- Prefer Effect Match for closed unions in new Effect projects, ending with `Match.exhaustive` or an equivalent compile-time exhaustiveness check. Preserve established ts-pattern until migration is chosen. Do not use catch-all branches to hide newly added variants.
- Reducers must always match their action union exhaustively. In Effect projects use Effect Match ending with `Match.exhaustive`; do not replace this with a switch, fallback branch, or default return of the previous state. Adding an action must require implementing its transition.

## Services, Layers, and Lifetimes

- Context services declare dependencies; Layers construct live implementations. Assemble them in application/bootstrap modules rather than constructing infrastructure inside domain logic.
- Backend startup owns shared configuration, connection pools, and telemetry. Resolve identity at the request boundary and pass the actor explicitly into identity-sensitive domain services by default. A request-scoped actor service is appropriate within HTTP middleware/handlers; domain operations should not retrieve identity from ambient context. Preserve a stronger established convention or document a concrete reason for an exception. Never install a request actor in a process-wide Layer. Frontend application/provider setup owns its runtime and registry; never recreate them per render or share actor state across SSR requests.
- Nest scopes for sessions, operations, and subscriptions. Acquire resources with finalizers; interrupt and join owned work on teardown. Avoid unowned daemon fibers for screen/session work.
- On logout or actor change cancel session work, reset or partition sensitive atom/cache state, and release session resources. A stable application runtime need not imply a globally shared actor.
- Compose Effects in domain code. Execute them only at designated framework, startup, event, test, or interop boundaries using the selected runtime. Do not scatter `runPromise` through helpers or start a second runtime for every operation.
- Use supported cancellation/AbortSignal bridges; verify driver/SDK behavior. Interruption does not guarantee that a remote mutation rolled back. Apply `$engineering-resilience` for timeout, retry, idempotency, and transaction safety.

## HTTP, Schema, and Persistence

- Prefer the Effect HTTP stack for new Effect backends, including typed API contracts where useful. A concrete plugin/hosting/integration requirement can justify Fastify; keep the bridge small and reviewed.
- Define endpoint-specific request, success, and expected error schemas in domain-owned shared contracts. Decode external input before domain effects; validate/encode outgoing responses. Keep API, domain, and `Db` shapes explicit when they differ.
- Prefer Effect Schema for forms, configuration, storage, adapters, and API contracts. Schemas validate/transform cohesive values; live-state permissions and business policy stay in services. Present all safe field/banner messages instead of raw issues.
- Retain Drizzle ORM and Kit for schema-aware queries and generated migrations. Prefer a verified native Effect driver where supported. Direct Effect SQL is allowed where useful; a generic declared row type does not validate SQL against the database schema. Decode persisted rows at the repository boundary.
- Services own transactions. Keep Drizzle and direct SQL participating in the same verified connection/transaction context; do not assume separately acquired clients share a transaction. Prove commit, rollback, and cleanup behavior. Preserve the isolated Migration Proof Harness and the production migration mechanism.
- Drizzle-generated Effect schemas can support persistence validation; do not automatically expose database columns or insert/update shapes as public API contracts.

## Conditional Detail

- **React/React Native queries, mutations, hooks, forms, or subscriptions:** [Frontend Integration](references/frontend-integration.md), with `$engineering-frontend`.
- **Service/workflow tests, test Layers, virtual time, schema properties, or integration proof:** [Testing](references/testing.md), with the core verification contract.

## Completion

Apply the core rigor and independent-review gates. Report the locked stack, integration boundaries, proof and platform limitations, and any explicitly chosen fallback. Effect replaces selected implementation machinery; it does not waive security, resilience, observability, accessibility, migration, or runtime-acceptance requirements.
