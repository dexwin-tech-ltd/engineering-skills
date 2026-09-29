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

- Delivery mode: `$issue-delivery` to a ready-to-merge handoff.
- Review checkpoints: `<none; treat the issue as one delivery unit>` or
  `<ordered checkpoint IDs and outcomes>`.
- Automatic transitions: implementation -> checkpoint validation ->
  checkpoint review -> authorized corrections -> checkpoint revalidation and
  re-review -> next checkpoint -> final full validation and integration review
  -> pull request -> CI follow-through.
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
- Re-review rule: after every correction batch, re-run invalidated proof and
  obtain a fresh independent review of the full resulting issue-base-to-current-
  head change; preserve finding IDs and dispositions.
- Churn threshold: escalate when the same root cause survives two correction
  attempts or the fix oscillates. Use a stricter issue-specific limit when risk
  warrants it.
- Final integration review: checkpoint reviews do not replace a final
  `$code-review` of the complete issue-base-to-current-head diff.
- Goal behavior: a durable goal supplies persistence only while an authorized
  transition exists. It never changes a finding verdict or route, expands
  correction authority, or permits crossing a non-clean checkpoint.
- Completion target: current-head acceptance evidence, clean independent
  review, green required automated CI, current Issue Completion Record, and a
  truthful pull request with only human approval and merge remaining.
```

Tighten the default contract for the issue's risk, but never broaden automatic
authority. A durable goal supplies persistence, not permission to resolve a
material ambiguity. Independent Reviewer contexts remain read-only; the
Delivery Operator owns authorized edits, validation, publication, and CI
follow-through.
