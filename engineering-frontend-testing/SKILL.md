---
name: engineering-frontend-testing
description: Explicit frontend testing and accessibility doctrine for work that touches web/mobile unit tests, accessibility quality bars, or mandatory E2E coverage.
---

# Engineering Frontend Testing

Use this companion skill with `$engineering-for-certainty` when work touches frontend test stack choices, accessibility requirements, or web/mobile E2E coverage.

## Scope

Apply this skill when the work includes:

- web component, flow, or hook testing
- mobile component, screen, or hook testing
- frontend accessibility requirements
- Playwright or Maestro test coverage
- frontend build-sequence changes related to testing

## Doctrine

- Prefer `@testing-library/react` for web unit/component tests.
- For mobile unit/component tests, prefer the repo's existing Expo/Jest integration, commonly `expo-jest`, instead of introducing a parallel test stack without a strong reason.
- Web E2E tests are mandatory for important user-visible flows when a web app exists. Prefer Playwright.
- Mobile E2E tests are mandatory for important user-visible flows when a mobile app exists. Prefer Maestro.
- Accessibility is a default quality bar. Use semantic roles/labels, keyboard or equivalent navigation support where applicable, and screen-reader-friendly loading, error, and success states.
- Add accessibility-aware assertions for critical user-visible flows when the local test stack supports them.

## Frontend Test Sequence

For web or mobile features:

1. API adapter tests for request/response validation and error mapping.
2. Hook tests for query/mutation behavior and status unions.
3. Flow tests for reducer transitions and orchestration.
4. View/component tests for rendering, permissions, success states, and error states.
5. Accessibility checks for critical user-visible states and flows.
6. E2E coverage for important happy-path and failure-path user flows on supported platforms.

## Review

- Web unit/component tests prefer `@testing-library/react`.
- Mobile unit/component tests follow the repo's existing Expo/Jest integration unless there is a documented reason to diverge.
- Critical user-visible states and flows include accessibility coverage.
- Important supported-platform user flows include mandatory E2E coverage.
