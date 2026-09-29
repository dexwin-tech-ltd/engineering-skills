# Runtime Acceptance Planning

#### Runtime Acceptance Plan

Every issue that changes observable runtime behaviour must define a Runtime
Acceptance Plan using `$engineering-for-certainty`'s
[Runtime Acceptance Pass](../../engineering-for-certainty/references/runtime-acceptance.md).
A documentation-only or other non-runtime issue may record `Not applicable`
with a concrete reason.

The plan must:

- map every accepted externally observable criterion and materially distinct
  outcome to a stable runtime scenario;
- include one complete primary journey and a targeted exploratory check of the
  changed area and its integration seams;
- name the local runtime, startup command, safe test data, real external
  boundary, exact actions or requests, expected outcomes, cleanup, and evidence;
- require local proof against the final combined candidate, reusing current
  automated real-boundary results where they cover the exact scenario;
- require preview or staging proof when the exact candidate can be safely
  deployed before merge and deployment is automated or separately authorized;
- record post-merge-only staging proof as a mandatory downstream release gate
  with its owner and trigger;
- identify every necessary proxy, what it proves, its blind spot, and the later
  literal gate when that blind spot is material;
- define a secret-free scenario ledger tied to the exact revision and
  environment, plus which later changes invalidate and require each scenario to
  run again;
- include a Test Identity Plan and Test Message Sink or inbox rules from
  `$engineering-auth-security` when authentication is exercised; and
- when the frontend is design-backed, read `$engineering-frontend`'s
  [Design Conformance And
  Audit](../../engineering-frontend/references/design-conformance.md), create its
  issue-ready Design Reference Manifest and Evidence Bundle, and include the
  exact source, baseline, states, platforms, viewports, interactions, matrix,
  source-drift method, and approved deviations in the plan.

For a feature with material UI and backend integration, read
[`$engineering-frontend`'s UI-First
Review](../../engineering-frontend/references/ui-first-review.md). Plan the
feature-specific scenario names, relevant data boundaries and transitions,
shareable review links, selector behavior, staging revision evidence, and the
user's UI approval gate. Name the frontend-only proxy, its integration blind
spot, and the later real-backend scenarios. Record the evidence for whether a
reviewable unmerged candidate can reach staging, including an authorized manual
deployment, or staging receives only merged work. Record how the unfinished
production surface remains inaccessible through a default-off trusted gate;
if such a gate is unavailable, plan to defer production deployment of the
UI-only code.
Make the integration stage depend on recorded user approval of the exact
frontend staging revision. Do not mark the full feature complete at the
mock-backed stage.
