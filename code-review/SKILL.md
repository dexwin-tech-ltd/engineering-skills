---
name: code-review
description: Perform staff-level code reviews for diffs, pull requests, commits, branches, or local changes. Use when the user asks for a code review, PR review, change review, risk review, or wants issues found across correctness, security, reliability, data integrity, performance, observability, maintainability, and testing quality.
---

# Code Review

## Overview

Perform a staff-level code review that prioritizes material engineering risk over style. Review as a senior engineer, SRE, security engineer, QA engineer, and future maintainer.

Use subagents for focused passes when that is more efficient and available, especially for broad changes, security-sensitive code, concurrency/data-integrity work, or large test suites. Keep subagent prompts scoped to raw artifacts or clear review areas; do not pass conclusions you want them to confirm.

## Review Workflow

1. Identify the review target: local diff, branch diff, commit range, PR, or specific files.
2. Read the relevant code, tests, schemas/contracts, migrations, configuration, and documentation needed to understand the change.
3. Explain your understanding of the change before listing issues.
4. Review the change for:
   - Correctness
   - Security
   - Reliability
   - Data integrity
   - Performance
   - Observability
   - Maintainability
   - Testing quality
5. Identify hidden assumptions, edge cases, failure modes, and future maintenance risks.
6. Refute candidate findings before reporting them.
7. Run or recommend targeted validation when it materially improves confidence.
8. Report only material issues. Ignore style issues unless they materially affect readability, correctness, maintainability, or operational safety.

## False-Positive Control

Treat initial review observations as candidate findings, not findings. Before reporting one:

1. State the precise failure claim and the code path or behavior it depends on.
2. Identify what evidence would disprove it.
3. Search the relevant code, tests, schemas/contracts, docs, config, migrations, and call sites for that evidence.
4. Check whether existing invariants, guards, types, feature flags, transaction boundaries, retries, tests, or usage constraints already make the candidate harmless.
5. Classify the candidate:
   - **Confirmed**: evidence supports a real, material risk.
   - **Refuted**: repo evidence shows the risk is already handled or cannot occur. Drop it.
   - **Uncertain**: evidence is mixed or depends on an unstated assumption. Either gather more evidence or present it as an open question/residual risk, not as a finding.

Prefer at least two independent evidence points for High or Critical findings when practical, such as the changed code plus a missing/contradictory test, or a call site plus a schema/contract mismatch. One direct evidence point is enough for mechanically provable issues, such as a missing export, failing command, type error, or unreachable file path.

Use subagents as an independent validation surface when all of these are true:

- Subagent or multi-agent tools are available.
- The candidate is material enough that a false positive would waste significant user time or steer the fix incorrectly.
- The validation can be scoped without telling the subagent the expected conclusion.

Ask subagents to inspect raw artifacts or a bounded review area and return evidence-backed findings. Compare their result with your own refutation pass; do not report a finding solely because a subagent said so.

## Finding Standard

For every issue, include:

- Severity
- Impact
- Reasoning
- Suggested fix

Ground findings in concrete files, lines, code paths, behaviors, or missing tests. Do not pad the review with speculative concerns; if a risk depends on an assumption, state the assumption explicitly.

Use severity consistently:

- Critical: likely exploitable security issue, data loss/corruption, severe outage, or broken core workflow.
- High: likely production bug, privilege boundary issue, reliability regression, or missing protection for important data.
- Medium: plausible edge-case bug, operational blind spot, maintainability risk that will likely cause defects, or meaningful test gap.
- Low: minor risk worth fixing but unlikely to cause material harm soon.

## Output Format

Start with the understanding of the change, then list findings ordered by severity. Include file and line references whenever possible.

Use this structure:

1. Understanding of the change
2. Findings
3. Hidden assumptions, edge cases, failure modes, and future maintenance risks
4. Tests or validation performed
5. Residual risk or open questions

If no material issues exist, explicitly state:

`No material issues identified.`

Do not bury material findings below a summary. Keep summaries brief and secondary to actionable review results.
