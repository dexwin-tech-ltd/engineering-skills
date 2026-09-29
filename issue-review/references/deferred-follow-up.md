# Deferred Follow-Up Inbox Mode

Use when authorized `$issue-delivery` or review-to-merge supplies confirmed
findings routed `DEFER_FOLLOW_UP`. This is a focused planning write under
`$engineering-for-certainty`'s [Follow-Up Inbox](../../engineering-for-certainty/references/follow-up-inbox.md)
contract, not full issue preparation.

- Re-verify each finding against the current issue, current head, applicable
  doctrine, and all deferral criteria. Rarity or Low severity does not suffice.
- Search the repository's existing issues and backlog for the root cause. Link
  an exact existing owner rather than creating a duplicate.
- Group new findings by root cause. Use the repository's canonical backlog or
  intake file and status convention. If none exists, create `BACKLOG.md` at the
  repository root with a `Follow-up inbox` section and use `Inbox` status so a
  human has a predictable place to triage.
- Give each entry a descriptive title, intake status, trigger, observable
  effect or credible maintenance consequence, supporting evidence, why the
  current change remains releasable, and the originating issue or pull request.
  Do not invent priority, product meaning, or an implementation plan.
- Write all new entries and origin links in one coherent pass. Return their
  paths and stable headings to the composing workflow for the completion
  record, pull-request description when applicable, publication, and current-
  head revalidation.
- Make no no-op edit or commit when an existing entry already covers the
  finding and its origin and evidence are current.

When a follow-up is selected for work, return to the normal `$issue-review`
workflow to create or revise one implementation-ready Smallest Coherent Slice
with acceptance criteria, proof, and a Branch Contract. Inbox status alone
never authorizes implementation.

If the verified branch cannot receive the repository-local backlog write,
return that exact gap. A chat note, pull-request comment, external tracker item,
or TODO does not satisfy the inbox filing requirement.
