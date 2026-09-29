# Follow-Up Inbox

Use for confirmed discoveries made during authorized implementation or an
explicitly authorized review-to-merge workflow. Respect a governing issue or
user instruction that sets a narrower disposition boundary. A read-only review
may recommend a follow-up but does not write the inbox.

## Decide Whether The Current Work Can Ship

Defer only when the discovery is outside the current issue or task's promised
behavior and acceptance criteria; weakens no required validation, applicable
doctrine or repository gate for the current change, security, permissions,
data integrity, migration safety, or operational reliability; conceals no known
regression; and
leaves the current change independently releasable. Rarity or a Low severity
label alone never establishes eligibility. A material mismatch against an
approved design for the delivered surface belongs to the current issue.

Verify that an asserted doctrine violation applies and has a concrete failure
or credible maintenance consequence before treating it as a finding. Correct an
applicable required rule in the current change; route a material choice through
the issue's decision path. An unrelated existing violation may enter the inbox
when the current work remains safe to ship. Do not create entries for optional
style preferences without a concrete consequence.

## Capture And Triage

At discovery, retain a brief evidence note in the active task and keep delivery
moving. Promptly tell the user about a conspicuous user-visible finding outside
scope, recommend an inbox entry, and continue independent issue work unless the
user redirects it. A finding that cannot yet be shown eligible follows the
normal decision or blocker path.

At closeout, before marking the current work done, re-check eligibility against
the final change and deduplicate by root cause against existing issues and
backlog entries. Link an existing exact owner instead of creating a duplicate.
Use the repository's canonical backlog or intake file and status convention
when it has one. If it has no such file, create `BACKLOG.md` at the repository
root with a `Follow-up inbox` section and use `Inbox` status. Keep each entry
short and discoverable:

- a descriptive title and intake status, without inventing priority;
- the trigger, observable effect or credible maintenance consequence, and
  supporting evidence;
- why the current issue or task is independently releasable; and
- the originating issue, pull request, or task reference.

Link each entry from the originating issue's completion record when one exists
and from the delivery handoff. A backlog entry records work for later triage;
it is not an implementation-ready issue. When selected for work, use
`$issue-review` to prepare the full issue and its implementation contract.
Never claim an entry was filed when the repository could not be written; report
the missing write authority or location and leave the filing step pending.
Do not create an empty backlog file when there are no eligible findings.
