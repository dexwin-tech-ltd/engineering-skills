# Effect Testing

Read for tests of Effect services, workflows, scopes, time, and schema properties. Apply core verification rules, including the behavior-based property-testing and mutation-analysis triggers.

## What Effect Covers

Prefer `@effect/vitest` in new Effect backend/web projects. Use its supported effect/scoped/layer test APIs for the locked version. Plain pure functions remain ordinary tests. Preserve existing mobile Expo/Jest and UI test stacks rather than introducing a parallel runner merely to use these helpers.

- Provide deterministic test services through Layers at real dependency seams; domain services run unchanged. Do not replace service logic with an expected answer or mock internal helpers.
- Use TestClock for sleep, timeout, backoff, and scheduling behavior. Coordinate fiber progress and advance virtual time explicitly; avoid arbitrary real sleeps and do not assume TestClock controls a driver's real network clock.
- Exercise typed expected failures, defects, and interruption distinctly. Test cleanup/finalizers and resource release, including cancellation during pending work.
- Use supported Effect Schema/Arbitrary/TestSchema tools where they improve schema or domain properties. Contract-derived generators, meaningful assertions, shrinking/replay, and bounded varied exploration remain required. Schema-valid generation alone does not establish business invariants.
- Isolate mutable test Layers, runtime/registry state, and caches per test or explicitly reset them. Share expensive immutable infrastructure only when isolation remains proved.

## What Remains

Effect test services do not prove a real database query, HTTP serialization, browser lifecycle, native runtime, or migration. Keep real HTTP/database integration and the isolated Migration Proof Harness; RTL for components/flows; Playwright/Maestro for supported user journeys; and Stryker or the selected mutation tool for triggered invariants. Keep runtime acceptance and independent review.

Choose assertions against observable contracts rather than Effect implementation details or framework helpers. Flaky tests require diagnosis; routine retry-until-green is not verification. A constrained retry for a known harness transient must remain visible and does not erase the original failure.

## Primary References

Consult the selected version's official documentation and installed declarations rather than assuming a cross-version API:

- [Effect API](https://effect.website/docs/v4/api/)
- [Effect Vitest source](https://github.com/Effect-TS/effect/tree/main/packages/vitest)
- [Drizzle Effect PostgreSQL](https://orm.drizzle.team/docs/connect-effect-postgres)
- [Drizzle Effect Schema](https://orm.drizzle.team/docs/effect-schema)
