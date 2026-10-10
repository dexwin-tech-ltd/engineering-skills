# Post-Merge Human Review

Use when the human asks for pending reviews, help understanding a merged PR,
or handling their review outcome. Read
[Delivery and Human Review](../../engineering-for-certainty/references/delivery-and-human-review.md)
for authority, selection, indexes, and correction rules.

## Find and orient

For a queue request, inspect root `PRS_PENDING_HUMAN_REVIEW.md` and live merged
PRs with `human-review:pending` in the requested repositories. List brief
descriptions and reasons, consequential items first and then older items.
Report known missing entries and incomplete access. Inspection alone does not
authorize recording outcomes, changing labels, or editing the file.
Include each verified walkthrough link or known publication-pending state.

For a specific PR, verify its repository, merged status, original review range,
head, resulting merge/squash commit, approved intent, and guided description.
Recover the frozen original PR diff through provider evidence or immutable
revision identities. Do not review today's entire main branch or compare a
deleted head to a moving base. Have the delivery operator prepare missing
objects; Independent Reviewers do not fetch or switch branches. If original
evidence cannot be recovered, name the gap instead of inventing the diff.

Lead with a small explanation of the problem and changed behavior. Show one
useful screenshot, before/after example, diagram, or code entry point when it
helps. Let the human choose where to go deeper. The
[PR writing guide](../../pull-request-creation/references/pr-body-writing.md#human-review-after-merge)
sets the compact presentation standard; do not paste a full issue or evidence
ledger into chat.

Prefer the PR's **View feature walkthrough** link for layered orientation.
Verify that it describes the original delivered revision and respects repository
visibility. Use [$feature-walkthrough](../../feature-walkthrough/SKILL.md) when
creation or repair is authorized; a read-only queue/explanation request does not
authorize remote publication. If hosting is pending or unavailable, guide the
human from the PR, original evidence, or an agent-opened local HTML copy and
report the gap. Opening the page or passing agent checks never completes human
review. Keep its archived explanation after queue removal.

Identify known deployment state separately. If current production includes
later changes, explain that inspecting it does not establish the behavior of
this PR's exact merged revision. Retain original evidence and assess current
code separately when verifying whether a reported defect still exists.

## Discuss and verify

Support the human's review rather than treating an agent's clean verdict as a
human outcome. Answer concrete questions with code and evidence. Use
`$code-review` in a distinct read-only Independent Reviewer context when defect
verification or a requested deeper audit requires it; do not automatically run
another complete review merely to orient the human.

Keep new defect claims, design preferences, and missing context distinct. Clear
bounded feedback can start verified correction delivery; material choices need
grilling. A question such as "what does this function do?" authorizes an
explanation, not a repair or queue removal.

## Record the explicit outcome

When the human says review is complete, record their actual outcome on the
original PR under the agent's own identity when acting for them. Include brief
feedback and linked correction/follow-up owners; verify publication. Do not
publish an approval in their name. Then reconcile the pending label and index
through the shared policy. If they only provide partial feedback, keep review
pending and continue answering without repeatedly asking for completion.

Create repairs on new branches from the current verified base and link both
PRs. Verify the old claim against current code before changing it. Preserve
the original merge and review history. The repair ships through normal proof
and is independently assessed for human-review selection. Critical repairs
use the formal issue path; eligible Quick/Standard task repairs can use a bounded
handoff. Bookkeeping-only
index maintenance uses a small isolated PR and is exempt from human review.

Report the original human-review state, feedback recorded, index updates
verified or pending, and separate repair states. A completed review can coexist
with a correction still awaiting a decision, delivery, or human inspection.
