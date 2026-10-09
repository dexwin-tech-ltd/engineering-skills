# Issue Completion Record

### 12. Issue Completion Record

The rewritten issue must define its completion requirements. The issue is not
complete until the implementation or integration agent writes an **Issue
Completion Record** to the canonical issue file after final review and before
reporting the issue as done. A chat summary, pull-request description, commit,
or CI result may support the record but cannot replace it.

The record must contain:

- the final status and completion date;
- the last behavior-changing reviewed head to which acceptance, validation, and
  Runtime Acceptance evidence bind;
- the production, test, configuration, and documentation surfaces actually
  changed;
- the reconciled result of every traceability row, including exact validation
  commands and outcomes;
- every Runtime Acceptance scenario result, exact revision and environment,
  proxy blind spot, invalidated proof re-run, and linked downstream release
  gate; for design-backed work, include the baseline, source-drift result,
  Design Audit Matrix outcomes, approved deviations, comparison methods, and
  independent audit result;
- the final `$code-review` outcome and the disposition of every confirmed
  finding;
- when checkpoint reviews were used, the checkpoint ID, accepted head SHA,
  review range, review outcome, correction dispositions, and invalidated proof
  re-run for each checkpoint;
- deviations from the issue and any unplanned changes;
- residual risks and every unverified or deferred check, with a linked owner or
  trigger for downstream work;
- every eligible deferred finding, with a link to its repository backlog entry
  or exact existing owner and the evidence that made deferral safe; and
- branch, commit, and pull-request references when available;
- the shipping boundary, human-review selection and reason, and links to required
  queue tracking. Record verified merge and release state when available; the
  original PR owns later human outcomes and shipping reconciliation.

The record is an evidence index, not an evidence dump. Prefer compact tables and
durable links to raw CI, pull-request, test, or review evidence.

Keep implementation proof, shipping state, and human-review state distinct.
The pre-merge record may link the PR for later provider-confirmed merge and
release evidence instead of requiring a recursive post-merge evidence commit.
Pending human review alone does not reopen completed delivery.

Do not require the record to name the commit that contains the record itself.
When later commits change only the canonical issue, roadmap or index, backlog
entries, or completion evidence, record the last behavior-changing reviewed
head and require an independent current-head review to verify that every later
commit is evidence-only and invalidates no recorded proof.

The **Delivery Operator** owns the write-back. The final **Independent
Reviewer** must verify the completed record against the raw diff, test output,
and review evidence; the reviewer does not become the canonical issue writer. Update any
roadmap, index, or completed-work archive that tracks the issue's status in the
same write pass so those surfaces cannot contradict the issue.

Keep the issue at `Needs Verification` while any issue-owned acceptance,
review, or highest-risk verification gate lacks evidence. An explicitly
out-of-scope downstream or release gate does not block `Done` only when the
issue links it and names its owner or trigger. Never use `Done` with a caveat to
hide missing issue-owned evidence.
