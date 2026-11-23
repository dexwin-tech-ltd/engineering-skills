# Session Continuity

Use this procedure when authorized delivery crosses a handoff, pause, or
session change.

## Record the Resume Point

Use the governing issue, PR, or another existing authorized run record.
Capture the goal and permitted stop, branch and worktree, base and candidate
SHAs, significant decisions and evidence links, last accepted checkpoint or
review, open findings and blockers, and the first authorized next action.
Record durable evidence in its existing home.

## Reconcile Before Acting

1. Read the canonical issue and the authorized record. Confirm the effective
   scope, decision state and publication ceiling. Honor an unresolved
   user-owned decision and any narrower stop, such as draft-only.
2. Use read-only Git inspection to verify the worktree, owner, branch,
   status, base, head and relevant diff. Use the existing provider when
   applicable to re-read the PR base and head, state, checks and approvals.
   Compare these with the recorded evidence inputs, including configuration,
   test paths and environment. Establish who's writing before editing a
   worktree with unexplained or overlapping changes.
3. Carry forward evidence whose inputs still match. If a candidate or
   relevant input changed, rerun affected proof and update the verdict under
   [Review and Corrections](../../engineering-for-certainty/references/rigor-and-verification.md#review-and-corrections).
   Resume at the first permissible action. Escalate competing ownership,
   missing authority, ambiguous state, or an unresolved decision through the
   existing [Pause Handoff](../SKILL.md#pause-handoff).

A handoff records the current state and scope. Publication and goal
transitions still follow the existing authority and
[Goal Persistence](goal-persistence.md) rules.
