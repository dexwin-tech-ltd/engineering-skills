# Task-Level Pull Request

Use this path only when the user requests publication of completed Quick or
Standard work that has no canonical issue. The user request and compact task
handoff govern scope. Critical work and any explicitly issue-driven work use
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
   Follow the repository's title and PR-template conventions. Include the
   outcome, changed scope, checks actually run, Standard review evidence when
   applicable, and material risks. Link a GitHub issue only when one exists
   and should close on merge. Omit empty boilerplate and sensitive data.
4. Use **Draft** only for explicitly requested work in progress or proof that
   can run only after PR creation. Otherwise mark the PR ready for human review
   when the task-level proof is complete. Pending CI alone does not force Draft.
   Never describe the PR as Ready to Merge until required CI and human gates
   are actually satisfied.
5. Re-read the remote PR and verify repository, URL, base, head, head SHA,
   title, body, and draft state. Report any pending CI or evidence honestly.

This path does not authorize merge, deployment, reviewer assignment, labels,
or unrelated cleanup. It does not let an Implementation Worker or Independent
Reviewer publish; the responsible user-facing agent retains publication
authority and checks the returned evidence.
