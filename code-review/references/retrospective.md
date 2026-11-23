# Issue-Review Retrospective

Read only after the user verifies findings or explicitly requests a retrospective.

## Phase 4: Feed Verified Findings Back Into Issue Review

Run this phase only after the user has reviewed the reported issues and explicitly confirmed which findings are valid, or when the user explicitly requests the retrospective. Do not delay the initial code-review report while waiting for this feedback.

Do not run this retrospective automatically during Composing Delivery Mode.
Finishing delivery does not authorize editing review doctrine; wait for the
user's separate confirmation or request.

For each user-confirmed finding:

1. Recover the governing issue, ticket, feature file, plan, or implementation handoff used for the change. If none exists, state that the finding cannot support an `issue-review` improvement.
2. Trace the finding to the planning artifact: identify the missing, ambiguous, contradictory, weakly testable, or unverifiable instruction that allowed the defect. Distinguish a planning failure from an implementation mistake made despite clear guidance.
3. Inspect the current `issue-review` skill before proposing changes. Check whether an existing gate already covers the failure and was merely not followed; do not propose duplicate doctrine. If `issue-review` is unavailable, skip this retrospective and state that dependency clearly.
4. Test generality. Propose a skill change only when it would prevent a recurring class of issue-writing or issue-review failures across projects, not when it encodes project-specific facts or the details of one bug.
5. Prefer the smallest change that adds a check, sharpens an existing gate, or requires stronger evidence. Preserve the distinct roles: `issue-review` improves implementation instructions; `code-review` diagnoses the resulting code.

Do not recommend an `issue-review` change for:

- Findings the user rejects or has not verified.
- Pure implementation errors that contradicted clear, sufficient issue guidance.
- Gaps already covered explicitly by the current `issue-review` skill unless the evidence shows that wording is too weak or easy to misapply.
- Defects that could not reasonably have been anticipated during issue preparation.

Present the retrospective separately from the code findings. For each proposed skill update include:

- The verified finding or recurring failure class it addresses.
- The causal gap in the governing issue or handoff.
- The current `issue-review` section affected.
- The exact proposed wording or a compact patch.
- Why the change is generalizable and how it would have made the defect less likely.
- Any cost, false-positive risk, or added review burden.

Close the issue-preparation portion with one of these explicit outcomes:

- `Proposed issue-review improvements:` followed by the proposals, and ask the user whether to apply them.
- `No issue-review update warranted.` followed by the evidence-bound reason.

Never edit the `issue-review` skill from the Independent Reviewer context.
Return any authorized update to a Planning Agent or Delivery Operator for
mutation and validation.

## Recurring Agent Mistakes

Use the same retrospective trigger. Inspect only the repositories and evidence
the user authorized. Look for at least two distinct, verified occurrences of
the same failure mechanism. Link the evidence for each occurrence and group
multiple reports of one execution together. Handle a single occurrence through
the ordinary finding route.

Use the evidence to find the prevention point:

1. Check existing safeguards first. If a valid safeguard was skipped or
   bypassed, verify the path it covers and where the actual path diverged.
   Recommend making the existing safeguard effective on that path.
2. Otherwise, prefer the smallest practical change at the earliest effective
   point. Consider one safe path in the architecture, then a type or schema
   constraint, then a targeted lint or CI check with a useful failure message,
   then a behavioral test. Use concise guidance when the choice needs judgment.
3. For an executable safeguard, name a prior wrong input or sequence that
   should fail for the expected reason and a valid case that should pass
   through the same boundary. For an architectural change, specify how to show
   that the unsafe path is unavailable or rejected.

Return the verified occurrences, shared mechanism, proposed owner, smallest
change, trade-off, and exact proof plan in the retrospective. Keep genuine
issue-preparation gaps on the existing route above. Deduplicate proposals that
address the same prevention point.

The Independent Reviewer provides a proposal. A separately authorized delivery
context applies the change and verifies the proof. Use the project's issue or
backlog conventions when the user authorizes a follow-up.
