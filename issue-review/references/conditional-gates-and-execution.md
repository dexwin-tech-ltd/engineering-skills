# Conditional Gates and Execution

## Conditional Gates

Apply only when the issue scope triggers them. Use repo-specific docs and existing patterns to decide whether each gate applies.

- **Auth and permissions**: identify the server-side authorization boundary; client-only checks never suffice.
- **Observability**: require typed Safe Log Events, source-specific allowlists, correlation/request context, privacy and log-injection tests, retention and reader access, and verification. Apply `$engineering-resilience` when telemetry uses queues, retries, timeouts, circuit breakers, or an external sink.
- **Resilience**: require timeout, retry/backoff, idempotency, concurrency, and recovery behavior where relevant.
- **External data boundary**: name the validator/parser/schema used before raw data reaches domain logic; prefer `.safeParse()` or the repo's equivalent boundary API.
- **Database writes or concurrent writes**: state uniqueness constraints, idempotency, race handling, and deletion policy.
- **Destructive test operations against shared-tooling infrastructure**: when the Test Approach includes operations that delete or reset state (volume/database teardown, `down -v`-style resets, bucket/queue purges) against infrastructure whose tooling (compose project name, database name, bucket name, queue name, etc.) could also be used to run a real or production instance, require the issue to name how the test instance is isolated (a distinct project name, prefix, or environment label) so its teardown can never reach a real instance's resources.
- **Event or audit emission**: name event types and payload constraints; verify schema files if the repo has them.
- **Shell execution**: state the approved command wrapper or argument-safety pattern.
- **Outbound HTTP or third-party APIs**: require timeout, retry policy where appropriate, and named error mapping.
- **Discriminated unions or enums**: name every exhaustive handling site that must change; for coded errors, apply the code-to-details matrix and envelope tests from gate 10.
- **Result/error contracts**: follow the repo's expected-failure style and do not introduce throw-based expected failures or conflicting patterns silently.
- **Frontend behavior**: include states, accessibility expectations, responsive behavior, and the user flow that proves the change. Name and verify the literal platform or library mechanism for imperative querying, navigation interception, subscriptions, focus restoration, or similar behavior. For design-backed work, require the issue-ready baseline, Evidence Bundle, Design Audit Matrix, drift check, comparison plan, and typed outcomes from `$engineering-frontend`. When operational logging is present, name where and when each event emits and prove it cannot fire per render, unbounded retry, or expected domain outcome.
- **Async frontend mutations**: for every in-flight state, require a transition table with rows for each mutable control and for success, failure, retry, discard, and navigation. Each row must state whether the action is allowed, which snapshot owns the pending data, the next state, and the user-visible result. Cover edits made while a request is pending, stale or superseded responses, retry ownership, discard semantics, and navigation away/back. Every allowed transition and prohibited action must map to an exact test in the traceability ledger.
- **Generated code or fixtures**: state regeneration commands and which generated files should or should not be edited by hand.
- **Security or privacy**: state secret handling, PII exposure, data retention, and permission implications.
- **Pinned third-party tool or version-dependent defaults**: when the design relies on a tool, image, framework, or library default, verify the assumption against the exact pinned version using its source, release notes, or changelog rather than current-version knowledge. If the behavior can change silently on upgrade, require the controlling flag, environment variable, or config value to be set explicitly.

### Execution, Checkpoint Reviews, And Final Review

For non-trivial issues, include an Execution Plan that identifies which
workstreams are independent and which are ordered. Use parallel Implementation
Workers only when their production ownership and traceability rows do not
overlap materially and each writer has filesystem isolation plus a named
integration path. A shared implementation worktree has at most one active
writer. Independent read-only investigation and review may run in parallel when
their workstreams are genuinely separate.

When the issue defines review checkpoints, every checkpoint must be a coherent,
green, behavior-complete state with owned acceptance criteria, exact validation,
a frozen review head, and an explicit advance condition. Checkpoint reviews
reduce the amount of new code assessed at once; they do not create separate
issues, branches, or pull requests.

- Give each Implementation Worker its approved behaviour, bounded files or
  symbols, acceptance criteria, required validation, allowed-change boundary,
  escalation conditions, and required return evidence: changed files,
  validation outcomes, failures, deviations, and residual risk.
- Give every Branch Contract its own dedicated linked worktree by default,
  including the Canonical Integration Branch and each Helper Branch. Concurrent
  writes require that filesystem isolation. Record the repository convention or
  user's explicit direction when implementation will use the shared checkout.
  A clean review context does not itself require a worktree.
- Name one Canonical Integration Branch from the issue's Branch Contract. The Execution Plan must name how each Helper Branch enters it: cherry-pick coherent commits, merge the branch, or rebase and fast-forward according to repository history conventions. Never copy files between worktrees as the integration mechanism.
- Validate each helper branch, then integrate in dependency order. Resolve conflicts only on the canonical branch and re-run every affected traceability row.
- Run the complete triggered validation and issue-against-diff audit on the combined canonical branch.

After all checkpoints are accepted, run the complete triggered validation,
issue-against-diff audit, and final `$code-review` against the combined
issue-base-to-current-head diff. Revisit interactions across checkpoints,
shared contracts, configuration, deleted behavior, and integration seams.
Checkpoint evidence supports this review but cannot replace it.

Make final code review the last implementation gate. For multi-slice,
medium-risk, or high-risk changes, require multiple fresh Independent Reviewer
contexts when available: independent finder passes using `$code-review` and a
separate skeptical verifier that receives raw evidence without the finder's
expected verdict. For tiny low-risk changes, allow one Independent Reviewer
with a separate skeptical pass inside that fresh review context. If no fresh
review context is available, use deliberately separated self-review only as
supplemental evidence, record the limitation, and keep the independent-review
gate unsatisfied.

Reconcile and deduplicate every confirmed finding, obtain user adjudication when the workflow requires it, fix accepted blockers, and re-run affected proof. Then complete the Issue Completion Record gate, including reviewer verification of the written record, before declaring the issue complete.

If the repo has domain-specific gates, apply them after discovery. Examples include approved-only content, event schema invariants, tenant boundaries, import provenance, or feature-flag rules.
