# Frontend Validation

Read when implementing or reviewing meaningful frontend behavior, tests, accessibility proof, or supported-platform E2E coverage.

## Testing

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

1. Plan/gap review, including UI states, error states, accessibility states, and route/screen ownership.
2. Shared contracts and API adapter types.
3. API adapter tests for request validation, response validation, and error mapping.
4. Hook tests for query/mutation behavior and the selected Effect or Result outcome branches, including defects, interruption, and stale-attempt protection where reachable.
5. Flow tests for reducer transitions, orchestration, navigation, and submit outcomes.
6. View/component tests for rendering, permissions, validation, success states, and error states.
7. Implementation from API adapter inward to hook, flow, and view.
8. Accessibility checks for critical user-visible states and flows.
9. E2E coverage for important happy-path and failure-path user flows on supported platforms.

When the backend operation is not yet available, complete the sequence through
the mock adapter and record the production adapter plus live integration proof
as pending work. The frontend-first slice may be complete when its declared
frontend behavior and safeguards pass, but the feature is not end-to-end
complete until the production adapter is connected to the real backend and its
contract-boundary and live integration tests pass. Mock implementations may
remain afterward for development and testing.
