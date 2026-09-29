# Issue Attention and Reading Contract

## Issue Attention And Reading Contract

An issue must be complete without becoming a transcript, raw evidence store, or
copy of reusable engineering doctrine. Optimize for instruction salience: the
Implementation Worker must be able to distinguish approved intent, acceptance
criteria, change authority, and stop conditions from supporting detail.

For every non-trivial child issue, add a concise `Agent Start Here` section near
the top. Keep it to roughly fifteen lines or fewer and include:

- the one observable outcome;
- the exact Branch Contract and pull-request base;
- the acceptance-criterion IDs;
- the Runtime Acceptance environments and any downstream release gate;
- the allowed-change and pause boundaries;
- the review-checkpoint sequence, when present; and
- every linked appendix that must be read before a named checkpoint.

Keep reusable rules in the governing skills and repository documentation. The
issue should name the activated doctrine and record only issue-specific
decisions, contracts, risks, and deviations. Do not copy full engineering
manuals, generic review procedures, or raw test output into each issue.

Use linked appendices only for dense issue-specific material such as large
state-transition tables, error matrices, migration fixtures, or verified
baseline evidence. Critical product decisions, acceptance criteria, authority
boundaries, and pause conditions must remain in the core issue. Every appendix
required for a checkpoint must be named in that checkpoint's reading set.

Keep the Issue Completion Record concise. Record exact commands, outcomes,
finding dispositions, and durable references, but do not paste raw logs,
complete review transcripts, or repeated doctrine into the canonical issue.

Use issue length only as a review signal:

- At roughly 300 lines, run an explicit compression and decomposition check.
- At roughly 500 lines, require the issue-review handoff to explain why the
  child remains one Smallest Coherent Slice and why the remaining material
  cannot be compressed or moved to a linked appendix.
- Do not split an issue solely to satisfy a line count.
- Parent issue packs may be longer, but they must not become the direct
  implementation target for `$issue-delivery`.

For a slice whose risk warrants intermediate independent review, read
[Review Checkpoint Planning](review-checkpoint-planning.md) and
define semantic checkpoints. Otherwise name one delivery unit and use targeted
checks during implementation, followed by the final integration review.
