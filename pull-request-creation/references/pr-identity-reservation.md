# PR Identity Reservation

Use only when the responsible Delivery Operator has publication authority for
an approved task or issue and needs the PR URL for real completion bookkeeping.
Prefer reserving it before implementation or the first expensive publication
cycle, using an existing approved planning or metadata commit. Otherwise use
normal completed-work publication. Read-only review, planning-only work without
publication authority, local-only work, and stop-before-publication boundaries
do not authorize reservation.

## Preconditions

Verify the exact repository, task or issue scope, base, branch, dedicated
worktree or established repository exception, ownership, and intentional diff.
Formal issues retain their approved Branch Contract and Approved Issue Commit;
task-level work uses its approved handoff without inventing a canonical issue.
Preserve Existing PR Correction Contracts and reuse their exact PR.

A new PR needs a real diff from its base. Use genuine approved planning,
bookkeeping, or completed implementation content. Do not manufacture empty or
fake planning commits, extra branches, or unfinished behavior merely to obtain
an identity. All repository gates for publishing that initial head still apply.
If no suitable diff exists, defer reservation rather than fabricating work.

Check repository automation before opening a draft. Reservation does not
satisfy an accepted UI checkpoint or authorize a preview, deployment, or other
gated side effect. If draft creation would cross such a gate, use its approved
workflow or defer reservation. Do not disable automation or apply skip labels
to evade the gate.

## Reserve or reuse

1. Find the PR for the exact head and base before creating one. Return an
   existing identity without duplicating the PR or changing its readiness.
2. For a new identity, stage and commit only the verified intended content,
   run every initial-publication check and normal commit/push hook, and push
   the exact branch. If a check fails, stop dependent publication as usual.
3. Create a **draft** with the truthful planned outcome and scope. State that
   its URL is reserved for bookkeeping; name unfinished implementation,
   validation, review, or approval. Do not claim completion or request review
   of unfinished work. Follow repository draft/label policy.
4. Re-read repository, URL, base, branch, head SHA, and draft state. Return the
   identity and actual initial-publication results to the Delivery Operator.
   Keep an issue's existing readiness state truthful.

The initial push may still be expensive. A draft requires a published branch;
reservation does not bypass hooks or CI and does not guarantee fewer pushes.
Its benefit is enabling known URL-dependent bookkeeping to finish before the
final combined-candidate checks, rather than forcing another cycle afterward.

## Complete the same PR

Finish implementation and known walkthrough, queue, and completion bookkeeping
on the same branch. Use the normal task-level or issue-driven publication path
to validate and review the full final candidate, run required hooks, publish it,
and verify current-head CI. Verify the existing PR's final description and
tracking rather than recreating them.

Remove draft status only when all normal readiness gates pass, unless the user
requires it to remain draft. An identity draft never substitutes for the
separate UI checkpoint-preview contract or its accepted head and staging
approval. Later relevant changes invalidate affected proof and require updated
review under the core correction contract. Merge and deployment retain their
separate authority and repository gates.
