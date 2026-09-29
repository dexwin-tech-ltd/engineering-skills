---
name: engineering-frontend
description: Frontend engineering doctrine for web/mobile architecture, API integration, forms, accessibility, design conformance, operational telemetry and tracing, runtime acceptance, and testing. Use with engineering-for-certainty when work touches frontend modules, routes/screens, API adapters, hooks, flows/views, TanStack Query/Form, accessibility, design-backed UI, client telemetry, or web/mobile unit/E2E coverage.
---

# Engineering Frontend

Use this companion skill with `$engineering-for-certainty` when work touches frontend architecture, API consumption, forms, accessibility, or web/mobile tests. Preserve repo conventions first; these rules define the default shape when the repo does not already have a stronger local convention.

## Always-Loaded Contract

Preserve the repository's existing frontend architecture, libraries, and test stack. Keep route/screen, flow, view, hook, adapter, and test boundaries explicit; validate external data before it reaches UI or domain code; preserve accessibility of changed user-visible states; and cover important supported-platform flows. When priorities conflict, preserve correctness and accessibility before broader E2E coverage.

For a task-level Quick change, use a focused direct UI check and inspect the final diff. Escalate when the change affects interaction, accessibility behavior, shared design tokens, multiple states, or an authoritative design contract that a focused check cannot establish. Formal observable-runtime issues follow the Runtime Acceptance contract.

## Open the Relevant Detail

Read only the references whose triggers apply. If the affected behavior or coupling is uncertain, open the plausible reference before deciding that it does not apply.

- **Routes, screens, flows, views, components, reducers, handlers, or navigation-owned loading:** [Frontend Architecture](references/frontend-architecture.md).
- **API adapters, input/output validation, transport and domain error mapping:** [Frontend API Integration](references/api-integration.md).
- **Frontend work before its backend exists:** [Frontend-First API Mocks](references/frontend-first-api-mocks.md). Only backend-calling adapter implementations are mocked; consuming layers retain production contracts.
- **Material UI in a feature that also needs backend integration:** [UI-First Review](references/ui-first-review.md). Prepare the interactive frontend stage first, plan named scenarios, review it on staging, and keep the later real-backend gate explicit.
- **Query hooks, cache identity, loader/prefetch integration, or server-state unions:** [Server State Hooks](references/server-state-hooks.md).
- **TanStack mutation hooks or attempt-specific orchestration:** [Result Mutation Hooks](references/result-mutation-hooks.md). Expected domain failures resolve as `Result`; every hook exposes reactive `state` and attempt-specific `run` from one mutation instance.
- **Forms or accessibility behavior:** [Forms and Accessibility](references/forms-and-accessibility.md). Validate with schemas, render plain user-facing messages, preserve every field and general error, and keep changed UI states accessible. Also read [Frontend Validation](references/frontend-validation.md) for affected critical flows.
- **Observable frontend issue or authoritative design:** [Frontend Runtime and Design Proof](references/frontend-runtime-and-design.md). For an authoritative design, also read [Design Conformance and Audit](references/design-conformance.md).
- **Operational client events or spans:** [Client Observability](references/client-observability.md) and `$engineering-observability`.
- **Meaningful feature behavior, unit/component tests, or E2E coverage:** [Frontend Validation](references/frontend-validation.md).
- **Independent review of frontend work:** [Frontend Review Checks](references/frontend-review-checks.md), under `$code-review`'s evidence and independence rules.

## Completion Self-Check

Use `$engineering-for-certainty`'s completion self-check. Confirm affected UI states, accessibility, contract boundaries, and direct runtime evidence at the selected rigor level. Load the specialist references that apply to the change; a small styling edit does not need every frontend reference.
