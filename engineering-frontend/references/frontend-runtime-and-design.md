# Frontend Runtime and Design Proof

Read for observable frontend issues or changes governed by an authoritative design.

## Runtime Acceptance And Design Conformance

For a task-level Quick change without a formal issue, use the focused direct
UI check from `$engineering-for-certainty` and inspect the final diff. A local
copy or styling change does not create a full issue-level scenario or design
matrix. Escalate if it changes interactions, accessibility behavior, shared
design tokens, multiple states, or an authoritative design contract whose
compliance cannot be established with a focused check.

For every frontend issue that changes observable runtime behaviour, follow
`$engineering-for-certainty`'s
[Runtime Acceptance Pass](../../engineering-for-certainty/references/runtime-acceptance.md).
Reuse current browser or device automation that proves the exact candidate and
required state; run uncovered states and exploratory checks directly.

- Use browser control to navigate a running web application through the same
  routes and controls a user uses. Use computer, emulator, or device control for
  native or operating-system-dependent interfaces.
- Exercise every accepted visible outcome, one complete primary journey, and a
  targeted exploratory check of the changed area and integration seams. Include
  keyboard, focus, labels, and assistive-technology evidence required by the
  issue.
- Check the browser console and failed network requests during each relevant web
  journey. A visually correct screen with an unexpected console failure or
  failed request does not pass without a named accepted explanation.
- Record platform and viewport, actions, visible outcome, console and network
  result, screenshots when visual proof matters, exact revision and environment,
  and cleanup without recording personal data or authentication secrets.
- A browser pass through a mock-backed running frontend may prove an explicitly
  frontend-only slice only when the issue names the mock adapter as a proxy and
  records the missing backend integration proof. It does not make the feature
  end-to-end complete.

When a formal issue's implementation is based on an authoritative design, read and follow
[Design Conformance And Audit](design-conformance.md). Before the
issue becomes implementation-ready, it requires an issue-owned Design Reference
Manifest, stable source signature, frozen design images, validated HTML/Tailwind
visual reference, platform-specific supplements when needed, and a complete
state-and-viewport Design Audit Matrix. Delivery compares the running frontend
against that baseline; independent code review re-fetches the source, detects
design drift, and audits the complete matrix.

Keep design conformance unverified when the authoritative source, approved
version, evidence bundle, required runtime state, matrix row, or independent
audit is missing, inaccessible, failed, or stale. A material implementation
mismatch fails the pass. A conflict with accepted behaviour, accessibility, the
design system, or platform conventions requires a decision instead of a silent
deviation.
