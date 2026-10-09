# Goal Persistence

Read when a durable goal is active during issue delivery. The governing issue retains transition authority.

## Goal And Gate Interaction

A durable goal is a persistence mechanism, not transition authority.

When a goal is active:

- continue only while the current delivery state has an authorized next action;
- never change a review verdict or finding route to preserve momentum;
- never treat the goal's stopping condition as permission to change product
  meaning, scope, architecture, contracts, security posture, dependencies, or
  test strategy;
- keep `AUTO_CORRECT` work inside the current checkpoint until its corrected
  head is revalidated and re-reviewed;
- pause and yield on `USER_DECISION` or `BLOCKED`, leaving the goal incomplete;
- do not cross a checkpoint with an unresolved confirmed finding; and
- mark the goal complete only at the completion condition declared by the
  governing issue or explicit user instruction. The default is verified merge
  with required selective human-review tracking and authorized release
  follow-through, not waiting for post-merge human review;
  an explicit narrower publication boundary may define a truthful local
  completion target instead. A publication stop with no local completion target
  leaves the goal incomplete.

An active but incomplete goal may be waiting for the user or an external owner.
It does not require the operator to keep acting when no authorized transition
exists.
