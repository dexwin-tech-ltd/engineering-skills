# Review Loop Template

### 13. Review Loop Contract

Every rewritten issue must state how implementation hands off to review and how
verified findings are routed. Do not rely on a long chat prompt, an implied
agent workflow, or the existence of a durable goal. If automatic delivery is
not intended, state that the workflow is human-gated and disable automatic
correction explicitly.

For an issue intended for `$issue-delivery`, include:

```md
## Review Loop Contract

- Assurance mode: `<Fast by default for bounded behavior; Extensive when requested
  or consequential risk/uncertainty requires it>`; record the reason and any
  explicit stronger proof or review gates. Formal delivery alone adds no
  independent-review requirement.
- Delivery mode: `$issue-delivery` through verified merge, followed by the
  established authorized release process; record any explicit narrower boundary.
- Human review: selective after merge under Delivery and Human Review; record
  the provisional selection and reason, then reassess the final PR. Selected
  work needs `human-review:pending`, a root `PRS_PENDING_HUMAN_REVIEW.md` entry,
  and a minimal PR description before merge.
- Review checkpoints: `<none; treat the issue as one delivery unit>` or
  `<ordered checkpoint IDs and outcomes>`.
- Automatic transitions: implementation -> checkpoint validation ->
  self-review or required independent review -> authorized corrections -> affected
  revalidation and updated review -> next checkpoint -> combined validation and
  self-review or required full independent integration review
  -> pull request -> CI follow-through -> merge-gate verification -> verified
  merge -> authorized release follow-through and tracking reconciliation.
- Checkpoint advance rule: advance only from a clean accepted checkpoint head.
  `AUTO_CORRECT` returns to correction and re-review of the same checkpoint.
  `USER_DECISION` and `BLOCKED` pause delivery. An unresolved confirmed finding
  never advances.
- Follow-up inbox rule: `DEFER_FOLLOW_UP` is a finding route only for confirmed,
  independently releasable discoveries outside this issue under
  `$engineering-for-certainty`'s Follow-Up Inbox contract. It may coexist with
  a `CLEAN` checkpoint result. Retain evidence during delivery and file a
  deduplicated repository backlog entry before issue completion.
- Auto-correction authority: `AUTO_CORRECT` only for confirmed, deterministic,
  in-scope corrections that preserve approved intent, architecture, contracts,
  security posture, dependencies, and test strategy.
- User decision triggers: `USER_DECISION` for product or domain meaning,
  acceptance or scope changes, architecture, public contracts, schemas,
  migrations, auth or permissions, security policy, dependencies, test
  strategy, material verifier disagreement, or missing product context.
- Blocked triggers: `BLOCKED` for missing authority, credentials, access,
  external state, required skills, or out-of-scope prerequisites.
- Residual-risk rule: `RESIDUAL_RISK` is a finding route, not a checkpoint
  result. When the issue explicitly classifies the stated assumption as
  non-blocking and it does not weaken an acceptance criterion or highest-risk
  verification gate, record the risk and permit a `CLEAN` checkpoint result.
  Otherwise use `USER_DECISION` as the checkpoint result.
- Re-review rule: rerun correction-invalidated proof and update self-review or
  required independent review for the current candidate. Carry forward coverage
  only after checking continuing validity; broad or uncertain impact requires
  full re-review. Preserve finding IDs and dispositions.
- Churn threshold: escalate when the same root cause survives two correction
  attempts or the fix oscillates. Use a stricter issue-specific limit when risk
  warrants it.
- Final integration check: inspect the combined issue-base-to-current-head
  change. Extensive or an explicit independent gate requires final `$code-review`;
  checkpoint reviews do not replace that initial full integration review.
- Goal behavior: a durable goal supplies persistence only while an authorized
  transition exists. It never changes a finding verdict or route, expands
  correction authority, or permits crossing a non-clean checkpoint.
- Completion target: current-head acceptance evidence, current self-review or required independent
  review, green required automated CI, current Issue Completion Record, and a
  verified merged PR with required human-review tracking and accurate release
  state, or the explicit narrower target. Pending post-merge human review alone
  does not block completion or later shipping.
```

Tighten the default contract for the issue's risk, but never broaden automatic
authority. A durable goal supplies persistence, not permission to resolve a
material ambiguity. Independent Reviewer contexts remain read-only; the
Delivery Operator owns authorized edits, validation, publication, CI, merge,
and already-authorized release follow-through. Read
[Delivery and Human Review](../../engineering-for-certainty/references/delivery-and-human-review.md)
for exceptions, queue reconciliation, and human outcome handling.
