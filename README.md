# eng-for-certainty

This repository is automatically synced from [seyofori/skills](https://github.com/seyofori/skills) at source commit `beec9929d849539aa0469d59e2b9e0a5f45d8805`.

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
- `issue-review`
- `grilling`
- `domain-modeling`
- `grill-me`
- `grill-with-docs`

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

Install docs-backed grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill domain-modeling   --skill grill-with-docs
```

Install general plan grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill grill-me
```
