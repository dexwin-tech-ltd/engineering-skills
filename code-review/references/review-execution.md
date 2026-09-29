# Review Execution

Use this reference to realize the Independent Reviewer contract on any capable
agent harness. The shared skill defines responsibilities and evidence; the
active harness chooses concrete agents, models, providers, reasoning settings,
permissions, tools, runtimes, and isolation.

## Ownership

The active Independent Reviewer owns the review plan, candidate ledger,
normalization, deduplication, final verdicts, and complete return record.
Delegated finder and verifier contexts return evidence to that owner and never
gain workflow mutation authority.

## Execution Paths

Choose a path that preserves the required independence and coverage:

1. managed orchestration for several bounded packets;
2. parallel isolated review contexts for independent packets;
3. sequential clean review contexts when parallelism is unavailable; or
4. explicitly separated discovery and verification passes in the active
   Independent Reviewer context when that context did not implement the work.

Record the requested assurance and the mechanism actually used. If the active
context implemented the candidate, a same-context pass is supplemental
self-review, not Independent Review; return that limitation as a blocking gate.

## Read-Only Safety

Enforce read-only review through harness permissions or an equivalently
constrained mechanism. A prompt alone is not a permission boundary. Review
contexts may run safe, non-mutating validation but must not edit, commit, push,
publish, resolve threads, or alter external state.

## Finder And Verifier Packets

Give each finder raw governing requirements, exact diff boundaries, relevant
repository contracts, validation evidence, and a distinct review angle. Return
raw candidates with reachability, impact, and refuting evidence.

After normalization, give each verifier the candidate claim, raw evidence, and
relevant code—not another reviewer's conclusion as authority. Verification is
not voting. The Independent Reviewer assigns the final verdict from the
evidence and records disagreement.

When evidence depends on user-owned context, return a structured
`NEEDS_CONTEXT` candidate. The user-facing workflow asks the question; review
helpers do not contact the user independently.

## Completion Barrier

Do not close until every planned packet completed or has a recorded fallback,
every candidate has one final verdict, the current frozen head was reviewed,
and all coverage or independence limits are explicit.

## Planning Depth and Execution

### Plan review depth and execution separately

Make review-planning decisions first:

- What review depth does the requested effort and change risk require?
- Which changed surfaces require coverage?
- How many genuinely independent finder work packets exist?
- Which candidates would require additional independent verification?
- Which targeted validation could materially change confidence?

Then make execution-planning decisions:

- Can parallel isolated review contexts execute the work packets effectively?
- Would managed orchestration materially improve coordination, isolation,
  latency, or context containment?
- Can the active harness enforce read-only permissions and the required context
  isolation?
- If permission or enablement is required, is the expected benefit large enough
  to justify involving the user?
- Which fallback preserves the review obligations when the preferred mechanism
  is unavailable?

### Choose the execution mechanism

Use the execution paths above. Add optional contexts only when a distinct work
packet can be checked independently and the expected coverage, latency, or
model-cost benefit exceeds handoff and reconciliation effort. The governing
rigor or issue may require independent passes regardless of that optional
break-even decision.

Do not assume managed orchestration is better merely because it is available.
Prefer direct delegation when the review requires only a small number of
independent work packets and the Independent Reviewer can manage their results
without material context pressure or latency.

Managed orchestration is materially useful when one or more of these apply:

- the plan requires more independent review contexts than the Independent Reviewer can comfortably manage turn by turn;
- the review has several coherent surfaces plus a second-stage verifier fan-out;
- finder and verifier stages can be productively pipelined;
- intermediate candidate and verification records would materially crowd the Independent Reviewer context;
- the same branching, deduplication, or retry pattern must run repeatedly;
- the expected latency reduction or context isolation is substantial.

A high-risk change alone does not require managed orchestration. A small
high-risk change may be better served by two carefully scoped isolated review
contexts.

### Preserve requested assurance

A fallback execution mechanism may increase latency or reduce context independence without changing the requested review obligations. Preserve the requested effort as far as reasonably possible.

Do not claim that the requested assurance was fully achieved when runtime limitations prevented required coverage or independence. Complete the supportable review and record:

- the requested review effort;
- the actual execution mechanism;
- the coverage and verification completed;
- the specific assurance property that could not be achieved;
- the strongest available alternative used.

### Size the finder budget

Estimate effective diff size from additions plus deletions in reviewed text files. Exclude generated, vendored, lockfile, and binary changes from the line calculation, but still inspect them when they affect contracts, dependencies, deployment, or runtime behavior.

Start with one complete finder pass. Add a separate finder when a distinct
surface or invariant merits independent scrutiny and its expected benefit
outweighs prompt, context-transfer, reconciliation, and integration effort.
High and Max effort still require their stated coverage and independence; a
governing issue may require multiple fresh contexts. Increase scrutiny for:

- authentication, authorization, tenant isolation, money, sensitive data, migrations, destructive persistence, concurrency, or irreversible writes;
- changed public contracts, environment schemas, deployment configuration, or cross-layer behavior;
- weak integration coverage or unusually broad architectural reach.

Use fewer contexts when the diff is mechanically repetitive or cannot be divided without separating code from the contracts needed to understand it. Never spawn empty or substantially duplicate finders to satisfy a numeric quota.

Diff size controls review capacity, not risk. A small high-risk change may require more independent review than a large routine change.
