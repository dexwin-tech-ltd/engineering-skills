# Finding Presentation

Read when verified findings must be reported, or when composing a complete clean-review closeout.

### Explain findings for the reader

Apply this standard to every finding presented to a person: review discussions,
complete reports, proposed and published GitHub comments, and findings escalated
by composing delivery workflows. Internal agent return records may retain their
structured technical format; a workflow presenting them to a person must apply
this standard.

Assume the reader understands software development but has not traced this code
path. Make each finding understandable on its own, on the first read:

- Explain what the affected code is supposed to do.
- Describe the concrete input, state, or event that triggers the problem.
- Explain what the code does instead and why it takes that path.
- Connect that behaviour to the material consequence without skipping causal steps.
- Explain what should change and how that correction addresses the problem.

Use familiar words, concrete actions, and short sentences. Explain an unavoidable
technical term immediately. Keep exact identifiers and file references as
supporting evidence, rather than expecting them to explain the behaviour. The
one-sentence defect statement is a summary, not a substitute for the explanation.
Simple does not mean terse: preserve every step needed to evaluate the claim.
Do not invent a causal link to complete the story; investigate missing evidence
or narrow the claim and preserve its uncertainty before presenting it.

Before presenting a finding, check that a reader can explain what should happen,
what goes wrong and when, why the consequence follows, and how the proposed
correction helps, without opening the referenced files or asking for a simpler
explanation. Rewrite any finding that fails this check. Scale the explanation to
the defect; do not repeat the same facts merely to fill separate headings.

### Finding record and queue

Put actionable findings first. For each finding include:

- Severity and verifier verdict.
- Clickable file and precise line.
- One-sentence defect statement.
- Concrete failure scenario and affected code path.
- Evidence and impact.
- Suggested correction.
- Explicit assumption when applicable.
- Missing regression test when relevant.

Investigate, verify, deduplicate, and rank the complete finding set before presenting the first result. This broad analysis prevents the current finding from being framed without a later-known dependency or shared root cause.

Present one confirmed finding at a time in severity and impact order. Give it a stable ID and show `**Progress: Finding <position> of <total> - <remaining> remain after this**` on every response that presents or continues it. Calculate position as prior dispositions plus the current finding; calculate total as prior dispositions plus the current and queued findings. Do not show a remaining count alone; a percentage may appear only as secondary information. Keep discussing the current finding until the user accepts it as valid, rejects it, requests a revision, defers it, or asks for named evidence. Only then present the next finding, after recomputing the queue for dependencies, duplicates, or invalidated claims. If recomputation changes the total, state `**Queue revised: <old> -> <new>.** <reason>` before the next finding; never silently change the denominator or stable finding IDs. A generic request to review code is not a request for batching.

In Composing Delivery Mode, use the reference contract's delivery return record
instead. Complete the full queue before returning it. Return verdicts,
evidence, correction guidance, and route-relevant facts without selecting
workflow routes or applying corrections. The Delivery Operator owns routing,
correction, durable follow-up capture, and user escalation.

In Checkpoint Review Mode, return the checkpoint review outcome using the
checkpoint reference contract. The Delivery Operator derives the checkpoint
result after routing the returned findings. A clean review outcome covers only
the declared checkpoint range and integration seams; it is not a clean final
review of the complete issue.

Batch at most ten findings only when the user explicitly requests a batch or complete report. Preserve the same evidence for every item, allow adjudication by stable ID, and never treat a batch boundary as permission to omit Critical or High findings. There is no total finding cap.

When the review queue is empty, provide this closeout:

1. Brief understanding of the change.
2. Scope, requested effort, actual execution mechanism, effective diff size, planned and actual finder count, finder work packets, verifier count, doctrine skills applied, and the checks each activated companion added.
3. Validation performed and results.
4. Conditional residual risks, `NEEDS_CONTEXT` questions, and remaining coverage gaps.

For a triggered Design Audit, also record the authoritative source and approved
baseline, source-drift result, Evidence Bundle path, matrix coverage, comparison
methods, independent verifier coverage, typed outcomes, approved deviations,
and anything still unverified.

Before presenting the first finding, confirm that:

- every planned finder packet completed or has a recorded fallback;
- every triggered Design Audit completed and classified its required matrix
  scope, or the missing proof is recorded as an explicit verification gap;
- every candidate has exactly one final verdict;
- no verifier result refers to a superseded pre-deduplication claim;
- any managed-orchestration decline or execution limitation is recorded once without weakening the evidence standard;
- the reported review effort distinguishes requested assurance from assurance actually achieved.

Before delivering each finding or an explicitly requested batch, verify that the selected scope and effort are recorded, every candidate has exactly one verdict, every changed surface in scope was inspected, and all limitations and residual risks are classified. Keep the Independent Reviewer read-only. A composing workflow performs any explicitly authorized external writes in its own context.

Also verify that every directly affected companion area was identified and its
skill loaded, and that doctrine observations passed the same materiality,
reachability, refutation, and verdict requirements as every other candidate.

If no material issues survive verification, state `No material issues identified.` Do not manufacture findings to fill the format.

Use human-readable output by default. Provide structured JSON when the user requests machine-readable output, preserving severity, verdict, file, line, summary, failure scenario, evidence, and suggested fix.
