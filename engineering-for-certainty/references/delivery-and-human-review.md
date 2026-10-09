# Delivery and Human Review

Read for authorized implementation or delivery, merge coordination, and human
review after merge. This policy applies to formal issues and task-level work.

## Authority and shipping

An authorized implementation, delivery, or fix request includes intentional
commits, PR publication, CI follow-through, and merge of the approved scope by
default. An explicit narrower task or issue boundary, such as local edits,
draft PR only, reviewer reassessment only, or stop before merge, controls.
A clear local-only implementation request defines a local completion target;
do not treat it as permission for remote publication. Honor any separate
no-commit instruction. Reading a skill and requests to explain, investigate,
plan, or review do not grant delivery authority. Implementation Workers and Independent Reviewers
retain their bounded roles; the responsible Delivery Operator owns shipping.

Before merge, preserve the selected rigor, required independent agent review,
runtime and design proof, current-head CI, and repository gates. Use
[$pull-request-review's merge gates](../../pull-request-review/references/review-to-merge.md#merge-gates)
for the exact candidate. Reuse valid current-head evidence; do not commission
another full review merely to transfer between skills. New or invalidated proof
still requires the normal revalidation and independent review.

Pause for unresolved material decisions, explicit human gates, missing required
proof, known blocking defects, or consequences that cannot be adequately
contained or recovered. A sensitive but approved, bounded, recoverable change
can use post-merge human review after its Critical proof passes; Critical alone
does not add a blanket human merge gate. Do not waive required reviewer
approvals, change repository protections, impersonate an approver, or bypass
checks. A post-merge queue is not a substitute for safe shipping.

After verified merge, continue the established, already-authorized release
process without a new human-review gate. A repository that requires a separate
production-release decision still requires it. Report merged, deployed,
release-pending, or failed release accurately, with revision and environment.
Human review is a separate state and does not keep completed delivery open.

## Select PRs for human review

Queue a PR when any of these applies to its actual behavior or consequences:

- important product behavior, a consequential engineering choice, or a public contract;
- significant effects across components, domains, or shared infrastructure;
- architecture, data, or external commitments that will be hard to change later;
- critical behavior such as access control, money, persisted data, migrations,
  destructive operations, or production reliability.

Explain the selection briefly in the PR. Routine, isolated, easily reversible
work can finish without human review. A file name, line count, or rigor label
alone is insufficient: copy on an auth screen need not qualify. Queue uncertain
importance; pause uncertain shipping safety. Honor an explicit request for
human review even when the selection criteria would otherwise exempt the PR.
Bookkeeping-only queue maintenance is exempt; a repair is assessed normally.

## Prepare the queue before merge

For each selected PR, the publishing operator must:

1. Prepare the compact guided description using
   [the PR writing guide](../../pull-request-creation/references/pr-body-writing.md#human-review-after-merge).
2. Apply and read back `human-review:pending`. Creating this exact label when
   absent is part of authorized delivery; preserve unrelated labels and any
   repository reviewer-reassessment labels.
3. Once the PR URL exists, add one entry keyed by that URL to root
   `PRS_PENDING_HUMAN_REVIEW.md` on the PR branch. Create the file if absent;
   include a brief description, selection reason, and useful review focus.
   Preserve other entries. The entry reaches the base with the original merge,
   not through a separate post-merge enrollment PR.
4. Include the bookkeeping commit in final current-head verification and CI.
   Verify the description, label, and entry before merge. If permissions or
   repository rules prevent required tracking, report the exact blocker rather
   than silently exempting important work.

Example entry (replace the illustrative URL):

```markdown
- [#142 — Centralize permission checks](https://github.com/OWNER/REPO/pull/142)
  Changes authorization across API routes. Review focus: access boundaries
  and compatibility.
```

The merged pending queue is `is:pr is:merged label:"human-review:pending"`,
scoped to the chosen repositories. The file on a PR branch is preparatory;
only verified merged entries belong to the post-merge queue. After merge,
read back the provider's merged state and resulting commit, and verify the
entry on the actual base branch. Record those identities and known release
status in the PR without inventing deployment evidence. A failure in this
post-merge reconciliation is incomplete tracking, not an unmerged PR; repair
it with an isolated bookkeeping PR and report the gap.

## Human outcomes and reconciliation

The explicit human outcome recorded on the original PR is authoritative. The
label and Markdown file are indexes of that outcome, not proof of review.
Never clear pending status because an agent reviewed, a PR merged, a release
succeeded, time passed, or one index entry disappeared.

When the human explicitly finishes review, record acceptance, corrections
requested, or discussion needed, along with feedback and linked follow-up
owners. A partial question or comment does not finish review. An authorized
agent may record the human's explicit chat instruction in a PR comment under
its own identity, identifying the source; never submit an approval as the human
or infer completion from the agent's own conclusion.

Verify that outcome, remove only `human-review:pending`, and remove only that
PR's file entry through a small isolated maintenance PR under normal checks
and merge rules. Keep corrections tracked separately even when human review
is complete. Re-read both indexes after their mutations. On interruption,
report which updates remain; later reconciliation resumes from the recorded
outcome without duplicate comments or unrelated deletions. Resolve label/file
disagreement using PR history and explicit human outcomes. If outcome evidence
is missing, retain or restore pending status for selected merged work.

When asked for the queue, check live merged PRs and the file, report known
discrepancies, and present consequential items first, then older items. Repair
indexes only with authorized reconciliation, not from a read-only queue
request. Flag known omissions; do not claim a stale file is a complete live queue. Queue size
or age alone never blocks new shipping, expires an item, or counts as acceptance.
This policy does not create a schedule, watcher, or external notification.

## Corrections after merge

Clear bounded human feedback authorizes investigation and delivery of a
confirmed correction within the original approved intent, without another
ceremonial approval. Verify the claim before editing. Use a new branch and PR
from a current verified base; link the original PR and feedback both ways.
Never push a fix onto a merged head or rewrite its history. An eligible Quick
or Standard task-level correction can use a bounded handoff; Critical and
formal issue work use the formal issue planning and completion gates.
Mechanical capture of established intent and
verified findings is authorized; new scope or design choices are not.

Corrections get the normal risk classification, validation, independent
review, CI, merge gates, and human-review selection. Use grilling for material
product or architectural choices; capture out-of-scope improvements through
the Follow-Up Inbox. Urgent recovery uses established incident/recovery
authority; escalate actions beyond it. Completing the original human review
does not imply that its repairs are delivered.
