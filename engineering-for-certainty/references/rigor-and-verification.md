# Risk, Rigor, and Verification

Use this contract for implementation work. Choose the cheapest proof that gives
sufficient confidence in the actual change. A repository rule or an approved
issue may require stronger proof; a lower rigor label never overrides it.

## Select Rigor

Assess plausible failure impact, likelihood, reversibility, coupling, and the
strength of available checks. Classify the behavior that may change, not the
directory name, line count, or apparent simplicity of the edit. Record a short
reason for the selected level. **Standard is the default.**

| Level | When it fits | Completion path |
| --- | --- | --- |
| **Quick** | A clearly local, reversible change with no credible effect on a sensitive invariant, public contract, or cross-system behavior. Examples include copy, styling, a small local refactor, or an obvious pure-function fix. | Inspect the nearest relevant code and instructions; implement; run the smallest direct check; inspect the final diff; finish when the requested outcome is proved. No routine independent model review or canonical issue is required. |
| **Standard** | Normal feature or bug work with bounded integration risk. | Read relevant context; make a concise plan; implement with focused checks; verify the assembled behavior where integration matters; obtain one meaningful independent review; batch in-scope corrections; rerun invalidated proof and obtain a fresh independent review of the full resulting change after each correction batch. |
| **Critical** | A change that could affect authorization, authentication, payments or financial calculations, persisted data, migrations, destructive operations, concurrency, security, production infrastructure, or another high-impact invariant. | State the invariants and failure modes; use the relevant specialist doctrine and explicit proof, including adverse cases and recovery where applicable; use independent adversarial review and broader verification justified by the risk. Revalidate invalidated proof and independently review the full resulting change after each correction batch. |

A sensitive file alone does not force Critical. A copy edit on an authentication
screen may be Quick if the changed behavior is demonstrably isolated. A change
to a permission guard is Critical even if it is one line. If isolation cannot
be established cheaply, use Standard or Critical until evidence resolves it.

An explicit `$issue-review` or `$issue-delivery` request follows that formal
issue workflow, including its approved evidence and independent-review gates.
Quick is a task-level path for work that does not require that workflow. Do not
silently reclassify an approved issue or bypass an existing repository gate.

## Spend Verification Where It Changes Confidence

Start with the smallest relevant module, nearby patterns, affected callers,
and existing checks. Expand only when a changed contract, failing check, or
other evidence reveals wider coupling. Reuse current plans and verified facts;
do not rerun broad scans, full suites, or the same checks without a reason they
could reveal a newly invalidated assumption.

- **Quick:** Run an existing focused automated check when it directly covers the
  outcome. Otherwise use a focused manual or visual check and state what was
  observed. Do not add a test that merely restates a trivial edit.
- **Standard:** Test changed behavior and failure paths that matter. Use the
  nearest useful compiler, lint, unit, integration, or runtime check. Exercise
  real wiring when a local test could hide an integration failure. Let the
  governing issue or repository define any mandatory broader gate.
- **Critical:** Prove the affected invariant and its important failure modes.
  Use deterministic constraints, tests, static analysis, migration proof,
  runtime acceptance, and recovery checks where applicable. Do not replace
  missing high-impact evidence with model confidence.

When changed behavior has an important invariant across a broad input space or
sequence of actions, assess the property-testing trigger in
[Verification Details](verification-details.md#property-based-testing).
The trigger follows behavior and available proof, not the rigor label alone.

Automated checks prove only the behavior they exercise. Model review should
focus on missing requirements, semantic correctness, architecture, security,
edge cases, and maintainability. A minor naming or style preference without a
concrete consequence does not block completion.

## Escalate and Stop

Escalate when checks expose unexpected behavior; requirements or scope become
materially ambiguous; a sensitive invariant, hidden coupling, migration,
data-loss, or concurrency risk appears; the architecture conflicts with the
plan; repeated fixes fail; or the available proof cannot support the claimed
outcome. Reclassify the work, update the plan or handoff, and run the additional
proof the new risk requires. Never keep Quick merely because it started Quick.

Stop when the requested outcome and applicable acceptance criteria are met,
direct proof is current for the final change, the final diff is understood,
there is no unresolved blocking finding or high-risk uncertainty, required
repository gates pass, and eligible discovered follow-ups have the closeout
disposition required by [Follow-Up Inbox](follow-up-inbox.md). Standard and
Critical also require the independent review their level or governing workflow
calls for. Do not continue searching for hypothetical improvements after these
conditions hold.

## Handoffs, Parallel Work, and Measurement

Keep a short plan in the active task for simple work. Save a compact handoff
only when work crosses an agent, session, or publication boundary. Reuse an
approved issue and its completion record when present. A handoff names the
outcome, owned files or interfaces, relevant decisions and invariants, checks
and results, deviations, open risks, and escalation triggers. It links to raw
logs instead of copying them. Each receiving stage verifies currency and
examines the actual diff; it does not rediscover settled context from scratch.

Delegate bounded execution by required capability, not a provider or model
name. State exact behavior, ownership, permitted local decisions, validation,
return evidence, and stop conditions. The active harness chooses the least
costly model that can satisfy the contract. Reserve ambiguous architecture,
sensitive invariants, difficult debugging, and material escalation for stronger
judgment. Parallel writers need disjoint ownership, isolated worktrees, and a
named integration path; otherwise work sequentially. Delegation never lowers
the selected verification or review bar.

Before adding an optional Implementation Worker, investigator, finder, or
verifier, check the break-even case: is there a bounded output that can be
verified independently, and is the likely elapsed-time or model-cost saving
greater than the prompt, context transfer, waiting, reconciliation, and
integration work? Delegate when that case is credible; handle small, tightly
coupled work directly. Do not invent a numeric threshold or ask the model to
estimate its own cost. Keep any independent review required by the selected
rigor level or approved issue even when optional delegation is not worthwhile.

Measure elapsed time, model tokens or actual cost where exposed, check runs,
review passes, correction cycles, escalations, and later rework from runner,
tool, and issue or PR records. Do not ask a model to estimate its own time or
cost. If exact cost is unavailable, report token use as a proxy and label cost
unavailable. Compare similar tasks by rigor and change type; treat escaped
defect trends as longer-term evidence. Do not create per-task measurement files
when the runtime already records the events.
