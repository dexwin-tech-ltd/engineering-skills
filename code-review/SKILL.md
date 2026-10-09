---
name: code-review
description: Perform evidence-verified, staff-level code reviews for diffs, pull requests, commits, branches, local changes, or specific files. Apply engineering-for-certainty as the governing doctrine and require relevant engineering companion skills for auth/security, resilience, observability, and frontend changes.
---

# Code Review

## Objective

Act as the **Independent Reviewer** for the assigned review. The review context
must be read-only and, when the governing workflow requires independence, must
not be the context that implemented the candidate.

Find material engineering risks with high recall, then independently refute or verify every candidate before reporting it. Prioritize correctness, security, data integrity, reliability, and operational safety; treat material style and clarity risks as a required review angle, not aesthetic feedback. Keep ordinary reviews read-only.

Treat review assurance and execution mechanism as separate concerns. Low,
Medium, High, and Max describe the required depth and assurance. Managed
orchestration, parallel isolated review contexts, sequential clean contexts,
and separated passes inside one Independent Reviewer context describe how that
assurance is pursued.

Treat parallelism as an execution technique, not as a review angle. Use logically independent finder and verifier contexts when the runtime supports them and the review surface provides useful independent work packets. Scale finder count by effective diff size, risk, and change topology rather than creating one worker per review angle.

Use a current issue or task handoff for intent, changed files, prior checks,
known risks, and review range. Verify its claims against the raw diff and
governing contracts rather than repeating the implementer's repository survey.
For Standard work, one meaningful independent review is the default; add
finder or verifier contexts only when risk, scope, or disputed evidence makes
their extra cost useful. A task-level Quick change does not invoke this skill
routinely. Critical work receives the deeper independent scrutiny its affected
invariants require.

Keep finder prompts independent. Give each finder raw artifacts, governing contracts, and a bounded review surface without other finders' conclusions. Give verifiers a normalized candidate claim and raw evidence without the finder's preferred verdict.

Use the most suitable permitted execution mechanism for the specific review.
Read and follow [Review Execution](references/review-execution.md) when selecting
or delegating review contexts. Degrade gracefully when a mechanism is
unavailable while preserving the requested review depth as far as the runtime
reasonably permits. Record any limitation that materially reduces context
independence, coverage, or verification confidence. A separated pass in the
implementation context is never independent review.

## Required Engineering Doctrine

Before gathering review scope, load and follow `$engineering-for-certainty`.
Treat it as the governing engineering standard while honoring its priority for
explicit user requirements and established repository conventions.

Load each companion whenever the reviewed change directly modifies its area or
explicitly integrates with its APIs:

- Load `$engineering-observability` for logs, metrics, traces, telemetry, audit
  records, correlation context, redaction, alerting, or frontend log ingestion.
- Load `$engineering-resilience` for external calls, retries, timeouts, mutation
  safety, idempotency, concurrency, queues, cron tasks, webhooks, jobs, or other
  async processing.
- Load `$engineering-auth-security` for authentication, authorization, cookies,
  sessions, CSRF, tokens, actor context, permissions, protected routes, policy
  enforcement, or secrets.
- Load `$engineering-frontend` for frontend modules, routes or screens, API
  adapters, forms, accessibility, client state, or web and mobile testing.

If `$engineering-for-certainty` or a triggered companion is unavailable, stop
before reviewing. Name every missing skill and tell the user to install
`code-review` together with the core doctrine and required companions. Do not
reconstruct missing doctrine from memory.

Use doctrine observations as candidate issues, not automatic findings. Every
candidate must be applicable to the changed surface, identify a material
consequence, and survive the normal verification pipeline. Do not infer
process-only violations, such as failure to use TDD, from a diff that cannot
establish how the work was performed.

Use each activated companion as a finder and verification obligation, not only
as background reading. Record the core doctrine, activated companions, and the
checks they added in the final review scope.

## Pipeline

Run the review as six distinct phases:

1. Gather the target, intent, governing rules, and validation context.
2. Find candidate issues through independent review angles, including style and clarity.
3. Normalize and deduplicate candidates by root cause.
4. Verify every survivor and run targeted validation where useful.
5. Rank confirmed findings into one review queue and work through one finding at
   a time, or return the complete verdict queue to an authorized composing
   delivery workflow, then report residual risks and open questions.
6. After the user verifies the reported findings, analyze whether the governing issue or handoff could have prevented them and propose targeted `issue-review` skill improvements.

### Composing Delivery Mode

When `$code-review` is invoked as the read-only analysis engine inside
`$issue-delivery` or another explicitly authorized delivery workflow, read and
follow [Review Loop Contract](references/review-loop-contract.md) before Phase
0. Require a governing issue or approved task handoff with correction and
escalation authority. Return the complete evidence-based verdict queue to the
composing workflow; the Independent Reviewer stays read-only and does not select finding routes.

### Checkpoint Review Mode

When the composing delivery workflow identifies a review checkpoint, read and
follow [Checkpoint Review](references/checkpoint-review.md) before Phase 0.
It changes the declared range and return record, not the review standard or the
requirement for a final full integration review.

## Phase 0: Gather the Review Scope

### Resolve the target

Honor a user-supplied PR, branch, commit, range, file list, or explicit base before applying defaults.

For a merged PR, freeze the original PR diff using immutable base/head evidence
or a verified provider patch, and record the resulting merge/squash commit
separately. Do not substitute a moving default branch, an empty upstream diff,
or today's whole repository. A later-code audit is a distinct target. Agent
review never supplies the explicit human outcome required by post-merge review.

For an implicit local or branch review:

1. Inspect repository status before selecting the diff.
2. Prefer the merge-base range `@{upstream}...HEAD` when an upstream exists.
3. Otherwise use the repository's actual default branch when discoverable, then a local `main` or `master`, then `HEAD~1` as the final fallback.
4. Include staged and unstaged changes for an implicit local or branch review. For an explicit committed target, exclude them unless the user asks to include local work.
5. List relevant untracked files separately because `git diff HEAD` does not include them.
6. Detect renames, submodules, generated files, lockfiles, migrations, and binary changes instead of silently excluding them.
7. Record the selected base, head, working-tree inclusion, and reviewed file set. Explain an unexpectedly empty target.

Do not fetch, pull, switch branches, or mutate repository state from the
Independent Reviewer context. When the target must be prepared or refreshed,
return that requirement to the composing workflow or Delivery Operator and
review the frozen SHA it supplies.

### Recover intent and contracts

Read the smallest sufficient set of artifacts that define the intended behavior:

- PR or issue description and acceptance criteria.
- Feature files, plans, ADRs, and relevant documentation.
- Schemas, API contracts, migrations, configuration, and public types.
- Existing tests and nearby implementation examples.
- The issue's Runtime Acceptance Plan and current scenario evidence, including
  exact revision and environment, proxy blind spots, design comparison,
  invalidated re-runs, and downstream release gates when applicable.

For a checkpoint review, also read the checkpoint row, its owned acceptance and
traceability rows, its required appendices, the previous accepted checkpoint
head, and the frozen candidate head. Do not load unrelated issue history or raw
evidence unless a candidate requires it.

Compare the diff with the stated intent. Treat an incomplete or contradictory change as a candidate even when the edited code is internally consistent.

For every frontend change, inspect the issue, pull request, implementation
handoff, and repository design documentation for an authoritative design
artifact. When one applies to any changed surface, read
[`$engineering-frontend`'s Design Conformance And
Audit](../engineering-frontend/references/design-conformance.md), raise the
review to at least Medium effort for that surface, and resolve the approved
Design Reference Manifest, Evidence Bundle, Design Audit Matrix, and current
source access before planning finders. Do not require the author to have used a
special label when the authoritative design is otherwise discoverable. A casual
inspiration image does not activate the audit unless the issue adopts it as the
design authority.

### Read governing instructions

Read every applicable repository or ancestor agent-instruction file and relevant
architecture or contribution document. For convention findings, cite the exact
governing rule; do not report vibes-based style preferences.

### Choose review effort

Honor an explicit user effort level. Otherwise scale effort by change risk, not only diff size:

- **Low**: resolve scope, scan each hunk and enclosing function, audit removed behavior, and verify a small set of high-confidence candidates.
- **Medium**: run all relevant core finder angles, trace changed contracts, inspect tests, and verify every survivor.
- **High**: run independent focused passes, activate relevant specialist angles, and independently verify material candidates.
- **Max**: use the widest justified fan-out, trace broader contracts and architecture, and run the strongest safe targeted validation.

These are review-depth levels, not replacements for Quick, Standard, and
Critical implementation rigor. Choose the review depth that the actual risk
requires; Standard does not automatically mean Medium, nor Critical Max.

Increase effort for privilege boundaries, irreversible writes, migrations, public contracts, concurrency, financial or sensitive data, core workflows, and weak test coverage. State the chosen effort and reviewed scope in the final response.

At High or Max effort on a multi-file change, enumerate every changed file that is not generated, vendored, or a binary asset, and confirm each received at least an Angle A and Angle B pass before candidate-gathering is called complete. Do not let a large new module absorb review depth at the expense of smaller adjacent files in the same diff — a five-line configuration or deployment change fails exactly as often as a five-hundred-line new module, and gets less scrutiny by default.

### Plan review execution

Choose assurance depth before execution mechanism. For multiple finder or verifier contexts, High or Max effort, or delegation, use [Review Execution](references/review-execution.md) for packet design, isolation, and finder budget. Add contexts only when they justify handoff and integration cost; never weaken required independent assurance.

## Phase 1: Find Candidate Issues

Treat every observation as a candidate, not a finding. Run each relevant angle independently so one framing does not suppress another.

Read [Finder Angles](references/finder-angles.md) to plan coverage. Inspect every relevant hunk and removed behavior (A and B); trace changed contracts and apply only specialist angles triggered by the changed surface. A finder packet may cover several angles; do not create one agent per angle. For frontend work, read the applicable [Frontend Review Checks](../engineering-frontend/references/frontend-review-checks.md).

## Candidate Standard

Normalize each candidate before verification:

```text
file
line
category
provisional_severity
summary
failure_scenario
code_path
supporting_evidence
assumption
refuting_evidence_to_find
suggested_fix
```

Write `failure_scenario` as concrete input or state -> executed path -> wrong output, corruption, security exposure, outage, or material maintenance consequence. Drop candidates that have no nameable material failure or consequence.

Allow finders to return their strongest candidates first. Use a soft limit of six candidates per angle to control noise, but never suppress additional Critical or High candidates. Record when an angle was truncated.

## Phase 2: Normalize and Deduplicate

Collapse candidates that share one root cause. Prefer one finding that names all affected consumers over repeated symptoms. Keep candidates separate when they require different fixes or have independently actionable failure paths.

Re-evaluate provisional severity after deduplication; broad impact may raise severity, while duplicated symptoms must not inflate it.

Maintain one Independent Reviewer-owned candidate ledger. Normalize incoming candidates to the Candidate Standard and attach their finder packet and evidence source.

Finders may return their strongest candidates before completing their whole packet. At High or Max effort, verification may begin while other finders are still working only after the Independent Reviewer has normalized the candidate and established a sufficiently stable root-cause claim.

Do not begin early verification for a candidate likely to merge with findings from another active packet. If later evidence changes, broadens, or merges the root-cause claim, discard the stale verification result and verify the final normalized claim again.

The Independent Reviewer may reconsider managed orchestration once after
candidate normalization when the discovered verifier fan-out is materially
larger or more complex than the initial plan. Do not reconsider it after the
user has declined it during the same review.

Do not finalize the finding queue until every finder has completed, every changed surface has received its required coverage, and global deduplication is complete.

## Phase 3: Verify Every Survivor

For each candidate:

1. Restate the precise failure claim and required code path.
2. Identify the evidence that would disprove it.
3. Search relevant code, tests, contracts, schemas, documentation, configuration, migrations, call sites, and runtime constraints.
4. Check guards, types, feature flags, transaction boundaries, retries, tests, deployment assumptions, and usage constraints.
5. Reconstruct reachability from entrypoint to failure.
6. Assign exactly one verdict.

Use these verdicts:

- **CONFIRMED**: Repository or runtime evidence supports a reachable, material failure.
- **CONDITIONAL**: The failure is realistic but depends on a clearly stated environment, state, workload, or product assumption.
- **REFUTED**: Cited evidence proves the path is guarded, impossible, already handled in the change, or immaterial. Drop it.
- **NEEDS_CONTEXT**: Required product or operational context cannot be recovered. Ask or list it as an open question; do not present it as a finding.

Before finalizing a `CONDITIONAL` verdict, check for in-repo evidence that already resolves the stated assumption — integration or sandbox documentation, fixtures, existing tests, or configuration defaults describing the real current environment or upstream behavior. If that evidence shows the assumption already holds today, reclassify the candidate as `CONFIRMED` instead of leaving it `CONDITIONAL`.

Route uncertainty before assigning `NEEDS_CONTEXT`:

- **Discoverable fact**: inspect code, tests, contracts, documentation, configuration, history, or available tools.
- **User-owned decision**: ask a numbered question that states the decision, impact, options, and recommendation. Keep it out of findings until answered.
- **Empirical unknown**: run the narrowest safe targeted validation or investigation that can resolve it. Do not ask the user to guess runtime behavior.

Only `CONFIRMED` candidates become normal findings. Place `CONDITIONAL` candidates under residual risk with their assumptions. If further evidence establishes reachability, reclassify the candidate as `CONFIRMED` before reporting it as a finding. Never report a finding solely because a delegated review context proposed it.

Prefer two independent evidence points for Critical or High findings when practical. One direct point is enough for mechanically provable failures such as a type error, missing export, failing command, unreachable path, or reproduced defect.

### Independent skeptical verification

Give every normalized survivor at least one skeptical verification pass that is logically independent from its finder. Ask the verifier to seek both confirming and refuting evidence and to cite the exact guard, call path, contract, runtime constraint, test, or reproduction supporting its assessment.

For Critical or High candidates, or candidates with material uncertainty, use a second independent verifier when available. Give the verifiers different mandates where useful:

- one attempts to prove the claimed path unreachable, guarded, or immaterial;
- one reconstructs reachability and material impact from the entrypoint.

Use a third verifier only when the first two materially disagree, rely on incompatible assumptions, or leave an important evidentiary gap.

Do not determine verdicts by majority vote. Evidence outranks vote count. One direct reproduction may establish a defect; one cited guard may refute several unsupported confirmations. The Independent Reviewer must reconcile the evidence and assign the final verdict under the normal CONFIRMED, CONDITIONAL, REFUTED, and NEEDS_CONTEXT definitions.

When delegated review contexts are unavailable, perform the same verifier work
as deliberately separate skeptical passes. If the active context implemented
the candidate, record that the review is not independent and return the missing
independence as a blocking gate to the Delivery Operator.

### Targeted validation

Run the narrowest safe command that materially changes confidence: a focused test, type check, build, linter, static analysis, minimal reproduction, integration test, or schema or migration validation.

Distinguish validation executed from validation merely recommended. A passing test refutes a candidate only when the test exercises the disputed behavior and contains a meaningful assertion. Do not mutate production systems or external state during validation.

For Runtime Acceptance evidence, keep the reviewer read-only. Reconcile the
scenario ledger against the issue, diff, deployed build identity, screenshots or
sanitized responses, and current repository state. Use a safe read-only runtime
reproduction only when it can materially resolve a disputed claim without
creating accounts, changing shared data, deploying, or exercising destructive
behaviour.

Return missing, failed, stale, secret-bearing, or wrong-revision runtime evidence
as a verification gap to the composing delivery workflow. Create a normal code
finding only when that gap and the inspected implementation support a concrete,
reachable, material failure; do not turn process incompleteness alone into a
code defect.

## Severity and Ranking

Use severity consistently:

- **Critical**: Likely exploitable security issue, irreversible data loss or corruption, severe outage, or broken core workflow.
- **High**: Likely production bug, privilege-boundary failure, reliability regression, or missing protection for important data.
- **Medium**: Reachable edge-case bug, operational blind spot, material performance issue, maintainability risk likely to cause defects, or meaningful test gap tied to changed behavior.
- **Low**: Minor but concrete risk worth fixing that is unlikely to cause material harm soon.

Rank by severity and impact, then confidence, breadth, category, and urgency.
Give substantial weight to what users and stakeholders will see on primary UI
journeys, including material divergence from an approved design; functioning
code does not make a conspicuous visual or interaction failure Low severity.
Within comparable severity, rank a prominent user-visible failure ahead of
internal cleanup or convention risk. Keep security, data integrity, core
correctness, and reliability consequences in the impact assessment. Do not let
a trivial correctness issue outrank a materially higher-severity architectural
risk.

## Output

### Present the verified result

Read [Finding Presentation](references/finding-presentation.md) before reporting findings or a clean closeout. Present one verified finding at a time unless the user explicitly requests a batch or an authorized composing workflow receives the full queue. Keep stable IDs, material evidence, the selected scope and effort, actual execution mechanism, validation, and coverage limits. An empty verified queue is reported as `No material issues identified.`

## Phase 4: Retrospective When Requested

After the user confirms findings or asks for a retrospective, read [Issue-Review Retrospective](references/retrospective.md). Do not run it during composing delivery or edit issue-review doctrine from the Independent Reviewer context.

## Comment and Fix Modes

Keep a normal review read-only.

- For GitHub inline publication, use `$pull-request-review` as the composing workflow. This skill supplies verified findings and evidence; the composing workflow owns existing-thread reconciliation, user adjudication, responsible-engineer tagging, fix snippets, GitHub writes, and post-write verification.
- When this skill is already running inside `$pull-request-review`, return stable finding IDs and complete candidate records to that workflow after verification. Do not load the composing skill from inside this skill or post comments directly.
- When this skill is running inside `$issue-delivery`, follow the Review Loop Contract, return verdicts and route-relevant facts, and leave routing, correction, commit, push, PR, and CI action to that composing workflow.
- A direct request to review a GitHub PR and publish comments should select `$pull-request-review` before analysis. If it is unavailable, keep the review read-only and name the missing workflow instead of reconstructing GitHub mutation behavior here.
- With an explicit fix request or `--fix`, present the review first, then hand
  the accepted correction to a Delivery Operator or bounded Implementation
  Worker context. The Independent Reviewer does not perform the fix. Do not
  auto-fix `CONDITIONAL` or `NEEDS_CONTEXT` candidates.

Do not post externally or modify the working tree from the Independent Reviewer
context. Return authorized actions and evidence to the composing workflow,
Delivery Operator, or bounded Implementation Worker.
