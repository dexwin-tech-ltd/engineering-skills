---
name: issue-review
description: Review an existing issue, ticket, feature file, bug report, roadmap item, or implementation handoff for agent readiness. Use when Codex is asked to tighten, validate, rewrite, prepare, or assess an issue so another agent or engineer can implement it with zero clarifying questions.
---

# Issue Review

Review the issue file against the bar: a competent implementation agent should be able to complete it with zero clarifying questions and produce a robust, validated change. If ambiguity remains, the issue is not ready.

Use the user's engineering-for-certainty doctrine as the default engineering standard when reviewing software issues: preserve repo conventions, validate trust boundaries, keep adapters thin and domain logic explicit, prefer explicit expected-failure contracts, and require tests for critical behavior and failure paths.

Require companion engineering doctrine when the issue touches its area:

- Observability: logging, metrics, tracing, audit records, correlation IDs, telemetry, redaction, or frontend log ingestion.
- Resilience: external calls, retries, timeouts, idempotency, concurrency, queues, cron jobs, webhooks, background jobs, or async processing.
- Auth/security: cookies, sessions, CSRF, token handling, actor context, protected routes, permission checks, policy registries, secrets, or authorization boundaries.
- Frontend testing/accessibility: web/mobile test-stack decisions, E2E coverage, accessibility assertions, keyboard behavior, focus management, screen-reader behavior, or platform-specific UI testing.

If the companion skill is available in the Codex session, use it. If it is not available, apply the local repo's equivalent docs or explicitly record the gap in the issue review.

If no issue path is provided, ask for one before proceeding.

## Workflow

1. **Read the issue**: identify the requested change, claimed files, dependencies, and current structure.
2. **Discover repo conventions**: inspect local docs and examples before applying generic rules.
3. **Verify claims against code**: check paths, symbols, line references, tests, schemas, commands, and stated behavior.
4. **Review gates one at a time**: ask about each gap separately, provide a recommended answer, and wait for confirmation before continuing.
5. **Accumulate answers**: do not edit the issue during the review.
6. **Cross-validate**: check all resolved answers for contradictions and missing dependencies.
7. **Write once**: rewrite the issue only after every gate passes and the full picture is consistent.

Never write placeholders, TODOs, partial decisions, or "TBD" sections to the issue. The file is either unchanged while review is in progress or fully resolved when review is complete.

## Convention Discovery

Before gate review, inspect only the relevant local sources:

- Repo guidance: `AGENTS.md`, `CONTEXT.md`, `CONTEXT-MAP.md`, `DESIGN.md`, `README.md`, `PRODUCT_DECISIONS.md`.
- Planning surfaces: `ROADMAP.md`, `ROADMAP.DONE.md`, `TODO.md`, `docs/`, `_features/`, `features/`, `issues/`.
- Similar completed or active issue files.
- Existing tests near the affected code.
- Schema/type files named by the issue or implied by the affected area.
- Architecture or engineering doctrine docs, including `engineering-for-certainty`-style local guidance when present.

Prefer the repo's current issue format and testing style over this skill's fallback structure. If the repo has multiple contexts, use `CONTEXT-MAP.md` or nearby context docs to select the right context before reviewing terminology.

If the issue uses a domain term that conflicts with the local glossary or product docs, stop and ask the user to resolve the term before continuing.

## Claim Verification

Report mismatches before gate review:

- File paths that are missing, renamed, or too vague.
- Line numbers that drift by 3 or more lines.
- Referenced functions, classes, types, constants, routes, tables, commands, events, or config keys that do not exist where claimed.
- Behavior claims contradicted by current code or tests.
- Dependency claims contradicted by roadmap, issue files, or completed-work archives.

Do not proceed past a mismatch until the correct reference is confirmed or the issue is updated in the accumulated answers.

## Universal Gates

Check every issue against these gates.

### 1. Problem Or Motivation

The problem must be concrete.

- Bugs: state actual behavior, expected behavior, and reproduction scenario.
- Features: state missing capability, user or system value, and why now.
- Chores/refactors: state the risk, constraint, or future work unlocked.

Vague phrases like "improve this", "fix logic", or "clean up" do not pass.

### 2. Affected Surface

The issue must name the affected files, modules, routes, commands, tables, schemas, APIs, UI flows, or docs. Include line numbers when the relevant area is narrow. A whole-file reference is acceptable only when the whole file is intentionally in scope.

### 3. Root Cause Or Background

For bugs, identify the root cause rather than only the symptom. For features and chores, include enough background for an agent to make local judgment calls without re-litigating the product decision.

### 4. Acceptance Criteria

Every criterion must be testable or inspectable. It should describe an observable outcome, not an implementation wish.

Good: "`getApprovedQuestions` excludes draft and rejected records."
Bad: "question filtering works correctly."

When a criterion uses universal or negative language ("only", "all", "every", "never", "no other"), verify it cannot be satisfied by a weaker existential check. Either the issue enumerates the exact elements the claim covers and states that each must independently hold, or it says explicitly that the check is a spot-check and why that's acceptable. A criterion like "the file contains only placeholder values" is otherwise easy to implement as "at least one placeholder is present" — a check that passes even when most of the listed values are real.

### 5. Scope Boundary

State what is explicitly not changing. This prevents adjacent refactors, UX expansion, schema churn, or product decisions from leaking into the task.

### 6. Implementation Change Control

The issue must state what judgment the implementation agent may exercise without
asking, and what discoveries require pausing.

Include:

- Allowed mechanical adjustments, such as import fixes, local naming alignment,
  adapting to existing helper APIs, formatting, or adding focused test fixtures.
- Pause triggers, including new migrations, schema changes, public contract
  changes, auth or permission changes, new dependencies, cross-domain refactors,
  behavior drift, test-strategy changes, or affected files outside the named
  surface.
- The canonical surface to update if the plan changes, such as this issue file,
  `ROADMAP.md`, an ADR, or a feature file.
- Any known uncertainty that should be resolved before implementation rather
  than discovered midway.

### 7. Dependencies

Name upstream blockers, downstream dependents, and ordering constraints. Check roadmap and nearby issue files for implied dependencies the issue forgot to mention.

If this issue changes or extends a rule, convention, or domain concept that another issue file explicitly claims to inherit, match, or reuse (search sibling issue files for phrases like "match issue #N", "per issue #N", "same as #N", "inherits from #N"), or that is documented in `CONTEXT.md`/`CONTEXT-MAP.md`, open every referencing file now. Either reconcile them in this same review pass, or record an explicit follow-up issue to do so before this issue is marked ready — never leave a referencing issue or the glossary silently stale.

### 8. Test Approach

Name the test file or test layer. Include at least one concrete scenario in the repo's local test style. Critical behavior changes must include tests for success, expected failures, validation failures, and error mapping where those cases apply.

When an acceptance criterion names a specific runtime mechanism (a "scheduled" job, a "background" retry, an "on reconnect" handler), the Test Approach must state whether verification exercises that literal mechanism or a named, justified proxy (e.g. a manual one-shot invocation of the same script the scheduler calls). An unstated substitution leaves a criterion looking tested when only an adjacent code path was actually exercised.

If no local convention is visible, use:

```ts
it(`
  given <context>
  when  <action>
  then  <assertion>
`, () => {
  // ...
})
```

### 9. Operational And Migration Safety

If the issue changes persisted data, generated artifacts, imports, migrations, deployment config, background jobs, or external integrations, state how the change is applied, rolled back or retried, and verified.

### 10. Observability And Errors

If the issue changes runtime behavior, state expected error behavior and any logging, metrics, audit events, or user-visible messages needed by the repo's conventions. If none are needed, say why.

### 11. Engineering Certainty

The issue must make the robust implementation path clear:

- Identify the trust boundary and validation schema for every external input.
- Keep route/page/controller/adaptor work thin; name where domain logic belongs.
- State the expected-failure contract used by the repo, such as `Result<T, E>`, error unions, or the local equivalent.
- Require exhaustive handling for discriminated states, enums, result variants, or UI status unions.
- State whether config, auth context, clocks, clients, or persistence are injected or otherwise supplied through the repo's existing pattern.
- For structural refactors, require behavior-preservation tests and name what must not drift.
- Identify any companion doctrine triggered by the issue and include its requirements in the issue.

## Conditional Gates

Apply only when the issue scope triggers them. Use repo-specific docs and existing patterns to decide whether each gate applies.

- **Auth and permissions**: identify the server-side authorization boundary; client-only checks never suffice.
- **Observability**: require sanitized logs/metrics/audit events, correlation/request context, and verification when the repo expects them.
- **Resilience**: require timeout, retry/backoff, idempotency, concurrency, and recovery behavior where relevant.
- **External data boundary**: name the validator/parser/schema used before raw data reaches domain logic; prefer `.safeParse()` or the repo's equivalent boundary API.
- **Database writes or concurrent writes**: state uniqueness constraints, idempotency, race handling, and deletion policy.
- **Destructive test operations against shared-tooling infrastructure**: when the Test Approach includes operations that delete or reset state (volume/database teardown, `down -v`-style resets, bucket/queue purges) against infrastructure whose tooling (compose project name, database name, bucket name, queue name, etc.) could also be used to run a real or production instance, require the issue to name how the test instance is isolated (a distinct project name, prefix, or environment label) so its teardown can never reach a real instance's resources.
- **Event or audit emission**: name event types and payload constraints; verify schema files if the repo has them.
- **Shell execution**: state the approved command wrapper or argument-safety pattern.
- **Outbound HTTP or third-party APIs**: require timeout, retry policy where appropriate, and named error mapping.
- **Discriminated unions or enums**: name every exhaustive handling site that must change.
- **Result/error contracts**: follow the repo's expected-failure style and do not introduce throw-based expected failures or conflicting patterns silently.
- **Frontend behavior**: include states, accessibility expectations, responsive behavior, and the user flow that proves the change.
- **Generated code or fixtures**: state regeneration commands and which generated files should or should not be edited by hand.
- **Security or privacy**: state secret handling, PII exposure, data retention, and permission implications.

If the repo has domain-specific gates, apply them after discovery. Examples include approved-only content, event schema invariants, tenant boundaries, import provenance, or feature-flag rules.

## Questioning Discipline

Ask one question at a time. For each question:

1. State the gate or contradiction.
2. Explain the concrete ambiguity or risk.
3. Give the recommended answer.
4. Wait for user confirmation before moving on.

If code or docs can answer the question, inspect them instead of asking the user.

## Cross-Validation

Before editing, verify:

- Acceptance criteria match what the affected surface can produce.
- Every acceptance criterion has a corresponding test or verification step.
- Scope boundaries do not contradict acceptance criteria.
- Dependencies are complete and ordered.
- Implementation change-control rules distinguish mechanical adjustments from
  material discoveries that require user approval.
- Auth, schema, event, import, migration, and error-handling choices match repo conventions.
- Trust boundaries, domain ownership, expected-failure contracts, and test coverage meet engineering-for-certainty expectations or document a concrete exception.
- Companion engineering doctrines were applied for observability, resilience, auth/security, and frontend testing/accessibility when those areas were in scope.
- File paths, line numbers, and symbol names still match after verification.
- Any rule or concept this issue changes has been checked against every issue file or glossary entry that claims to inherit it (gate 7); each is either reconciled or has a tracked follow-up.

Resolve every contradiction with the user before writing.

## Rewrite

Preserve the repo's established issue format when one exists. If no format exists, use this fallback:

```md
# <title>

Status: open
Type: Bug | Feature | Chore | Exploration
Severity: High | Medium | Low | Very Low

## Problem / Motivation

## Root Cause / Background

## Affected Surface

## Acceptance Criteria

## Out of Scope

## Implementation Guardrails

## Dependencies

## Test Approach

## Notes
```

Omit sections that genuinely do not apply. Do not add empty sections.
