# Review Loop Contract

Use this contract only when `$code-review` is the read-only Independent
Reviewer inside a composing workflow such as `$issue-delivery`. Direct review
keeps the interactive Review Queue behavior from `SKILL.md`.
Return the complete verified queue at once only when the composing workflow has
authority to process it; this does not weaken review independence or user
ownership of material decisions.

## Required Delivery Contract

The governing issue or approved task handoff must state authorized transitions,
the automatic-correction boundary, user-owned decisions, required revalidation and re-review, the churn
threshold, and the completion condition. Authorized `$issue-delivery` and
review-to-merge workflows may make the repository-local follow-up inbox writes
defined by [Follow-Up Inbox](../../engineering-for-certainty/references/follow-up-inbox.md);
read-only review may only recommend them. The responsible delivery operator
holds publication and merge authority under
[Delivery and Human Review](../../engineering-for-certainty/references/delivery-and-human-review.md),
subject to explicit narrower boundaries. Never infer authority from a goal,
loading a skill, or an analysis-only request.

## Review Ownership

The Independent Reviewer owns candidate discovery, normalization,
deduplication, skeptical verification, and final evidence verdicts. Every
review context stays read-only: it may inspect and run safe non-mutating proof,
but it must not edit, commit, push, publish comments, or resolve threads.

The Delivery Operator owns routes, corrections, publication, CI follow-up, and
user escalation. This separates whether a claim is true from what the delivery
workflow does next.

## Finding Routes

After receiving the complete verified queue, the Delivery Operator attaches
exactly one route to every non-refuted candidate: `AUTO_CORRECT`,
`DEFER_FOLLOW_UP`, `USER_DECISION`, `BLOCKED`, or `RESIDUAL_RISK`.

Use `AUTO_CORRECT` only when the finding is `CONFIRMED`; the failure and
correction are fully inside the approved issue or task; the correction has one
clear interpretation; it preserves approved architecture and public contracts; it
adds no dependency, migration, schema, permission, security-policy, or test-
strategy choice; it requires no choice between plausible product meanings; it
does not hide, weaken, or replace required proof; and verifier evidence has no
material disagreement. It authorizes the Delivery Operator—not the Independent
Reviewer—to apply the smallest correction and focused proof.

Use `DEFER_FOLLOW_UP` during authorized issue delivery or review-to-merge only
when the confirmed finding meets the [Follow-Up Inbox](../../engineering-for-certainty/references/follow-up-inbox.md)
eligibility contract. Severity supports ranking but never decides deferral by
itself. The reviewer returns root-cause evidence, why the current change
remains safe, the minimum affected surface, and a follow-up seed. The Delivery
Operator owns deduplication, repository-local inbox capture at closeout,
publication when authorized, and current-head revalidation. A full
implementation-ready issue is created only when the follow-up is selected for
work. `DEFER_FOLLOW_UP` is a finding route, not a checkpoint result.

Use `USER_DECISION` for material intent, scope, architecture, public contract,
schema, migration, permission, security, dependency, or test-strategy choices;
`NEEDS_CONTEXT`; product-sensitive `CONDITIONAL` results; evidence disagreement;
oscillating fixes; or the same root cause surviving the issue's correction
threshold. Use `BLOCKED` for
missing authority, access, external state, skills, or prerequisites. Use
`RESIDUAL_RISK` for a `CONDITIONAL` result whose explicit assumption does not
justify changing implementation.

## Checkpoint Advancement

After routing the review queue, the Delivery Operator assigns `CLEAN`,
`AUTO_CORRECT`, `USER_DECISION`, or `BLOCKED`. An eligible `DEFER_FOLLOW_UP`
finding may coexist with `CLEAN` when its evidence note is retained for
closeout inbox filing and it weakens no checkpoint-owned criterion, applicable
doctrine, or required proof. `RESIDUAL_RISK` may coexist with `CLEAN` only when
the issue makes the assumption non-blocking without weakening acceptance or
highest-risk proof.

## Delivery Return Record

After discovery, deduplication, and verification finish, return every result:

```text
finding_id
verdict
checkpoint_id_when_applicable
review_base_sha
candidate_head_sha
file_and_line
failure_scenario
evidence
suggested_correction
route_relevant_facts
follow_up_issue_seed_when_applicable
proof_invalidated_by_correction
required_rereview_scope
```

The Delivery Operator augments records with `route`, `route_rationale`, and
`checkpoint_result_when_applicable`, then may batch coherent corrections and
deduplicate authorized follow-ups.

## Re-Review

After correction, review the resulting head, rerun affected proof, reassess
prior findings, and inspect the full resulting diff. Preserve IDs for existing
root causes. Checkpoint review never replaces final full integration review.
Do not declare the loop clean until every `CONFIRMED` finding on the current
head has a valid correction, decision, or deferred disposition, every prior
finding has a disposition, required inbox entries are filed at closeout, and
every declared coverage or independence limitation is recorded.
