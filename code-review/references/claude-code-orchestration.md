# Claude Code Review Orchestration

Read this reference only when running in Claude Code.

## Contents

- Execution paths
- Read-only safety
- Permission and enablement
- When Workflow is worthwhile
- Coordinator
- Finder stage
- Verification stage
- User-owned context
- Completion barrier

## Execution paths

Use the same finder, candidate, verifier, and verdict contracts across every
execution path:

```text
Review coordinator
       |
       +-- Workflow orchestration
       |      optional scripted path
       |      use when materially beneficial and permitted
       |
       +-- parallel Agent subagents
       |      first-class concurrent path
       |
       +-- sequential clean contexts
       |
       +-- separated coordinator passes
```

Do not make Workflow the default merely because the tool exists.

## Read-only safety

Keep ordinary code review read-only under every execution path.

Claude Workflow agents run under the host's workflow permission behavior and
may receive edit-capable permissions. For every finder and verifier worker,
remove or deny mutation tools, including Edit, Write, NotebookEdit, and
worktree-changing operations.

Prefer read-only tools such as Read, Grep, Glob, and LSP. Do not give finder or
verifier workers unrestricted Bash access. Run narrowly selected validation
through the coordinator or a separately constrained validation worker.

Before adopting the Workflow path, verify that the current Workflow runtime can
enforce the required worker tool restrictions. If it cannot, do not use
Workflow for an ordinary read-only code review; use ordinary constrained Agent
subagents instead.

Prompt instructions such as “do not edit” do not replace an enforceable tool
boundary.

## Permission and enablement

When Workflow is available but requires approval:

1. State the concrete benefit in one sentence.
2. Invoke Workflow.
3. Let Claude Code's native approval prompt obtain permission.
4. If the user denies it, continue with parallel Agent subagents when available.
5. Do not ask again during the same review.

Do not add a separate AskUserQuestion step before the native Workflow approval
prompt.

When Workflow is absent because dynamic workflows are disabled or unavailable,
ask whether to enable them only when the planned review materially benefits from
scripted orchestration. Otherwise proceed without mentioning Workflow.

If the user explicitly requested Workflow, do not substitute another path
without telling them and obtaining direction.

## When Workflow is worthwhile

Prefer Workflow when:

- the plan needs a larger worker fleet than ordinary turn-by-turn delegation can
  manage cleanly;
- finder output feeds a substantial second-stage verifier fan-out;
- the pipeline needs deterministic branching, retry, or re-verification logic;
- intermediate results would materially crowd the coordinator context;
- the orchestration is valuable as a readable or reusable script.

Prefer ordinary Agent subagents for a small number of finders or verifiers.

## Coordinator

Keep the coordinator in the invoking context. The coordinator owns:

- review planning and execution-mode selection;
- finder work-packet construction;
- the canonical candidate ledger;
- normalization and root-cause deduplication;
- decisions about early verification;
- reconciliation of verifier evidence;
- final verdicts and ranking;
- the one-finding-at-a-time text Review Queue.

Do not require the entire skill to run with `context: fork`. Workflow
availability differs inside subagent contexts.

## Finder stage

Create the planned finder work packets concurrently up to the resolved finder
budget. Give every finder:

- the complete diff or the portion required to understand its bounded surface;
- relevant old and new files;
- governing issue, contracts, repository instructions, and applicable doctrine;
- its work-packet ownership;
- the Candidate Standard output shape.

Do not give a finder another finder's candidates or expected conclusions.

Allow finders to return strong candidates early while continuing their assigned
coverage. Send every result to the coordinator-owned candidate ledger.

## Verification stage

After coordinator normalization, create skeptical verifier work for eligible
survivors. Verification of stable early candidates may overlap with unfinished
finder packets.

Run one verifier for each normal survivor. Add a second verifier for Critical,
High, or materially uncertain candidates. Add a third only to resolve
substantive disagreement.

Do not ask for a vote. Ask for cited confirming and refuting evidence. Return
the verifier assessment to the coordinator, which owns the final verdict.

## User-owned context

Workflow runs cannot gather ordinary mid-run user input. When a worker
identifies a user-owned decision or missing operational context, return it as a
structured NEEDS_CONTEXT candidate. Let the coordinator ask the user after the
workflow stage completes.

Do not let a workflow worker guess user-owned product or operational decisions.

## Completion barrier

Before producing the Review Queue, wait for:

- every finder packet to finish;
- required Angle A and Angle B file coverage;
- final root-cause normalization and deduplication;
- re-verification of any merged or materially changed claim;
- final verdict assignment for every survivor.

Return control to the normal text Output protocol.

Do not require ReportFindings, persisted effort state, or automatic preflight
chaining.
