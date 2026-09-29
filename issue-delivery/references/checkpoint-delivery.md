# Checkpoint Delivery

Use this loop when the approved issue defines more than one review checkpoint.

## Before Each Checkpoint

Load only the checkpoint's required operating set:

- the core issue contract and `Agent Start Here`;
- the current checkpoint row;
- its owned acceptance criteria and traceability rows;
- its production and test surfaces;
- its triggered specialist passes and the proof each pass owns;
- every appendix explicitly required by that checkpoint;
- prior accepted checkpoint heads and unresolved residual risks; and
- the governing Review Loop Contract.

Do not treat earlier implementation summaries as substitutes for these sources.

Before implementation, the **Delivery Operator** either selects a suitable
**Implementation Worker** or declares that it will perform the worker role in
the primary context. Every delegated assignment must include the approved
behaviour, exact files or symbols, owned acceptance and traceability rows,
required validation and specialist proof, allowed mechanical judgment,
escalation conditions, and required return evidence. One shared implementation
worktree has at most one active writer.

## Checkpoint Loop

For each checkpoint:

1. The Implementation Worker implements only the current checkpoint's approved
   behavior and surfaces, then returns changed files, validation evidence,
   failures, deviations, and residual risk to the Delivery Operator.
2. The Delivery Operator inspects the returned diff and evidence against the
   assignment before accepting it.
3. Run its exact validation, delivery-owned specialist passes and proof, and the
   repository gates needed to leave the branch green. For design-backed work,
   run the checkpoint scope defined by
   [`$engineering-frontend`'s Design Conformance And
   Audit](../../engineering-frontend/references/design-conformance.md).
4. Create a coherent local checkpoint commit under the repository's history
   conventions and freeze its candidate head SHA.
5. Invoke `$code-review` in Checkpoint Review Mode using an Independent Reviewer
   context that did not implement the checkpoint.
6. Use the issue base as the first checkpoint base. For later checkpoints, use
   the previous accepted checkpoint SHA as the primary review base.
7. Require the Independent Reviewer to inspect the new range plus its
   integration seams with earlier accepted checkpoints and any shared contract
   it changes.
8. Wait for complete checkpoint discovery, verification, deduplication, and
   verdicts before editing.
9. The Delivery Operator inspects every reviewer verdict and routes each result
   through the Review Loop Contract.
10. Apply one coherent batch of independent `AUTO_CORRECT` findings where
   practical.
11. Re-run invalidated validation and specialist proof, freeze the corrected
    head, and re-review the same checkpoint.
12. Record the accepted head SHA only when the checkpoint passes its advance
    rule.
13. If this is a UI-first user-review checkpoint and a pull request is the
    only way to obtain its pre-merge preview, invoke `$pull-request-creation`'s
    checkpoint-preview path for the accepted head before staging review. Respect
    the effective publication boundary. Keep the pull request draft and the
    issue `Needs Verification`; the backend and final integration review are
    still pending. If the preview cannot be obtained, do not advance.
14. If this is a UI-first user-review checkpoint, run or reconcile the
    separately authorized staging deployment of the accepted UI candidate,
    record its exact build and scenario links, and wait for the user's UI
    approval. A clean engineering review is not that approval. If staging was
    replaced, recheck the affected evidence before requesting approval. If the
    user requests changes, correct and re-review the affected checkpoint work,
    record the corrected accepted head, update the same draft pull request
    when it owns the preview, then restage that candidate for approval.
15. Begin the next checkpoint from the accepted head only after every planned
    user-review gate has passed.

## Advance Rule

Advance only when:

- the reviewed candidate and accepted head are the same;
- all checkpoint-owned traceability rows have evidence;
- every triggered specialist pass has current evidence, an outcome, and any
  limitation recorded;
- every confirmed finding has a valid correction, decision, or eligible
  deferred disposition;
- every prior checkpoint finding has a disposition;
- correction-invalidated proof has been re-run; and
- every residual risk is explicitly permitted by the issue.

`AUTO_CORRECT` returns to the same checkpoint. `USER_DECISION` and `BLOCKED`
pause delivery and leave any durable goal incomplete.

`RESIDUAL_RISK` remains a finding route. Record a permitted residual risk and
return `CLEAN`; return `USER_DECISION` when the issue does not permit it.

`DEFER_FOLLOW_UP` remains a finding route, not a checkpoint result. Retain its
evidence note, return `CLEAN` only when the Follow-Up Inbox eligibility
contract passes and all checkpoint-owned proof remains valid, then file the
deduplicated entry at issue closeout. Return `USER_DECISION` when eligibility
or the approved issue meaning is unsettled.

The existence of a goal never permits advancement, finding suppression,
severity reduction, weakened proof, or silent selection of product meaning.

## Churn

Escalate to `USER_DECISION` when:

- the same root cause survives two correction cycles;
- the fix oscillates between alternatives;
- a new finding exposes ambiguity in the issue;
- the proposed correction crosses the checkpoint's approved surface; or
- reviewers materially disagree.

## After The Last Checkpoint

Return to the main `$issue-delivery` workflow. Run complete issue-owned
validation and a final full integration review of the issue-base-to-current-head
diff before the normal pull-request creation or readiness update. An early
checkpoint-preview draft remains draft until the full issue proof and final
review pass.
