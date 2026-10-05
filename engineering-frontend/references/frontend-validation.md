# Frontend Validation

Read when implementing or reviewing meaningful frontend behavior, tests, accessibility proof, or supported-platform E2E coverage.

## Testing

Apply the core [Testing Doctrine](../../engineering-for-certainty/references/verification-details.md#testing-doctrine).
Choose coverage by behavior and risk; test placement does not require a
separate suite for every adapter, hook, flow, view, or component. Keep relevant
production collaborators running inside the behavior under test, and preserve
the required journey and integration gates below.

- Prefer `@testing-library/react` for web unit/component tests.
- For mobile unit/component tests, prefer the repo's existing Expo/Jest integration, commonly `expo-jest`, instead of introducing a parallel test stack without a strong reason.
- Web E2E tests are mandatory for important user-visible flows when a web app exists. Prefer Playwright.
- Mobile E2E tests are mandatory for important user-visible flows when a mobile app exists. Prefer Maestro.
- Test one-event-per-transition telemetry, deduplication or aggregation, production sampling and debug-event policy, queue bounds and drops, and the absence of PII or raw error objects.
- When client tracing changes, test origin-allowlisted context propagation, controlled-backend-only span export, client sampling and limits, meaningful span ownership, and the absence of sensitive attributes or baggage.
- For frontend-first work, test production hooks, flows, views, components, accessibility behavior, and E2E journeys through the mock adapter; do not replace those layers with hook, query-result, flow, or component mocks.
- Verify that mock adapter scenarios satisfy the exact production operation types and runtime contracts and cover every UI-relevant outcome without impossible states or random defaults.
- Mirror production file naming. Follow the repo's local test declaration and
  naming style. If none exists, use `test()` with multiline given/when/then test
  names.

## Build Sequence

For web or mobile features:

1. Plan/gap review, including UI states, error states, accessibility states, route/screen ownership, and the distinct risks each test boundary must expose.
2. Establish shared contracts and API adapter types.
3. Select focused and assembled tests for the affected behavior. Cover applicable request/response validation and error mapping; query/mutation outcomes, defects, interruption, and stale-attempt protection where reachable; reducer transitions, orchestration, navigation, submission, rendering, permissions, and validation. Place assertions at boundaries that can expose those failures instead of creating a suite per layer.
4. Implement from API adapter inward to hook, flow, and view, applying core test-first requirements. Add focused tests where meaningful local logic or fault isolation warrants them, and exercise real collaboration where wiring matters.
5. Run accessibility checks for critical user-visible states and flows.
6. Complete required E2E coverage for important happy-path and failure-path user flows on supported platforms.

For example, a form/flow test can cover a forwarding hook and its visible
outcomes together. A hook that owns stale-response protection can warrant its
own focused tests. A flow test through a mock adapter does not prove production
request serialization or live backend integration.

When the backend operation is not yet available, complete the sequence through
the mock adapter and record the production adapter plus live integration proof
as pending work. The frontend-first slice may be complete when its declared
frontend behavior and safeguards pass, but the feature is not end-to-end
complete until the production adapter is connected to the real backend and its
contract-boundary and live integration tests pass. Mock implementations may
remain afterward for development and testing.
