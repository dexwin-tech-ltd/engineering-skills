# eng-for-certainty

This repository is automatically synced from [seyofori/skills](https://github.com/seyofori/skills) at source commit `acf6aad5e799cbaaa7dd4907f55cc2262fd6e477`.

Do not edit this repository directly. Make changes in `seyofori/skills` and let the sync workflow publish them here.

See [CONTEXT.md](CONTEXT.md) for the shared workflow vocabulary and ownership boundaries.

## Migration

`deliver-issue` was renamed to `issue-delivery`. Update explicit
`$deliver-issue` invocations to `$issue-delivery`, then remove or
replace stale local `deliver-issue` installations.

## Included skills

- `engineering-for-certainty`
- `engineering-observability`
- `engineering-resilience`
- `engineering-effect`
- `engineering-auth-security`
- `engineering-frontend`
- `code-review`
- `pull-request-review`
- `pr-review-and-fix`
- `pull-request-creation`
- `feature-walkthrough`
- `issue-delivery`
- `issue-review`
- `grilling`
- `domain-modeling`
- `grill-me`
- `grill-with-docs`

## Portable agent roles

These skills define four portable responsibilities: Planning Agent,
Delivery Operator, Implementation Worker, and Independent Reviewer.
The active harness chooses the concrete agents, models, providers,
permissions, tools, runtimes, and isolation outside the shared skill
text. When delegation is unavailable, the primary agent may perform
safe planning, delivery, and implementation roles in sequence. A
same-context self-review never satisfies an independent-review gate.

## Automated issue delivery

### 1. Prepare and approve the issue

Invoke `issue-review` with the idea, feature, or existing issue. It
determines from the initial prompt whether implementation follows
immediately, and asks when that intent is unclear. It prepares the
parent pack and Smallest Coherent Slices, including each slice's Branch
Contract, acceptance criteria, traceability, change-control boundaries,
Review Loop Contract, and any semantic review checkpoints.

For immediate delivery, `issue-review` creates or verifies the
selected slice's declared worktree after approval, writes and commits
the final issue there, and hands that same context to `issue-delivery`.
Backlog-only review does not create an idle implementation worktree.

### 2. Let the Delivery Operator run

Invoke `issue-delivery` with the approved issue. It reuses the
`issue-review` worktree for immediate delivery, or creates the
backlog issue's declared worktree from a verified base containing the
approved issue commit. It never starts implementation on a second
branch.

`issue-delivery` coordinates implementation, checkpoint validation,
bounded Implementation Worker assignments, independent review,
authorized corrections, revalidation, final
integration review, pull-request creation or update, and CI
follow-through, then verified merge and the established authorized
release process. Explicit narrower instructions and repository gates
still control.

When the active harness supports durable goals, configure one through
that harness for work that should continue across turns. A durable
goal adds persistence, not authority.

Confirmed deterministic in-scope findings may be corrected and
re-reviewed automatically. Important, significantly cross-cutting,
hard-to-change, or critical-system PRs receive
`human-review:pending` and a root
`PRS_PENDING_HUMAN_REVIEW.md` entry with their link and brief
description. Routine PRs need no human review. The original PR holds
compact guided review material, a rendered HTML walkthrough link, and
the explicit human outcome. `feature-walkthrough` creates the layered,
self-contained page and publishes through repository-configured static
hosting that follows repository visibility. Old PR explanations remain
available after review; temporary tunnels are only immediate previews.
A hosting-only failure stays explicitly publication-pending with a
tracked repair and does not block otherwise-ready feature shipping. The
label and file reflect that outcome. Pending human review alone does
not block completed delivery or later shipping.

### 3. Handle human decision gates

Human input is required when delivery encounters product or domain
meaning, scope, architecture, public contracts, schemas, migrations,
permissions, security policy, dependencies, test strategy, material
reviewer disagreement, missing context, or an external blocker.

A durable goal supplies persistence, not authority. It cannot broaden
automatic correction authority or cross an unresolved decision,
blocker, or confirmed finding.

## Review a pull request through merge

Invoke `pull-request-review` with an explicit review-to-merge request,
for example: `Take PR #123 through review-to-merge.` A generic pull
request review remains non-merging.

Authorized delivery can also pass its current-head evidence to this
merge workflow without another per-task merge request. For human
review after merge, ask `pull-request-review` for the pending queue
or help reviewing a merged PR. Clear bounded feedback starts a new
linked correction PR; it never changes the merged PR's old branch.

The workflow verifies the complete finding queue before changing the
pull request. Deterministic in-scope corrections pass through
`issue-review` and `issue-delivery`; user-owned decisions pause for
grilling. Eligible nonblocking findings become issue-review-ready files
and canonical roadmap entries on the pull-request branch. The updated
head is revalidated and re-reviewed before merge.

Merge requires the exact reviewed head, green required checks, current
issue evidence, satisfied review policy, and no unresolved blocker.
After verified merge, the workflow deletes the exact remote head and
clean local branch/worktree only when dependency and data-loss guards
pass. It never bypasses protection or force-deletes.

## Install

Install the skill family:

```bash
npx skills add seyofori/eng-for-certainty
```

Install a companion with its core dependency:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-resilience
```

Replace `engineering-resilience` with `engineering-observability`, `engineering-auth-security`, `engineering-frontend`, or `engineering-effect` for another companion bundle.

Install code review with its complete engineering doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-effect   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review
```

Install issue review with its complete engineering doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-effect   --skill engineering-auth-security   --skill engineering-frontend   --skill grilling   --skill issue-review
```

Issue review requires every companion triggered by the issue. The
complete bundle above prevents a frontend, auth, resilience, or
observability issue from being reviewed without its governing doctrine.

Install pull request review with the complete review-to-merge doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-effect   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review   --skill grilling   --skill issue-review   --skill pull-request-creation   --skill feature-walkthrough   --skill issue-delivery   --skill pull-request-review
```

Install autonomous PR review and correction with its complete doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-effect   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review   --skill pull-request-review   --skill pull-request-creation   --skill feature-walkthrough   --skill issue-review   --skill issue-delivery   --skill grilling   --skill pr-review-and-fix
```

This workflow follows repository AGENTS.md label rules, records human
decisions in the PR, and routes decision requests to the verified
github-review-requested Slack channel. It normally continues through
verified merge under repository gates; an explicit reviewer-
reassessment boundary still stops before merge.

Install pull request creation:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill feature-walkthrough   --skill pull-request-creation
```

Install the delivery operator with its implementation, review, and
merge doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-effect   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review   --skill grilling   --skill issue-review   --skill pull-request-creation   --skill feature-walkthrough   --skill pull-request-review   --skill issue-delivery
```

Install docs-backed grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill domain-modeling   --skill grill-with-docs
```

Install general plan grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill grill-me
```
