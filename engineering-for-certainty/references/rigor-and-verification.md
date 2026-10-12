# Risk, Rigor, and Verification

Choose the cheapest proof that establishes the requested behavior. Fast and
Extensive are assurance modes, not authority to skip explicit user, repository,
or approved-issue gates. The same selection applies to tasks and formal issues.

## Select Rigor

**Fast is the default**, including ordinary features and bug fixes with bounded
integration risk. Assess plausible failure impact, likelihood, reversibility,
coupling, and available proof. Classify changed behavior, not file names, line
count, number of layers, or whether the request has a ticket. State the mode and
a short reason in the existing task, issue, or PR; no separate record is needed.

| Mode | When it fits | Completion path |
| --- | --- | --- |
| **Fast** | Ordinary bounded work whose consequences and affected boundaries can be established with focused investigation and direct checks. | Inspect relevant code, callers, and instructions; keep a short plan when useful; implement; obtain direct proof; self-review the final diff against the request, affected callers, and meaningful failure paths. No routine independent review or new canonical issue is required. |
| **Extensive** | Explicitly requested, or changed behavior could materially affect permissions, authentication or security, money, data integrity, destructive actions, production availability, recovery, or another high-impact invariant. Also use it when uncertainty prevents bounding those consequences. | State important assumptions, invariants, and failure modes; obtain deeper affected-behavior and integration proof, adverse cases and recovery evidence where relevant; require independent review of the complete resulting change. Broaden regression checks when shared behavior or uncertain coupling warrants it. |

Merely touching persistence, concurrency, an auth screen, or infrastructure does
not force Extensive. A nickname field using an established validated update path
may be Fast if storage and access rules remain intact and focused evidence proves
the outcome. Changing who may update another person's profile is Extensive even
if it is one line. Use bounded inspection to resolve uncertainty; if it remains,
escalate. A request for Fast never lowers a required high-impact proof floor.

Explicit issue or repository gates still apply in either mode. Invoking
`$issue-review` or `$issue-delivery` does not itself require Extensive or an
independent reviewer. Preserve the formal workflow's scope, acceptance,
dependencies, and completion record. Do not silently weaken an already-approved
issue: revise explicit requirements through its approval path. For legacy
Quick/Standard/Critical records, reassess behavior using these modes; retain all
explicit proof and review commitments until approved revisions change them.

## Spend Verification Where It Changes Confidence

Start with the smallest relevant module, nearby patterns, affected callers, and
existing checks. Expand when a changed contract, failing check, or other evidence
reveals coupling. Reuse current plans and verified facts; do not repeat broad
scans, full suites, or checks without a newly invalidated assumption.

- **Both modes:** establish changed behavior and meaningful failure paths with
  direct evidence. Reuse existing tests when they cover the new outcome. Add
  tests where they protect meaningful logic, failure handling, or a costly
  regression. Focused manual or visual checks may suffice for suitable changes;
  awkward automation alone is not a reason to omit needed regression protection.
  Compilation and lint alone do not establish runtime behavior. Exercise real
  wiring when local checks could conceal an integration failure. Briefly report
  what was checked and meaningful gaps; do not call missing evidence verified.
- **Extensive:** explicitly test affected invariants and important failure modes.
  Use integration, adverse-case, recovery, migration, property, mutation, or
  security proof where the behavior triggers it. Neither a full application suite
  nor a surrounding architecture audit is mandatory merely because the mode is
  Extensive. A whole-system audit is a separate explicit request.

For new or materially changed tests, apply
[Testing Doctrine](verification-details.md#testing-doctrine). Formal observable
runtime issues retain [Runtime Acceptance](runtime-acceptance.md). Companion,
issue, and repository gates remain binding; a mode is not a waiver. Review
confidence cannot replace missing behavioral or high-impact evidence.

## Review and Corrections

Fast uses self-review by default. Obtain a focused independent review only when
a concrete concern makes another perspective useful, or an explicit user,
issue, repository, or triggered specialist gate requires it. Ordinary integration
or multiple files alone is insufficient. A consequential concern escalates to
Extensive; a specific missing proof must still be obtained.

Extensive requires a read-only Independent Reviewer context that did not
implement the candidate. Its initial review covers the full resulting change,
including affected contracts and integration seams. Scale depth and additional
review contexts to actual risk. Self-review cannot satisfy an independence gate.

After a correction or later commit, in either mode:

1. Inspect what changed and rerun the proof it could invalidate.
2. Reassess prior findings and interactions with unchanged work.
3. Fast gets updated self-review. Where independent review is required, the
   Independent Reviewer may carry forward valid earlier coverage after examining
   the correction, its interactions, and the continuing validity of that coverage.
   Return an updated verdict on the exact current candidate, naming the prior
   reviewed revision, new range, carried evidence, and remaining limitations.
4. Require another full independent review when the correction changes shared
   behavior or contracts broadly, earlier coverage cannot be established, or its
   impact is uncertain. An explicit stronger re-review gate still controls.

Do not restart discovery solely because a local correction changed the head.
Do not attach an earlier clean verdict to a newer revision without current
verification. This rule also applies to evidence-only, walkthrough, and queue
commits: verify their actual effects before carrying forward proof. A required
initial full integration review cannot be replaced by separate checkpoint reviews.

## Escalate and Stop

Escalate when consequential risk or hidden coupling appears, repeated fixes fail,
requirements become materially ambiguous, architecture conflicts with the plan,
or available proof cannot support the outcome. Update the mode and evidence plan;
do not keep Fast merely because the work started Fast. Material user-owned
decisions still need direction; choosing assurance does not grant new authority.

Stop when acceptance is met, direct proof applies to the final candidate, the
final diff is understood, required review and repository gates pass, no blocking
finding or high-risk uncertainty remains, and eligible follow-ups have their
[Follow-Up Inbox](follow-up-inbox.md) disposition. Additional passes need a
concrete reason; do not search indefinitely for hypothetical improvements.

## Records and Delegation

Use the smallest record the lifecycle needs. Keep ordinary planning and working
evidence in the active chat; reconcile completion into the existing issue or PR.
Formal issues retain one concise completion record in the canonical issue.
Create a separate document only for cross-session or cross-agent continuity,
lasting operational guidance, or an explicit requirement. Extensive needs fuller
evidence, not automatically more files. Keep each fact in one clear home and
link to raw logs rather than copying them. Existing selected-human-review
walkthrough and repository documentation requirements remain in force.

Before optional delegation, check the break-even case: is there a bounded output
that can be verified, and is the likely elapsed-time or cost saving greater than
prompting, context transfer, waiting, reconciliation, and integration? Delegate
by required capability; the harness chooses concrete models and mechanisms. Do
not invent numeric cost estimates. Parallel writers need disjoint ownership,
isolated worktrees, and a named integration path. Required independent assurance
still applies even when optional delegation is not worthwhile.

## Rollout Evaluation

Introduce Fast as the shared default. The delivery operator evaluating the tenth
substantive Fast delivery after this rollout performs the initial evaluation;
if history or metrics are unavailable, name the gap rather than invent a count
or claim the checkpoint is complete. Count distinct feature or bug deliveries,
not correction passes or bookkeeping. Reuse existing task, runner, issue, and PR
records; do not add per-task measurement files, duplicate execution, a schedule,
or a shipping gate for this evaluation.

Compare similar historical and new tasks using elapsed time, tokens or actual
cost where exposed, review/correction cycles, escalations, and subsequent fixes.
Label unavailable metrics. Ten deliveries can expose overhead and usability
problems, not establish a reliable defect rate; assess escaped defects and
rework over longer use. Record the evaluation in the existing rollout PR or
canonical workflow record and propose evidence-backed adjustments for approval.
