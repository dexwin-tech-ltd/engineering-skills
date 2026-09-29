# Issue Rewrite

## Rewrite

Preserve the repo's established issue format when one exists. If no format exists, use this fallback:

```md
# <title>

Status: open
Type: Bug | Feature | Chore | Exploration
Severity: High | Medium | Low | Very Low
Branch: <exact conventional branch>
Base: <exact canonical branch or preceding stacked-PR branch>
Worktree: dedicated | shared checkout - <repository convention or explicit user direction>
Parent: <parent issue, when this is a child slice>

## Agent Start Here

## Problem / Motivation

## Root Cause / Background

## Affected Surface

## Acceptance Criteria

## Out of Scope

## Implementation Guardrails

## Dependencies

## Execution Plan

### Review Checkpoints

## Test Approach

### Traceability Ledger

## Runtime Acceptance Plan

## Design Reference Baseline

## Review Loop Contract

## Completion Requirements

## Notes
```

Omit sections that genuinely do not apply, including `Design Reference
Baseline` when no authoritative design applies, except keep `Runtime Acceptance
Plan` with a justified `Not applicable` entry for non-runtime work. Do not add
empty sections. Populate
`Completion Requirements` with the issue-specific write-back, evidence, status,
and propagation rules during readiness review. Add the actual `Issue Completion
Record` only after implementation evidence exists; never prefill it with
placeholders or predicted results.

For immediate delivery, return the declared branch, base ref, resolved base SHA,
runtime worktree path, and Approved Issue Commit SHA to `$issue-delivery`. For
backlog-only work, state that no implementation worktree was created. Never
report an issue as ready for immediate implementation from a different branch or
worktree than the one that contains its Approved Issue Commit.
