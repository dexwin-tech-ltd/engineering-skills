# Performance Verification

Use this method when making or acting on a performance claim.

1. Pin the baseline and candidate. Record the command, metric and unit,
   representative workload and dataset, environment and settings, expected
   outputs and work counts, and a reasonable noise band. Use the same setup
   on both sides and declare the run budget and stop condition up front.
2. Confirm both sides completed equivalent work. Check relevant outputs,
   work counts and errors before comparing timing. Reject a comparison when
   work is skipped, ends early, or differs in a way that explains the result.
3. Take comparable paired or interleaved samples to reduce order effects.
   Report sample counts, observations or spread, and whether the difference
   exceeds ordinary variation. Say when the result is inconclusive. Label
   a single quick observation as an estimate.
4. Profile separately when instrumentation changes timing. Report which
   bottleneck the profile suggests and whether it accounts for the measured
   result. Use comparable unprofiled runs for the reported timing.

## Iterative Tuning

Freeze the harness, correctness checks, noise band, and stopping condition.
Test one supported hypothesis at a time. Keep an iteration when its measured
benefit exceeds ordinary variation and its relevant behavior and error
checks pass. Revert changes that fail either condition. In the existing task
record, capture material kept and reverted decisions and the evidence behind
them. Stop when the target or run budget is met, or justified hypotheses
run out.
