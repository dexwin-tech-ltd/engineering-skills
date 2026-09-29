# Deferred Follow-Up Issue Mode

### Deferred Follow-Up Issue Mode

When an explicitly authorized pull-request review supplies findings routed
`DEFER_FOLLOW_UP`, create durable planning artifacts without changing the
implementation:

- Re-verify that every supplied finding satisfies the composing workflow's
  contract-based deferral criteria. Do not infer deferral from Low severity.
- Search existing issue files, roadmap entries, and completed-work archives.
  Reuse or update an exact existing issue instead of creating a duplicate.
- Deduplicate new findings by root cause and create one **Smallest Coherent
  Slice** per independently implementable outcome, not one file per review
  comment or one catch-all cleanup issue.
- Make each issue implementation-ready under this skill. When later
  implementation depends on product research, create a bounded discovery or
  decision issue with an exact evidence outcome rather than a vague issue or
  placeholder.
- Follow the repository's canonical filename, issue directory, roadmap or
  index, and backlog status. Do not invent priority or silently create a new
  planning system.
- Record the reviewed pull request as a dependency and choose the eventual
  Branch Contract against the repository's canonical post-merge base unless a
  verified stack requires another base.
- Write the complete issue files and roadmap or index update in one coherent
  pass, then return their paths and stable identities to the composing
  pull-request workflow for commit, push, PR-description update, and
  current-head revalidation.
- When an exact existing issue and roadmap entry already satisfy the finding,
  verify and return them without manufacturing a no-op file change or commit.

If no canonical planning surface exists or the supplied branch cannot receive
the planning files, stop and return that exact gap. Do not substitute a chat
note, pull-request comment, external tracker item, TODO, or invented directory.
