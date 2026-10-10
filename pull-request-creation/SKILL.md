---
name: pull-request-creation
description: Verify completed task-level or issue-driven work, publish intended commits, and create or update a truthful GitHub pull request. Use when the user asks to turn completed local work into a reviewable PR.
---

# Pull Request Creation

Load and follow `$engineering-for-certainty`. Convert completed, verified local work into a truthful GitHub handoff. Own branch verification, intentional publication, PR creation or update, readiness state, selective human-review enrollment, and remote verification.

Read [Delivery and Human Review](../engineering-for-certainty/references/delivery-and-human-review.md).
A standalone request to create a PR stops at that requested boundary. Inside
authorized delivery, return current-head evidence to the owning operator for
CI and verified merge; do not introduce a routine human approval stop.

For a completed Quick or Standard task without a canonical issue, read and
follow [Task-Level Pull Request](references/task-level-pr.md) instead of the
issue-specific preconditions and workflow below. Critical or explicitly
issue-driven work stays on the formal issue path. Do not demand a new issue
solely to publish an eligible task-level change.

For the formal issue path, accept a publication handoff only from the Delivery
Operator named by the governing workflow. An Implementation Worker or
Independent Reviewer cannot bypass that operator or turn its narrower
assignment into publication authority.

Do not implement missing feature work, perform code review, repair CI, invent evidence,
or broaden scope. Stop when the work is not ready unless the user explicitly
authorizes a work-in-progress PR or the approved issue's UI checkpoint-preview
plan requires the narrow draft path below.

## Preconditions

Resolve and read:

- the canonical issue or implementation handoff;
- any issue-approved Existing PR Correction Contract when publication updates
  an already-open pull request;
- its stable issue number and exact Branch Contract, including the declared
  pull-request base ref and worktree isolation mode;
- the repository's branch, commit, PR-title, PR-template, and release conventions;
- the intended base branch or preceding stacked-PR branch;
- the pre-work execution handoff's runtime worktree path, resolved base SHA, and
  Approved Issue Commit SHA;
- the complete local diff, commits, untracked files, and validation evidence;
- the traceability ledger, Runtime Acceptance Plan and current scenario evidence
  when observable runtime behaviour changed, and final independent review
  evidence.
- for design-backed frontend work, the approved Design Reference Manifest,
  resolvable Evidence Bundle, complete classified Design Audit Matrix,
  source-drift result, and independent Design Audit evidence.

If no canonical issue exists, ask for the issue identity before publishing. Distinguish the repository's local numbered issue from a GitHub issue number; never invent `Closes #N` from a local filename.

### UI Checkpoint Preview Draft

`$issue-delivery` may invoke this skill before full issue completion only when
an accepted UI-first checkpoint needs a pull request to create its pre-merge
preview. Require the approved issue's checkpoint-preview plan, the Delivery
Operator's publication authority, the accepted frontend head SHA, its current
validation and independent checkpoint review, and evidence for every
checkpoint-owned acceptance and design row. Verify the Branch Contract and
worktree as usual. The current branch head must equal the accepted checkpoint
head; do not add behavior-changing commits during this publication. Do not use
this path to excuse missing frontend checkpoint proof or a narrower publication
boundary.

For this narrow draft, apply the completion-evidence checks below to the
accepted frontend checkpoint rather than claiming full-issue completion. Keep
the issue `Needs Verification`; state the pending backend integration, final
review, and staging approval plainly in the draft pull request. Push the exact
accepted head, verify the remote PR state, and return it to `$issue-delivery`
to verify the resulting preview build, review its scenarios, and obtain user
approval. If the user requests UI changes, update this same draft only after
the corrected checkpoint passes
validation and independent re-review. On the later normal invocation, require
the complete issue evidence and final review before marking it ready.

## Workflow

### 1. Verify Scope And Branch

Inspect repository status and `git worktree list --porcelain` before staging or
creating a branch. Resolve the default branch and upstream without fetching,
pulling, switching, or rewriting history unless that operation is necessary for
the requested publication and authorized.

Require the issue's exact conventional branch name:

```text
<type>/<NN>-<short-kebab-description>
```

Prepend the exact platform-required prefix when the active harness defines one;
shared skill text does not choose that prefix.

If issue-driven completed changes are still on the default branch or any branch
that conflicts with the Branch Contract, stop rather than creating the recorded
branch as a late publication repair. Reconcile the approved issue and
implementation into their declared branch and worktree through the owning
workflow before publication.

For an explicit review-to-merge correction of an existing pull request, accept
the issue's **Existing PR Correction Contract** as a narrow exception to the
normal conventional branch name. Verify the exact repository, pull-request
number, head owner and branch, base branch, expected pre-correction head SHA,
push authority, and dedicated worktree. Update only that pull request; do not
rename its branch or open a replacement pull request. Every other branch and
worktree gate still applies.

Verify that the branch descends from the Branch Contract's declared base and
that the recorded resolved base SHA was truthful when implementation began.
Independent slices use the verified canonical branch; stacked slices use the
preceding pull-request branch. Do not substitute `origin/main` or another
default branch for a stacked base, and do not call a remote-tracking ref current
without fetching or otherwise verifying it when that action is necessary and
authorized.

When the Branch Contract requires a dedicated worktree, verify the pre-work
execution handoff's runtime path and confirm the implementation branch belongs
to a linked worktree rather than the shared checkout. Treat implementation in
the shared checkout without a recorded repository convention or explicit user
direction as a Branch Contract deviation and stop to reconcile it before
publication. The runtime path may differ across machines; never store it in the
canonical issue.

Verify that the Approved Issue Commit is an ancestor of the current head and
that the latest canonical issue, implementation, later issue updates, and
completion evidence all belong to this branch. For immediate review-and-
delivery, confirm the issue commit predates production-code commits and the
worktree matches the `$issue-review` handoff. Under an Existing PR Correction
Contract, require it to predate newly authorized correction commits rather than
the pull request's existing implementation. For backlog work, confirm the
verified base contains the approved issue revision. Stop on missing ancestry or
split branch ownership; do not open a second pull request to join the pieces.

Inspect every changed and untracked file. Reject unrelated work, unexplained generated artifacts, accidental binaries, stale planning changes, or ambiguous ownership. Never default to staging an entire mixed worktree.

### 2. Verify Completion Evidence

Confirm that:

- every acceptance criterion maps to production code and exact evidence;
- every Implementation Worker result was returned to and inspected by the
  Delivery Operator;
- the combined canonical branch, not only helper branches, passed required validation;
- the Independent Reviewer completed the final review and accepted findings
  were fixed and re-reviewed;
- triggered doctrine for migrations, auth/security, resilience, observability, and frontend work is satisfied;
- the Migration Proof Harness evidence exists when a database migration is present;
- material deviations are reflected in the canonical issue;
- no required evidence is represented by a placeholder, caveat, or unverified claim;
- every required issue-owned Runtime Acceptance scenario passed against the
  exact behavior-changing reviewed commit or deployed build, every invalidated
  scenario was rerun, and applicable auth and Design Conformance evidence is
  secret-free and complete. When the current head is later, verify that every
  intervening commit changes only canonical issue, roadmap, backlog,
  human-review index, feature-walkthrough explanation, or completion-evidence surfaces and invalidates no
  recorded proof.
- for design-backed frontend work, the authoritative source was rechecked
  against the approved baseline; the frozen images and HTML/Tailwind reference
  are resolvable and validated; every required matrix row has a current typed
  outcome; no unresolved design drift, design conflict, export defect, or
  evidence gap remains; and the required independent Design Audit belongs to
  the exact candidate.

If remote-only validation or a pull-request-created preview Runtime Acceptance
Pass is the only remaining evidence, leave the issue `Needs Verification` and
use draft readiness until that evidence and the independent final audit succeed.
A post-merge-only staging pass may remain a linked downstream release gate with
an owner and trigger; it blocks release, not issue-owned publication proof.

### 3. Resolve Stack Position

Use a Stacked Pull Request only when the issue records a real dependency. Verify the parent issue pack, preceding PR, head and base branches, merge order, and rebase or retarget procedure.

Independent slices must target the canonical base branch directly. Do not create a stack merely because several PRs belong to the same feature.

### 4. Commit And Push Intentionally

- Stage only the confirmed files or hunks.
- Preserve coherent existing commits; do not squash or rewrite them without authorization.
- Create a Conventional Commit only when intentional uncommitted work remains in scope.
- Include rationale, migration notes, validation, and issue references in the body when the subject is insufficient.
- Run the required pre-publication checks after the final staged or committed state.
- Push the exact branch with upstream tracking.

Stop on failed checks, rejected pushes, remote divergence, or ambiguous credentials. Do not force-push unless the user explicitly authorizes the exact rewrite.

### 5. Create Or Update The Pull Request

Prefer the configured GitHub connector after the branch is pushed; use authenticated GitHub CLI fallback only when the connector cannot represent the repository or stack correctly. Update an existing PR for the same head branch rather than opening a duplicate.

Use a conventional title that preserves issue identity, for example:

```text
feat(learning): issue 08 - persist draft answers
```

Follow a stricter compatible repository convention when present. Include `Closes #123` only for an actual GitHub issue intended to close on merge.

Apply [PR Body Guidance](#pr-body-guidance) while preserving required repository
template fields and the applicable evidence below. Read the local
[PR Body Writing Guide](references/pr-body-writing.md) when composing or
revising the description.

Write a conditional PR body containing only applicable sections:

1. Canonical local issue and GitHub issue link.
2. Problem and intended outcome.
3. What changed.
4. Explicitly excluded scope.
5. Important design decisions.
6. Exact automated validation and Runtime Acceptance results, including the
   tested revision and local, preview, or staging environment.
7. Migration proof evidence.
8. Security, privacy, resilience, and observability effects.
9. UI screenshots, the issue-owned Design Evidence Bundle and Design Audit
   result when applicable, and accessibility evidence.
10. Risks and rollback or recovery path.
11. Stack position and dependencies.
12. Deliberately deferred follow-up work.

Do not paste empty boilerplate, claim checks that did not run, or hide unresolved work in a completed-looking checklist.

### 6. Derive Pull Request Readiness

Choose exactly one outcome from evidence:

- **Do not create:** material implementation or evidence is incomplete and
  neither user-authorized WIP publication nor the approved UI
  checkpoint-preview path applies.
- **Draft:** the user explicitly requested a draft boundary or WIP publication, required evidence
  can only run after PR creation, or the approved UI checkpoint-preview path
  applies. Missing required issue proof keeps the issue `Needs Verification`;
  an intentionally retained draft alone does not make verified proof missing.
- **Ready for review:** the issue is verified complete, the traceability ledger,
  current Runtime Acceptance evidence, and final review are satisfied, accepted
  findings are fixed, every triggered Design Audit is current and complete, and
  no known blocker remains.

Pending GitHub CI alone does not make a completed PR a draft. When draft status exists only to obtain remote evidence, verify that evidence, complete the final audit, and mark the PR ready when every gate passes. Preserve an explicit draft-only boundary even when all proof passes.

### 7. Enroll Selected PRs And Verify Remote State

Assess the actual final change for post-merge human review. For selected work,
invoke [$feature-walkthrough](../feature-walkthrough/SKILL.md) to prepare and
browser-check the HTML explanation from the verified handoff, publish through
configured hosting, and place **View feature walkthrough** prominently in the
description. Keep a compact summary beside the link. This presentation work
does not authorize changing the feature or inventing proof. Include artifact
commits in required current-head checks and independent review. A hosting-only
failure leaves explicit publication-pending status and a tracked repair under
Delivery and Human Review; it does not alone force Draft or block shipping.
Verify `human-review:pending`, and add
the PR URL and brief description to root `PRS_PENDING_HUMAN_REVIEW.md` on this
branch before merge. Follow Delivery and Human Review for exact-label creation,
selection reasons, isolation, and reconciliation. Once the URL exists, publish
the bookkeeping commit to this same PR; rerun invalidated proof, obtain required
current-head verification, and follow CI on the resulting head. Keep the last
behavior-changing reviewed revision distinct from bookkeeping descendants.
Routine PRs and bookkeeping-only maintenance need no human-review entry.
Include the verified rendered walkthrough URL or its explicit pending status
in the queue entry. Preserve the final delivered explanation after merge and
human-review completion; a source-file link is only a labeled fallback.

Re-read the PR and verify the repository, number, URL, base, head, head SHA, title, body, draft state, issue links, and stack dependency. Confirm the remote branch contains the intended local commit.

Report the PR URL, readiness, branch and base, commits published, validation evidence, stack position, human-review selection and tracking, and anything still unverified. Never claim publication succeeded from a local push or mutation response alone.

## PR Body Guidance

Lead with the concrete problem and resulting behavior. Use the project's domain
terms, consulting its glossary when available. Scale detail to the change;
simple PRs may need only a short explanation and relevant validation.

Choose the smallest explanation that makes the change clear. Use a sketch when
it adds clarity; show a full block when a diff would hide important context.
Keep sketches beside the text they explain and verify them against the final
diff. Format-selection rules and original examples live in the local
[PR Body Writing Guide](references/pr-body-writing.md).

For behavior changes, pair observed before and after evidence when available.
Use screenshots for visible differences and actual test results or execution
output for runtime behavior. Distinguish an explanation of expected behavior
from an observed result. If the earlier state was not captured, say so when
material; never invent a failing run or screenshot. Reuse the verified handoff's
evidence and retain its revision and environment. Presentation does not replace
required proof or authorize new implementation work.

For material risks, name the affected users, consumers, data, or systems and
explain reversibility: what rollback restores, what it cannot restore, and any
backup, recovery, or roll-forward requirement. Keep these claims grounded in
the change and existing evidence. Avoid vague risk labels or speculative lists.

All required PR-writing guidance is maintained in this skill and its local
reference files. The source links below record historical inspiration only;
using this workflow does not require fetching or loading those external skills.
Upstream changes do not change our instructions unless deliberately adopted.

Attribution: adapted from [Matt Pocock's PR skill](https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/SKILL.md),
which credits [Dex Horthy's show-me skill](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md)
for its visual explanation approach.

## Write Safety

- Treat mixed worktrees, branch mismatches, missing issues, and incomplete evidence as stop conditions.
- Never stage unrelated user changes or use destructive Git recovery.
- Never publish secrets, private fixtures, raw logs, or sensitive local paths in the PR body.
- The owning delivery operator coordinates merge under Delivery and Human Review;
  PR creation alone does not perform it. Selective human-review label and index
  writes are part of authorized delivery. Other label changes, reviewer requests,
  assignments, and auto-merge require their own authorization.
