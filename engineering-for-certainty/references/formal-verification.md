# Formal Verification

Consider formal verification when a named consequential invariant has a
credible gap in ordinary or property-based tests. State the invariant, the
harm if it fails, and the gap the method must address. Choose the lightest
method that can close it, such as bounded state exploration for interleavings
or a code-level verifier for an implementation property.

## Establish Correspondence

- Identify the exact implementation revision and observable contract. Map
  model states, transitions, failure and retry behavior, and atomic steps to
  the code, storage guarantees, or contract they represent. Check assumptions
  such as which operations can interleave; an atomic model step must have a
  justified atomic counterpart.
- Specify bounds, assumptions and exclusions. Pick an exploration bound that
  can reach the failure of interest. Check sensitivity with a known bad
  variant when feasible and retain its shortest useful counterexample trace.
- Record the tool and command, revision, bounds, outcome, and any unresolved
  correspondence. A clean bounded run supports only the stated invariant
  within the explored model and its justified correspondence. Report a
  missing mapping as unsupported implementation evidence.

For example, model two retrying workers and one outbox item. An atomic claim
and mark can allow only one worker to send in a model with no worker or
delivery crashes. Split the eligibility read and mark to exercise the case
where both workers observe the item as unclaimed and then both send. Report
the explored action bound and the schedule that exposes the duplication.
This model leaves storage semantics, crashes, and delivery guarantees to
separate proof.

## Keep the Evidence Current

Treat changes to a mapped transition, concurrency or atomicity guarantee,
dependency, or bound as evidence invalidators. Recheck the mapping, update
the model and rerun the affected proof. If implementation and model disagree,
record whether the mapping drifted or a product defect was found. Keep
affected evidence unverified until the discrepancy is resolved.

Keep applicable [real-boundary proof](runtime-acceptance.md), integration
tests, review, and the existing
[mutation policy and repository pauses](verification-details.md#mutation-analysis)
in force. Ordinary work with adequate existing proof needs no formal model.
