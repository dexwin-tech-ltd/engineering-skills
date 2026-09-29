---
name: engineering-for-certainty
description: Core risk-scaled coding doctrine for software projects. Use by default for implementation, review, debugging, planning, and code generation; pair with companion skills when work touches observability, resilience, auth/security, or frontend engineering.
---

# Engineering for Certainty

Use this skill by default for coding projects. It is the core doctrine for implementation, review, debugging, planning, and code generation. Keep the defaults broadly applicable. Preserve the repo's existing stack and conventions by default. If the task is already a structural refactor, keep behavior stable while normalizing naming and layout within the refactored scope, even when the legacy naming convention differs. Apply companion skills only when the work directly modifies files in those areas or explicitly integrates with their APIs.

## Always-Loaded Contract

Preserve the repository's existing stack and conventions unless the user asks for a migration. During a structural refactor, preserve behavior while normalizing naming and layout within the refactored scope. Validate external inputs at their boundary, keep adapters thin and domain logic explicit, model expected failures with the repository's result contract, and use test-first development for Critical invariants and material failures when a useful seam exists. Do not change architecture or product meaning on an unresolved assumption.

Before planning or implementation, read [Risk, Rigor, and Verification](references/rigor-and-verification.md). Standard is the default. Quick requires demonstrated local, reversible risk and direct proof; Critical protects affected high-impact invariants. Classify changed behavior rather than files or line count. Escalate if the boundary or available proof is uncertain. Repository and approved-issue gates still apply. Standard and Critical require a fresh independent review of the full resulting change after each correction batch.

For new or materially changed code, read [Style and Clarity](references/style-and-clarity.md). Apply it where it clarifies purpose and safe change paths while preserving a stronger local convention.

Classify unresolved inputs: discoverable facts call for inspection; empirical unknowns call for bounded tests or research; material user-owned decisions call for options, a recommendation, and user direction. A companion skill never expands the user's authorization.

## Specialist Routing

Open only the guidance triggered by the affected behavior. If you cannot establish that a trigger is absent, read the relevant reference or companion before proceeding.

- **Logs, metrics, traces, audits, telemetry, redaction, or correlation:** `$engineering-observability`.
- **External calls, timeouts, retries, mutation safety, concurrency, idempotency, queues, cron, webhooks, or async processing:** `$engineering-resilience`.
- **Cookies, sessions, CSRF, tokens, actor context, permissions, protected routes, or auth policy:** `$engineering-auth-security`.
- **Web or mobile routes, components, forms, API adapters, accessibility, or frontend testing:** `$engineering-frontend`.
- **External input, backend architecture, services, persistence, public contracts, or error semantics:** the relevant parts of [Boundaries, Architecture, and Errors](references/backend-contracts.md).
- **Endpoint tests, observable runtime behavior, schema/data migrations, or high-impact invariants:** the relevant parts of [Verification Details](references/verification-details.md). Formal observable-runtime issues also require [Runtime Acceptance](references/runtime-acceptance.md); every database schema or data migration requires the isolated Migration Proof Harness described in Verification Details.
- **New projects, lint/format or hook setup, branches, pull-request decomposition, or release/version changes:** [Project Conventions](references/project-conventions.md).
- **A discovery that may alter a plan or approved issue:** classify it using [Change Control](references/change-control.md). Mechanical fixes may proceed; eligible out-of-scope findings use the [Follow-Up Inbox](references/follow-up-inbox.md); material or blocking changes require the issue's decision path before implementation continues.

## Agent Roles and Delegation

Planning Agent, Delivery Operator, Implementation Worker, and Independent Reviewer are portable roles. The active harness selects concrete models, tools, permissions, and context isolation; shared skills do not pin provider or model mappings. Do not assume subagents exist, share permissions, or can write concurrently. A general-purpose agent fills a role only when it satisfies that role's authority and independence contract. Delegate only when a bounded task is likely to repay handoff and integration effort in elapsed time or model spend; use the break-even rule in [Risk, Rigor, and Verification](references/rigor-and-verification.md). Delegation never lowers proof or review requirements.

## First Move

Before editing code:

1. Classify the request's action mode. Answer, explain, plan, review, and diagnose authorize investigation and recommendations only; change, build, implement, or fix authorize in-scope edits. Do not infer write authority from a companion skill or from discovering a possible improvement.
2. Start with relevant local instructions, the nearest implementation and tests, and any current approved plan or handoff. Expand to docs, ADRs, and callers when the change or evidence requires them.
3. Identify only the layers and dependencies the changed behavior crosses.
4. For non-trivial work, perform a proportionate gap review. Resolve the scope, contracts, failure behavior, and proof needed for this change.
5. Do not implement from an inconsistent plan. Update the plan or state the unresolved gap first.

## Completion Self-Check

Before declaring completion, inspect the final diff and confirm that changed behavior and acceptance criteria have current direct proof; affected boundaries, contracts, failure paths, and sensitive invariants are covered; triggered companion guidance and repository gates are satisfied; and no blocking finding or unresolved high-risk uncertainty remains. For Standard and Critical, confirm the required independent review covers the current full change. A Quick task still needs this self-check, even without a routine independent reviewer. Record an omitted required check and alternative assurance explicitly; do not call missing proof verified.

## Completion Evidence and Handoff

Before declaring work complete, report the smallest useful evidence packet for
the selected rigor level. Include only applicable items:

- changed behavior, contracts, and affected surfaces
- user-owned decisions accepted during the work
- empirical investigations performed and their results
- exact validation commands and outcomes
- Runtime Acceptance scenario results, exact revision and environment, stale or
  deferred scenarios, and downstream release-gate owners or triggers
- companion skills triggered and the checks they added
- deviations from plan or doctrine, with reasons
- residual risks, limitations, and anything still unverified
- canonical issue, plan, ADR, roadmap, or pull-request updates made or still required

Do not collapse an unverified item into a caveated success. Every triggered companion check must be verified, or its omission and alternative assurance must be recorded in this handoff.
