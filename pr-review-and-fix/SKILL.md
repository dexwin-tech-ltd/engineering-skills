---
name: pr-review-and-fix
description: Review and correct an open PR through verified findings, independent re-review, CI, repository labels, decisions, and verified merge by default. Use when asked to own review and corrections; honor explicit reviewer-reassessment boundaries. Merged-PR feedback uses the post-merge human-review route in pull-request-review.
---

# PR Review and Fix

## Ownership and authority

Own the open PR from review through authorized corrections and verified merge,
unless the user or governing issue explicitly stops at reviewer reassessment.
Read [Delivery and Human Review](../engineering-for-certainty/references/delivery-and-human-review.md).
For an already-merged PR, use `$pull-request-review`'s post-merge route and a
new correction branch/PR; never apply this open-head mutation workflow to it.
Use `pull-request-review` for GitHub evidence, comment formatting,
thread ownership, and readback; `code-review` as the sole analysis engine; and
`engineering-for-certainty` as governing doctrine. Load every engineering
companion triggered by the reviewed or corrected surface. Resolve these
required skills before starting; report missing dependencies rather than
reconstructing their instructions.

An explicit request to execute this combined workflow authorizes in-scope
corrections, meaningful regression coverage, local validation, commits and
pushes to the existing PR branch, PR comments and decision records, repository-
defined label transitions, re-review requests, repository-local Follow-Up
Inbox entries for eligible findings, and the Slack decision messages below.
Merely discovering or reading this skill grants no write authority.
Honor narrower user authorization and runtime permissions.

This workflow replaces ordinary interactive finding adjudication and the
`pull-request-review` issue-delivery handoff for clear in-scope corrections.
The active governing agent fulfills the Delivery Operator role and owns
implementation and publication. Independent Reviewer contexts remain
read-only. Retain this workflow's correction routes and decision notifications;
after correction, use the shared Review-to-Merge gates and guarded cleanup
without restarting ordinary interactive adjudication. Do not approve or dismiss
reviews, force-push, grant new deployment authority, bypass branch protection,
or perform unrelated cleanup. External mutations during runtime
validation require existing authorization.

Do not send private learning feedback or other Slack messages. Only decision
notifications and related clarifications in `github-review-requested` are
covered. Qualifying PR learning feedback retains the review skill's evidence
and deduplication rules; it must not delay decision escalation.

Run without waiting interactively or indefinitely. Finish independent work and
leave a durable handoff when blocked. Process responses on a later invocation
or when they arrive during the active run; this skill does not create a watcher
or automation.

## Establish state and the review contract

Read applicable `AGENTS.md`, referenced workflow documents, the PR description,
governing repository issue files or handoff, acceptance criteria, contracts,
review submissions, inline threads, PR discussion, and previous decisions.
Verify the repository, declared base, head branch, current head SHA, and working
state. Preserve unrelated work and use an isolated worktree when necessary.

Inventory every unresolved concern, including review-summary requests without
inline threads. Preserve stable finding IDs, source links, thread ownership,
verdicts, decisions, corrections, and proof. Deduplicate root causes without
losing individual thread associations. Recover this state from durable PR and
issue records on subsequent runs, not from presumed conversation memory.

Read `code-review/references/review-loop-contract.md` from the installed skill.
Before implementation, record this execution's Review Loop Contract in the
existing governing issue or handoff, limited to the authorized scope:

- Automatic review, in-scope correction, push, and CI follow-through are allowed.
- Only the AUTO_CORRECT criteria below permit correction without adjudication.
- Material choices use USER_DECISION; only findings eligible under
  `$engineering-for-certainty`'s Follow-Up Inbox contract may use
  DEFER_FOLLOW_UP.
- Every correction requires affected proof and full resulting-diff re-review.
- Two unsuccessful correction cycles for the same root cause force escalation.
- Completion means verified merge with required selective human-review tracking
  and authorized release follow-through, unless the explicit boundary is
  reviewer reassessment or another narrower target.

If no governing artifact exists, create a bounded PR correction handoff in the
repository's prescribed location, or a PR comment when no location is defined.
Record established intent and scope without inventing acceptance criteria.
Unsettled requirements remain decisions. Supply this contract and governing
artifacts to code-review in Composing Delivery Mode. This workflow owns durable
USER_DECISION handling instead of its interactive presentation requirement.

## Review and route

Review the complete declared-base-to-current-head diff. Use code-review's full
discovery, deduplication, skeptical verification, and evidence standards.
Establish intended behavior, concrete trigger, executed path, cause,
consequence, confirming and refuting evidence, correction, and regression proof.
Verify existing feedback against current code: moved lines, outdated anchors,
new commits, and replies saying “fixed” are not proof.

Investigate discoverable facts and use bounded validation for empirical
uncertainty. Distinguish defects from preferences and changed requirements.
Do not mechanically adopt a reviewer's suggested solution.

Route each result:

- **AUTO_CORRECT**: CONFIRMED, one clear interpretation under approved scope,
  preserves agreed architecture and public contracts, requires no unresolved
  product, dependency, migration, permission, or security-policy choice,
  preserves acceptance criteria and proof, and has no material evidentiary
  disagreement.
- **DEFER_FOLLOW_UP**: CONFIRMED, outside the approved PR correction scope, and
  independently releasable under the Follow-Up Inbox contract. Retain the
  evidence, promptly flag conspicuous user-visible findings, and file a
  deduplicated repository backlog entry after corrections and re-review.
- **USER_DECISION**: unresolved material intent or scope, architecture, contract,
  security or other user-owned choice; material disagreement evidence cannot
  settle; oscillating fixes; or the same root cause surviving two correction
  cycles.
- **BLOCKED**: missing access, authority, credentials, required skills, proof
  infrastructure, or external prerequisite. Name the clearing action or owner.
- **RESIDUAL_RISK**: conditional claim with explicit unresolved assumptions.
  Do not correct or publish it as a confirmed defect. Escalate if its resolution
  requires a material decision or prevents completion.

Retain refuted and already-addressed dispositions with evidence. Do not silently
defer blocking concerns or create follow-up issues to escape the current gate.

## Escalate USER_DECISION

Post in the existing relevant PR thread, or a top-level PR comment when no
thread exists. Include the stable finding ID, verified concern, exact decision,
why evidence and instructions do not settle it, options and trade-offs,
recommendation, and dependent work. Identify a decision request as such rather
than presenting uncertainty as a confirmed defect.

Notify the verified `github-review-requested` Slack channel with the PR link,
decision needed, recommendation, and direct link to the PR decision record.
Each Slack message must concern exactly one USER_DECISION or its associated
finding. Never batch separate decisions or findings into one message, even when
they concern the same PR. Include that item's stable finding ID, focused
question, options, recommendation, and direct PR decision-record link so it can
be understood and answered independently. Alternatives for that one decision
may appear together; independently answerable questions require separate
messages. Keep clarifications and responses in the corresponding Slack thread
and track notification delivery and deduplication per decision. This formatting
rule does not authorize Slack publication of ordinary non-decision findings.
Verify the exact channel; never guess or substitute a destination. Read back
both writes.
If Slack is unavailable or delivery fails, preserve the PR record, report the
failure, and continue independent work. If the PR record cannot be published,
retain its exact content in the handoff and report the failed publication.

Check existing records, responses, and notification history before posting.
Avoid duplicates; notify again only for material changes to the decision,
evidence, or blocking impact. Inspect actual state before retrying an uncertain
write. Ask necessary clarification in the existing PR or authorized Slack
conversation. Never implement changes dependent on an unanswered decision.

## Record and implement decisions

Check responses in the PR, corresponding Slack discussion, and available task
conversation. Accept only an explicit answer from a verified authorized
decision-maker that addresses the question. Establish authority through
repository instructions or a reliable account mapping. Silence, reactions,
elapsed time, labels, and unrelated comments are not decisions. Ambiguous,
incomplete, or conflicting responses require clarification; independent work
may continue.

For a clear decision:

1. Record the finding ID, decision, decision-maker, source link when available,
   implementation implications, and validation expectations in the original PR
   discussion. Do not add unauthorized requirements. If the answer is already
   in the PR, acknowledge its implications without duplicating it.
2. Read back the durable record before dependent implementation or transitions.
3. Mark a correction as awaiting fixes, or record a no-change disposition and
   rationale. An answered question is not a completed correction.
4. Apply and verify the AGENTS.md pending-fixes state, accounting for any other
   open decisions as specified below. A label-rule gap or failed label write
   does not revoke a recorded implementation decision: preserve labels, report
   the gap, and proceed with otherwise authorized corrections. It still blocks
   a claim that the required workflow-state completion gate passed.
5. Update the governing repository issue or handoff when the explicit decision
   changes its contract, then implement the clearly authorized correction
   without asking for a second confirmation. Escalate remaining contradictions.

Deduplicate decision records. A revised decision requires a new record naming
what it supersedes, followed by reassessment of findings, code, proof, and state.
A decision establishes intent; evidence must still prove implementation.

## Correct and independently re-review

Make the smallest coherent root-cause correction across affected paths. Add
meaningful regression coverage when appropriate and run repository-required
checks plus focused proof. Do not weaken tests, acceptance criteria, checks, or
security controls. Distinguish executed proof from unavailable checks.

Use a separate read-only Independent Reviewer context with code-review against the complete
resulting base-to-candidate-head diff, not merely correction commits. Supply raw
requirements, recorded decisions, prior finding IDs, current diff, proof, and
limitations. Require independent verification, attempted refutation, prior-
finding reassessment, and regression inspection.

Apply clear AUTO_CORRECT results, repeat invalidated proof, and independently
re-review the resulting head. Preserve IDs; new root causes get new IDs.
Escalate recurring or oscillating corrections at the two-cycle threshold. If
independent review is unavailable, record that blocker; a self-review does not
pass the independent-review gate.

## Publish and reconcile

Before pushing, recheck the remote PR head. Reconcile concurrent work without
overwriting it, repeating invalidated validation and review. If another active
operator is changing the same PR, coordinate ownership or stop conflicting
writes. Push only verified, independently safe corrections to the existing
branch; never publish a partial fix dependent on an unresolved decision.

Check CI for the exact pushed head. Correct failures caused by this work;
report unrelated, inaccessible, or pending checks accurately.

Reassess proposed `DEFER_FOLLOW_UP` findings against the corrected head. For
those still eligible, use `$issue-review` Deferred Follow-Up Inbox Mode to
write or reuse repository backlog entries, creating root `BACKLOG.md` when the
repository has no canonical backlog. Recheck the remote head, link the entries
in the PR description, and push any planning-only commit to the existing
branch. Re-run invalidated proof and independently verify that the new head
remains releasable; check CI for that exact head. A full issue is prepared only
when the follow-up is selected for work.

Reply to existing concerns with the change or refutation, commit/code evidence,
validation, and remaining limitations. Record newly found and corrected defects
in a durable correction summary; do not publish them as still blocking.
Publish remaining confirmed defects using pull-request-review's standalone
plain-language comment and stable-marker standards. Explicitly determine
blocking status from requirements and consequences, not severity alone.
Submit REQUEST_CHANGES only for a published confirmed defect still blocking the
reviewed head; otherwise use a comment review. Do not manufacture an approval
or dismiss a previous changes-requested review after fixes.

Leave human-owned threads open. Decision, clarification, recording, and
correction replies there are authorized. Resolve only workflow-owned threads
identified under pull-request-review's marker rules, independently verified as
fixed or obsolete with required proof, and only when GitHub permits resolution.
A matching account, implemented change, or recorded decision is insufficient.

Refresh head and discussion state before publication or decision-driven label
changes. Reassess stale evidence. Read back comments, review submissions,
thread resolutions, and other external writes; mutation responses alone are
not completion evidence.

## Repository labels

Applicable AGENTS.md and its referenced rules are the source of truth for label
names and transitions, overriding skill conventions and comment templates.
Never hardcode or import another repository's labels.

- Preserve the prescribed state or signal while USER_DECISION remains open.
- After recording a decision requiring implementation, apply the prescribed
  pending-fixes state before implementing it.
- Represent simultaneous pending fixes and unanswered decisions only as the
  repository specifies. If coexistence rules are missing, preserve labels and
  report the gap rather than implying all questions are settled.
- Transition to reviewer reassessment only after the completion gates pass.

If a required label or transition rule is unavailable or ambiguous, preserve
current labels, report the gap, and continue independent work. Do not create
substitutes or remove unrelated labels. Use these same repository rules in
published next-step instructions. Read back every label change.

## Completion

Request reviewer reassessment only when all blocking concerns have evidenced
dispositions, required corrections are pushed, no USER_DECISION remains,
required checks and CI pass for the current head, independent full-diff review
has no undispositioned confirmed findings, every deferred finding has a
verified backlog entry or exact existing owner linked from the PR, and
repository workflow labels match the required state. Verify the request and
avoid duplicates for an unchanged head.

Reviewer reassessment remains a request, not proof of approval. If the effective
boundary includes merge, prepare required human-review enrollment through
`$pull-request-creation`, verify its resulting head and CI, and apply
[$pull-request-review's merge gates and cleanup](../pull-request-review/references/review-to-merge.md#merge-gates).
Required reviewer approval and repository gates still block merge. Reuse valid
independent current-head evidence, reconcile tracking after verified merge,
and follow only the established authorized release process. Queue age or size
does not add a shipping gate.

Use accurate, potentially overlapping outcomes:

- **Merged; human review pending or not required**: shipping and tracking verified.
- **Corrections completed; awaiting reviewer approval**: correction proof passed,
  but an explicit reassessment boundary or required approval stops shipping.
- **Awaiting human decision**: independent work finished; decisions remain.
- **Decisions recorded; awaiting fixes**: decisions settled, but a named blocker
  prevents finishing their corrections.
- **Blocked**: name the missing prerequisite or proof and clearing action.

Read back external state and report PR link and final SHA, review scope,
corrections/commits, validation and independent-review/CI results, finding
and thread dispositions, backlog and decision/source links, Slack delivery status,
verified labels or rule gaps, re-review status, and remaining clearing actions.
An escalation is not a resolved finding, a recorded decision is not a verified
fix, and a push is not passing proof. Never claim merge approval.
