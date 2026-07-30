# eng-for-certainty

This repository is automatically synced from [seyofori/skills](https://github.com/seyofori/skills) at source commit `0281114cd0b4753d12ce4b094e82aba9018561ca`.

Do not edit this repository directly. Make changes in `seyofori/skills` and let the sync workflow publish them here.

## Included skills

- `engineering-for-certainty`
- `engineering-observability`
- `engineering-resilience`
- `engineering-auth-security`
- `engineering-frontend`
- `code-review`
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

Install issue review with its direct dependencies:

```bash
npx skills add seyofori/eng-for-certainty   --skill engineering-for-certainty   --skill grilling   --skill issue-review
```

Install docs-backed grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill domain-modeling   --skill grill-with-docs
```

Install general plan grilling:

```bash
npx skills add seyofori/eng-for-certainty   --skill grilling   --skill grill-me
```
