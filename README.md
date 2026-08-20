# eng-for-certainty

This repository is automatically synced from [seyofori/skills](https://github.com/seyofori/skills) at source commit `d4bbff73dec6b9565f23f3d933b7a80e87d99f4f`.

Do not edit this repository directly. Make changes in `seyofori/skills` and let the sync workflow publish them here.

See [CONTEXT.md](CONTEXT.md) for the shared workflow vocabulary and ownership boundaries.

## Included skills

- `engineering-for-certainty`
- `engineering-observability`
- `engineering-resilience`
- `engineering-auth-security`
- `engineering-frontend`
- `code-review`
- `pull-request-review`
- `pull-request-creation`
- `deliver-issue`
- `issue-review`
- `grilling`
- `domain-modeling`
- `grill-me`
- `grill-with-docs`

## Recommended issue delivery workflow

1. Use `issue-review` to turn the idea or feature into an approved
   parent pack and one or more Smallest Coherent Slices.
2. Give every child slice one exact Branch Contract, one pull request,
   testable acceptance criteria, traceability, change-control
   boundaries, and a Review Loop Contract.
3. For a substantial child slice, define two to five semantic review
   checkpoints. Each checkpoint must be behavior-complete, green,
   independently reviewable, and tied to owned acceptance and
   validation evidence.
4. Start delivery with a durable goal when the work should continue
   across turns:

   `/goal Use $deliver-issue to deliver <issue path> to a ready-to-merge handoff.`

5. `deliver-issue` implements, validates, and independently reviews
   each checkpoint. Confirmed deterministic in-scope findings may be
   corrected and re-reviewed automatically.
6. Product meaning, scope, architecture, contracts, schemas,
   migrations, permissions, security policy, dependencies, test
   strategy, material reviewer disagreement, and missing context
   return to the user.
7. After all checkpoints pass, run complete issue-owned validation and
   a final full integration review.
8. Create or update the pull request, follow required CI to a terminal
   state, and stop when only human approval and merge remain.

A durable goal supplies persistence, not authority. It never permits
the delivery operator to suppress an adverse finding, change its
route, broaden automatic correction authority, or advance through a
non-clean checkpoint.

## Install

Install the skill family:

```bash
npx skills add seyofori/eng-for-certainty
```

Install a companion with its core dependency:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-resilience
```

Replace `engineering-resilience` with `engineering-observability`, `engineering-auth-security`, or `engineering-frontend` for another companion bundle.

Install code review with its complete engineering doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review
```

Install issue review with its direct dependencies:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill grilling   --skill issue-review
```

Install pull request review with the complete review doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review   --skill pull-request-review
```

Install pull request creation:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill pull-request-creation
```

Install the delivery operator with its implementation, review, and
publication doctrine:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill engineering-observability   --skill engineering-resilience   --skill engineering-auth-security   --skill engineering-frontend   --skill code-review   --skill pull-request-creation   --skill deliver-issue
```

Install docs-backed grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill domain-modeling   --skill grill-with-docs
```

Install general plan grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill grill-me
```
