---
name: issue-review
description: Create or review an issue, ticket, feature file, bug report, roadmap item, or implementation handoff for agent readiness. Use when asked to draft, create, tighten, validate, rewrite, prepare, or assess an issue so another agent or engineer can implement it with zero clarifying questions, including decomposing feature-sized work into Smallest Coherent Slices and validating or assigning stable numbered Conventional Commit-style filenames.
---

# Issue Creation And Review

Act as the **Planning Agent**. Create or review the issue against the bar: a
competent **Implementation Worker** should be able to complete its bounded
assignment with zero clarifying questions and produce a robust, validated
change. If ambiguity remains, the issue is not ready.

The user-facing agent retains interactive planning. Delegate bounded,
non-interactive investigation only when its expected time or model saving
exceeds handoff and integration effort, and the Planning Agent has the required
context and permissions. If delegated work discovers a user-owned decision, it returns
the decision and evidence to the user-facing agent instead of guessing or
attempting indirect user interaction. When delegation is unavailable, continue
in the primary agent without weakening this skill's discovery, decision, or
readiness gates.

Use plain language at a Grade 10 reading level in the rewritten issue, review findings, decision summaries, and clarification questions so they are quick and easy to understand. Prefer short sentences and familiar words. Preserve exact domain terms, code identifiers, and contract language, and explain necessary jargon when it first appears. Never simplify away technical precision.

Use the user's engineering-for-certainty doctrine as the default engineering standard when reviewing software issues: preserve repo conventions, validate trust boundaries, keep adapters thin and domain logic explicit, prefer explicit expected-failure contracts, and require tests for critical behavior and failure paths.

This skill prepares a **formal issue** when the user requests one or an
issue-driven delivery workflow requires one. A demonstrably Quick task can use
the task-level path in `$engineering-for-certainty` without creating an issue.
For an issue that does exist, record the selected rigor and its short risk
reason; keep issue-specific plans and checks proportional while preserving the
issue's explicit acceptance, traceability, and review gates. Do not expand a
small issue into a broad architecture survey without evidence of coupling.

Require companion engineering doctrine when the issue touches its area:

- Observability: logging, metrics, tracing, audit records, correlation IDs, telemetry, redaction, or frontend log ingestion.
- Resilience: external calls, retries, timeouts, idempotency, concurrency, queues, cron jobs, webhooks, background jobs, or async processing.
- Auth/security: cookies, sessions, CSRF, token handling, actor context, protected routes, permission checks, policy registries, secrets, or authorization boundaries.
- Frontend engineering: frontend architecture, routes or screens, API adapters,
  hooks, flows or views, forms, client state, accessibility, design-backed UI,
  client telemetry boundaries, or web/mobile testing.

If the companion skill is available in the current agent environment, use it. Otherwise use
the local repository's equivalent doctrine. If neither is available, name the
missing doctrine and do not declare the issue implementation-ready.

For an existing issue, require its path. When the user asks to create an issue,
discover the canonical issue directory, format, index, and next stable number.
Ask for a target path only when those facts cannot be discovered safely.

## Workflow

1. **Establish the issue input**: read the existing issue, or gather the approved problem and decisions for a new issue; identify the requested change, claimed files, dependencies, and current structure.
2. **Determine delivery intent**: use the initial request when it clearly says that implementation will or will not follow immediately; otherwise ask before creating a worktree or performing the final issue write.
3. **Discover repo conventions**: inspect local docs and examples before applying generic rules, including issue filename rules and the next unused issue reference.
4. **Verify claims against code**: check paths, symbols, line references, tests, schemas, commands, and stated behavior.
5. **Decompose and name the work**: prove the issue is one Smallest Coherent Slice or create an ordered child pack, then assign every slice its exact Branch Contract and PR base.
6. **Resolve gaps with `$grilling`**: inspect discoverable facts, investigate empirical unknowns, map all user-owned decisions by dependency, and work through one material decision at a time using stable question IDs.
7. **Accumulate answers**: maintain the decision map across items; do not edit the issue during the review.
8. **Build traceability and execution**: map each acceptance criterion to its production owner and exact verification, resolve the issue-ready design baseline when authoritative designs apply, define safe sequential or parallel implementation ownership, and set the Review Loop Contract for correction and escalation.
9. **Control issue attention**: keep the implementation contract concise, separate reusable doctrine and raw evidence, and define semantic review checkpoints only when delaying review creates material integration or rework risk.
10. **Cross-validate**: check the resolved issue, traceability ledger, checkpoint plan, execution plan, and conditional gates for contradictions and missing dependencies.
11. **Establish immediate-delivery isolation**: after shared understanding is confirmed and every selected slice has its exact Branch Contract, create or verify each selected implementation branch and linked worktree before the final write.
12. **Write and anchor once**: write the resolved issue and required planning updates in the owning context. For immediate delivery, commit them on the implementation branch before production-code changes begin.

Never write placeholders, TODOs, partial decisions, or "TBD" sections to the issue. The file is either unchanged while review is in progress or fully resolved when review is complete.

## Delivery Intent

Classify the initial request as immediate delivery, backlog only, or unresolved before the final write. For immediate delivery or ambiguous intent, read [Delivery Intent and Issue Ownership](references/delivery-intent.md); do not create an implementation worktree until the exact slice, branch, base, and shared understanding are settled. Backlog-only work creates no idle worktree.

## Convention Discovery

For issue creation, naming, or repository planning conventions, read [Convention Discovery](references/convention-discovery.md). Preserve the repository's established issue format and stable numbering.

### Composed Review-to-Merge Authorization

When an explicit review-to-merge workflow supplies verified findings, read [Composed Review-to-Merge Authorization](references/composed-review-to-merge.md) before writing a mechanical issue update. Product or scope decisions still require user confirmation.

### Deferred Follow-Up Issue Mode

When a composing pull-request workflow explicitly routes verified findings to `DEFER_FOLLOW_UP`, read [Deferred Follow-Up Issue Mode](references/deferred-follow-up.md).

## Decomposition And Branch Contract

Every creation or review must make one explicit decomposition decision: either
the issue is already one Smallest Coherent Slice, or it must become a parent
pack of smaller child issues. Do not use line count alone.

A Smallest Coherent Slice must:

- have one coherent observable outcome;
- be independently implementable, testable, and reviewable;
- leave the repository green after integration;
- own exact acceptance criteria, production surfaces, tests, dependencies, branch, and PR;
- avoid splitting contracts, persistence, behavior, and proof into technical micro-tasks that are not meaningful alone.

The slice need not be independently deployed when the product intentionally releases only after the full pack is complete. When splitting, record the parent, ordered children, cross-slice contracts, and release boundary. Preserve existing pack-local numbering when it is already stable.

Do not create or approve one feature-sized implementation issue or pull request
when the feature contains multiple Smallest Coherent Slices. Keep the feature as
a parent issue pack, and require each child slice to own exactly one branch and
one pull request. A single feature-wide pull request fails readiness unless the
issue proves that the work is one indivisible coherent outcome rather than
several outcomes grouped for convenience.

Require each slice to record its exact conventional Branch Contract before implementation:

```text
<type>/<NN>-<short-kebab-description>
```

Preserve any platform-required prefix. The type and stable issue number must
agree with the issue filename. A branch suggestion or pattern without the
resolved name does not pass.

For correction of an existing pull request under an explicit review-to-merge
workflow, do not rename or replace its published head branch merely to satisfy
the normal issue-derived naming convention. Instead, record an **Existing PR
Correction Contract** containing the resolved repository, pull-request number,
head owner and branch, base branch, reviewed head SHA, push authority, and
dedicated-worktree mode. This exception authorizes updates only to that existing
pull request. A fork without verified push authority is `BLOCKED`.

Record the exact pull-request base ref and worktree isolation mode with the
Branch Contract. Default to a dedicated linked worktree. Independent slices use
the verified canonical branch as their base; stacked slices use the preceding
pull-request branch. Do not substitute the default branch for a real stack
dependency or assume that a remote-tracking ref is current without verification.

Keep the issue portable: never store an absolute machine-specific worktree path
in the canonical issue. Require the pre-work execution handoff to record the
runtime path and resolved base SHA before work begins. Record a repository
convention or the user's explicit direction when the slice will use the shared
checkout instead.

Use Stacked Pull Requests only for real dependencies. Record each PR's head, base, preceding PR, merge order, and rebase or retarget procedure. Independent slices must share the canonical base branch and remain parallel rather than being forced into a stack.

## Issue Attention

Keep approved intent, acceptance, authority, and stop conditions prominent. For a non-trivial child issue or a long issue, read [Issue Attention and Reading Contract](references/issue-attention.md) before finalizing its structure.

## Claim Verification

Report mismatches before gate review:

- File paths that are missing, renamed, or too vague.
- Line numbers that drift by 3 or more lines.
- Referenced functions, classes, types, constants, routes, tables, commands, events, or config keys that do not exist where claimed.
- Behavior claims contradicted by current code or tests.
- Dependency claims contradicted by roadmap, issue files, or completed-work archives.
- Existing adjacent documentation that the issue would leave stale or false. When an issue adds content to a README, runbook, setup guide, enumerated list, exception rule, or placeholder-swap procedure, verify the neighboring claims against the current repo rather than checking only the new instructions for internal consistency.

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

When behavior depends on a platform or library mechanism, name the literal mechanism and its production owner. Do not accept abstractions such as "refetch," "refresh," "protect route exit," or "handle navigation" without the concrete API, hook, event, or interception point—for example, an imperative query call or History API interception—and the symbol or module that invokes it. Verify that the installed framework and version support the named mechanism.

When the issue introduces an environment variable that a runtime component reads directly, name the literal delivery mechanism, not only its presence in `.env` or `.env.example`. Trace the specific variable from its consumer—such as a workflow expression or `process.env` read—through every required injection point, such as a Compose `environment:` block or systemd `Environment=` entry. Do not infer its delivery from a superficially similar variable.

### 3. Root Cause Or Background

For bugs, identify the root cause rather than only the symptom. For features and chores, include enough background for an agent to make local judgment calls without re-litigating the product decision.

### 4. Acceptance Criteria

Every criterion must be testable or inspectable. It should describe an observable outcome, not an implementation wish.

Good: "`getApprovedQuestions` excludes draft and rejected records."
Bad: "question filtering works correctly."

For an issue that changes observable runtime behaviour, include an acceptance
criterion requiring every issue-owned Runtime Acceptance scenario to pass
against the exact final local candidate and every applicable authorized
pre-merge preview or staging candidate. Link post-merge-only staging proof as a
separate downstream release gate rather than implying it already passed.

When a criterion uses universal or negative language ("only", "all", "every", "never", "no other"), verify it cannot be satisfied by a weaker existential check. Either the issue enumerates the exact elements the claim covers and states that each must independently hold, or it says explicitly that the check is a spot-check and why that's acceptable. A criterion like "the file contains only placeholder values" is otherwise easy to implement as "at least one placeholder is present" — a check that passes even when most of the listed values are real.

When a criterion claims mutual exclusivity ("reachable from X, not from anyone else"), require independent verification in both directions: reject every disallowed side, and identify and accept the allowed side specifically rather than merely proving that some request succeeds. If the test method has a structural blind spot in either direction, state the blind spot and require a named compensating manual or automated verification step. For example, same-Docker-host traffic may use hairpin NAT and therefore cannot prove source-IP behavior across a genuine external network boundary.

When a criterion combines a formatted or rounded value shown to a user with a pass/fail or status indicator derived from the same raw quantity, require an explicit rule preventing disagreement at rounding boundaries. Derive both from the same displayed value, or state the precise reason divergence is intentional.

### 5. Scope Boundary

State what is explicitly not changing. This prevents adjacent refactors, UX expansion, schema churn, or product decisions from leaking into the task.

### 6. Implementation Change Control

The issue must state what judgment the Implementation Worker may exercise without
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

If the issue changes a config value, protocol, or URL with existing consumers, search for every consumer and inspect each one even when the expected conclusion is "no code change needed." State whether the change alters each consumer's failure modes, error classification, or default error state as well as its happy path. Pay particular attention to generic health checks, doctor commands, and catch-all error handlers, where a new expected failure can otherwise become a misleading operator message.

### 8. Test Approach

Name the test file or test layer. Include at least one concrete scenario in the repo's local test style. Critical behavior changes must include tests for success, expected failures, validation failures, and error mapping where those cases apply.

Before implementation, include a traceability ledger with one row per acceptance criterion:

| Criterion | Production owner | Exact test or verification |
|---|---|---|
| `<criterion ID and outcome>` | `<file + symbol/module>` | `<test file + case name, or justified manual check>` |

Split criteria that have multiple independently observable outcomes. Every row must name the code that owns the behavior and the exact evidence that will prove it; broad entries such as "frontend," "service layer," or "covered by tests" do not pass. Manual verification may substitute for automated criterion proof only when automation is impractical and the issue explains why. Runtime Acceptance proof remains required for observable runtime changes; a current automated test may supply a scenario when it exercises the assembled system through the real external boundary and asserts that exact outcome.

Require a post-implementation issue-against-diff audit by an **Independent
Reviewer** that did not implement the candidate and does not rely on the
Implementation Worker's completion summary. Reconcile every ledger row against
the actual production diff and test evidence, identify unplanned changes, and
leave the issue unverified while any row lacks evidence. A deliberately
separated pass in the same implementation context is supplemental evidence, not
independent review; record the limitation and keep the independent-review gate
unsatisfied.

When an acceptance criterion names a specific runtime mechanism (a "scheduled" job, a "background" retry, an "on reconnect" handler), the Test Approach must state whether verification exercises that literal mechanism or a named, justified proxy (e.g. a manual one-shot invocation of the same script the scheduler calls). An unstated substitution leaves a criterion looking tested when only an adjacent code path was actually exercised.

Apply the same rule to platform and library mechanisms named under Affected Surface: exercise the actual API, hook, event, or interception path, or name and justify the proxy and its blind spot.

When the issue introduces a new artifact covered by an existing repo-wide invariant, such as image pinning, network exposure, or secrets hygiene, inspect the shared test files or suites that encode that invariant. The Test Approach must name the existing assertions and explicitly extend them to the new service, image, port, secret, or other artifact rather than adding only scenario-specific tests.

When verification is split between standalone checks and a live or deployed instance, identify whether the issue-owned live-only portion contains the scenario most likely to expose an integration defect, such as cross-branch recombination, interacting failure paths, or multi-service behavior. If it does, the issue must remain explicitly unverified—use a status such as `Needs Verification`, not `Done` with a caveat—until that scenario has run successfully. When the environment can receive only merged changes, make the scenario an explicit downstream release gate with its owner and trigger; it blocks release rather than falsely claiming pre-merge proof.

If no local convention is visible, use:

```ts
test(`
  given <context>
  when  <action>
  then  <assertion>
`, () => {
  // ...
})
```

#### Runtime Acceptance Plan

For an issue with observable runtime behavior, read [Runtime Acceptance Planning](references/runtime-acceptance-planning.md). Require real-boundary scenarios and current exact-candidate evidence; keep the issue unverified while required proof is missing or stale. For non-runtime work, record a justified `Not applicable` entry.

#### Design Reference Baseline

When implementation is governed by an authoritative design, read [Design Reference Planning](references/design-reference-planning.md) and `$engineering-frontend`'s design audit guidance. Resolve the approved source, manifest, evidence bundle, and complete audit matrix before marking the issue ready.

### 9. Operational And Migration Safety

If the issue changes persisted data, generated artifacts, imports, migrations, deployment config, background jobs, or external integrations, state how the change is applied, rolled back or retried, and verified.

Every database schema or data migration must use the `$engineering-for-certainty` Migration Proof Harness. Require an isolated disposable local database container, the prior released schema, representative legacy and boundary fixtures, the exact production migration mechanism, after-state schema and data assertions, application read/write proof, and supported repeat invocation or no-op behavior. Require rollback proof only when rollback is part of the deployment contract; otherwise name forward recovery. Structural code migrations do not trigger the database harness.

If the issue adds persisted state alongside existing persisted state already enumerated in documentation, update every affected table, destructive-operations warning, backup or restore runbook, and blast-radius description. Documenting the new state in isolation does not pass when existing operational guidance would become false or incomplete. For example, a warning that says an operation destroys "both" named volumes becomes false when a third volume is added.

### 10. Observability And Errors

If the issue changes runtime behavior, state expected error behavior and any logging, metrics, audit events, or user-visible messages needed by the repo's conventions. If none are needed, say why.

When telemetry changes, require typed default-deny Safe Log Events. The issue must name each event family, allowed fields, source adapter, correlation ownership, retention and reader access, and tests proving that raw errors, arbitrary context, secrets, direct PII, and forbidden source-specific fields cannot reach logs, metrics, traces, audits, or fallbacks.

For frontend operational logging, require the client emission boundary and condition, deduplication or aggregation rule, production sampling or debug policy, bounded queue and delivery behavior, backend ingestion schema, authentication or reduced anonymous event set, rate and payload limits, non-recursive failure path, and Telemetry Budget. Product analytics and authoritative backend security or audit events remain separate.

When errors use a discriminated union or coded envelope, require a code-to-details matrix that names each error code, its compatible `details` shape, its producer, and each consumer's behavior. The Test Approach must cover every valid mapping plus missing envelopes, malformed envelopes, unknown codes, and code-incompatible `details`. State the fallback behavior for invalid combinations; do not let consumers trust `details` based on shape alone or a code alone.

### 11. Engineering Certainty

The issue must make the robust implementation path clear:

- Identify the trust boundary and validation schema for every external input.
- Keep route/page/controller/adaptor work thin; name where domain logic belongs.
- State the expected-failure contract used by the repo, such as `Result<T, E>`, error unions, or the local equivalent.
- Require exhaustive handling for discriminated states, enums, result variants, or UI status unions.
- State whether config, auth context, clocks, clients, or persistence are injected or otherwise supplied through the repo's existing pattern.
- For structural refactors, require behavior-preservation tests and name what must not drift.
- Identify any companion doctrine triggered by the issue and include its requirements in the issue.

### 12. Issue Completion Record

Every formal issue defines the write-back and status propagation required at completion. Read [Issue Completion Record](references/issue-completion-record.md) when drafting completion requirements or reviewing a completed issue. The Delivery Operator writes the evidence index; the final Independent Reviewer verifies it. Missing issue-owned proof keeps status at `Needs Verification`.

### 13. Review Loop Contract

Every rewritten issue states how implementation hands off to review and how verified findings are routed. Read [Review Loop Template](references/review-loop-template.md) when the issue is intended for `$issue-delivery`; otherwise state a human-gated workflow and disable automatic correction explicitly. Every correction batch requires invalidated proof to be rerun and a fresh full resulting-change review.

## Conditional Gates and Execution

For affected auth, telemetry, resilience, persistence, external data, frontend states, version-dependent behavior, or parallel implementation, read the relevant parts of [Conditional Gates and Execution](references/conditional-gates-and-execution.md). Optional parallel work must pass the delegation break-even rule in `$engineering-for-certainty`; distinct ownership and isolation remain required. A final independent full integration review follows all checkpoints and correction batches.

## Questioning Discipline

Use `$grilling` as the questioning discipline for unresolved gates and contradictions.

- **Discoverable fact**: inspect the code, docs, tests, history, or configured tools. Do not ask the user to recall repository facts.
- **User-owned decision**: map the full dependency-aware queue, then ask one material decision using a stable ID. State the gate or contradiction, concrete risk, options, and recommended answer. Keep the current decision active until it is accepted, rejected, revised, deferred, or blocked on named evidence.
- **Empirical unknown**: run a bounded reproduction, spike, benchmark, or research pass when safe and authorized. Report the result; do not turn an investigable unknown into a preference question.

After each reply, update the decision map, resolve dependencies, and recompute the queue. Inherit `$grilling`'s `Decision <position> of <total> - <remaining> remain after this` progress marker and explicit total-change notice. Present only the next eligible decision after the current one has an explicit disposition. Batch independent, low-consequence decisions only when the user explicitly asks for faster batch treatment; accept bulk replies such as `agree to all` only for that explicit batch.

Investigate and deduplicate the complete review landscape before reporting it, then present one confirmed finding at a time with its stable ID and `**Progress: Finding <position> of <total> - <remaining> remain after this**` on every response that presents or continues it. Calculate position and total using prior dispositions plus the active queue; do not show a remaining count alone. Keep the current finding active until the user accepts, rejects, revises, defers, or requests named evidence. Recompute the remaining queue after every disposition. If the total changes, state `**Queue revised: <old> -> <new>.** <reason>` before the next finding; never silently change the denominator or stable finding IDs. Batch at most ten only when the user explicitly requests a batch or complete report; there is no total finding cap.

For a long or multi-session review, persist the grilling decision map in a separate working artifact if useful. Never use partial issue content, placeholders, TODOs, or `TBD` sections as interview state.

Before rewriting, present the resolved understanding and explicitly confirm that it is shared. The user's approval authorizes the final coherent rewrite; it does not authorize partial writes during the interview.

## Cross-Validation

Before the final issue write, read [Cross-Validation](references/cross-validation.md) and resolve contradictions in scope, traceability, branches, proof, authority, and linked planning surfaces. Do not write an implementation-ready issue while a required gate is missing.

## Rewrite

After shared-understanding confirmation, read [Issue Rewrite](references/issue-rewrite.md) for the fallback format, final write, and immediate-delivery handoff. Preserve the repository's format when it exists. Do not write placeholders or partial decisions.
