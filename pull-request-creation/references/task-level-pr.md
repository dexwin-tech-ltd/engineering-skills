# Task-Level Pull Request

Use this path for publication of completed Quick or Standard work that has no
canonical issue, either directly requested or part of authorized delivery. The
user request and compact task handoff govern scope. Critical work and any explicitly issue-driven work use
the formal issue path in `SKILL.md` instead. Do not create a ceremonial issue
merely to publish an eligible task-level change.

## Preconditions

The responsible agent supplies the requested outcome, changed files, risk level
and reason, final diff, exact checks and results, known limitations, and
accepted decisions. For Standard, require a current independent `$code-review`
of the complete change and a fresh full review after every correction batch.
For Quick, require direct targeted proof and final diff inspection; no routine
independent model review is required. If the change has outgrown its level,
stop and escalate before publication. A task-level PR never claims formal issue
traceability or Ready-to-Merge Handoff status it has not earned.

Verify current repository instructions, status, untracked files, commits,
branch, upstream, intended base, and any open PR for the head. Identify every
file and hunk to publish. Preserve unrelated work. Follow the repository's
branch and worktree convention; where none exists, choose a coherent branch
from a verified base before publication. A platform-required prefix applies.
Do not claim a remote-tracking base is current without verifying it when that
matters to the publication. Stop on ambiguous mixed work, branch ownership,
base divergence, or missing required proof. Do not force-push or rewrite
history without explicit authorization.

## Publish

1. Stage only the intended files or hunks. Keep coherent existing commits;
   create a conventional commit for intentional uncommitted work when needed.
2. Run any required pre-publication checks against the final staged or
   committed state. Push the exact branch with upstream tracking. Stop on a
   failed check, rejected push, or ambiguous credentials.
3. Create or update the PR for the same head branch. Use the configured GitHub
   connector when it can represent the change; otherwise use authenticated CLI.
   Follow the repository's title and PR-template conventions and the shared
   [PR Body Guidance](../SKILL.md#pr-body-guidance). Read the local
   [PR Body Writing Guide](pr-body-writing.md) when composing or revising the
   description. Include the
   outcome, changed scope, checks actually run, Standard review evidence when
   applicable, and material risks. Link a GitHub issue only when one exists
   and should close on merge. Omit empty boilerplate and sensitive data.
4. Use **Draft** for an explicit draft-only boundary, explicitly requested work
   in progress, or proof that
   can run only after PR creation. Otherwise mark the PR non-draft
   when the task-level proof is complete. Pending CI alone does not force Draft.
   Never describe the PR as Ready to Merge until required CI and human gates
   are actually satisfied.
   Preserve a requested draft even after all checks pass; do not mark it ready
   or merge it without authorization to advance that boundary.
5. Re-read the remote PR and verify repository, URL, base, head, head SHA,
   title, body, and draft state. Report any pending CI or evidence honestly.
6. Apply the main skill's selective human-review enrollment step to important
   work. Include any queue-file commit in current-head verification and CI.
   Return the exact proof and compact task handoff to the owning delivery
   operator for merge-gate verification; do not require a formal issue merely
   to merge eligible task-level work.

The standalone PR-creation boundary does not include merge. Normal authorized
delivery does, under [Delivery and Human Review](../../engineering-for-certainty/references/delivery-and-human-review.md).
It does not grant new deployment authority, reviewer assignment, or unrelated
cleanup. It does not let an Implementation Worker or Independent Reviewer
publish; the responsible user-facing operator retains shipping authority and
checks the returned evidence.
