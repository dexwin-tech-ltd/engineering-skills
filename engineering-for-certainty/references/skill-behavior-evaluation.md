# Evaluate Material Skill Changes Blind

Use this method when changing which decisions an agent makes, what it does
before declaring work complete, or how much work it is expected to do. For a
wording change that leaves the expected action the same, check the guidance
through the normal review path.

## Set Up the Comparison

1. Write the decision the comparison should inform. Choose at least one
   representative task and one meaningful edge case. Start from synthetic data
   or an existing fixture approved for this use.
2. Before any run, define the required outcomes, executable checks where
   practical, disallowed changes, and the rubric for evidence that needs
   judgment. Record the time and cost limits, when a resource difference
   matters, and the rules for a tie or an incomplete run. Apply the same rules
   to both variants.
3. Pin a fixture revision. Make isolated copies with the same input for each
   paired case and use a fresh context for every run. Keep the ordinary user
   prompt, model settings, permissions, available tools, and run budgets
   identical. Verify that the guidance is the only difference in each pair.
4. Assign opaque labels and keep the label-to-variant key with the operator.
   Give the reviewer the common task, rubric, and labeled artifacts or their
   test results. Keep variant names and identifying metadata out of the
   review material. Record the scores before revealing the key.

## Judge and Record the Result

- Score actual outputs, relevant tool actions, and changes to the fixture.
  A wrong result or disallowed change fails its required check even when the
  answer is well written. Use presentation quality only where the rubric calls
  for it.
- Report the candidate as better only when it meets the predeclared improvement
  rule and passes the required checks. Use a tie for equivalent results under
  the rubric. Treat missing runs, broken controls, or mixed evidence that does
  not settle the question as inconclusive. Record which case drove the result.
- After unblinding, record the fixture revision, exact guidance variants, run
  settings, checks and outcomes. Include elapsed time, tool actions, and cost
  where actually measured. Label unavailable metrics explicitly. A small trial
  supports a conclusion about those cases. Use representative tasks before
  generalizing to routine delivery.
- Keep only the evaluation's relevant, sanitized evidence and summary.
  Limit collection to the evaluation's own outputs. Exclude secrets and
  unrelated conversation history. Record live delivery trends separately
  under [Rollout Evaluation](rigor-and-verification.md#rollout-evaluation).

## Worked Example

Ask how many lines in `status.txt` equal `done` exactly. Use the following
fixtures to compare an ordinary scope instruction with a candidate that asks
the agent to read the file and report a missing file explicitly:

| Case | Fixture | Required outcome |
| --- | --- | --- |
| Normal | Three lines containing `done`, `pending`, and `done` in that order | Return count 2 and leave the files unchanged. |
| Missing input | `status.txt` is absent | Report that the count is unavailable and leave the files unchanged. |

For each paired run, check the count or unavailable result against the fixture
and compare the files before and after. Show the reviewer two opaque labels
with those artifacts, keeping the guidance assignments sealed until scores are
recorded. Identical passing outcomes tie. A convincing count of 3 fails, and
a missing run makes that comparison inconclusive.
