# Verification Details

Use the relevant section when designing, adding, materially changing, or reviewing tests, or when work changes API endpoints, observable runtime behavior, persisted schema or data, important invariants across broad input spaces or action sequences, or a high-impact invariant that may need mutation analysis.

## Testing Doctrine

Use test-first development for Critical invariants and material behavior where a
failing test can express the requirement. For Standard work, prefer it when it
clarifies the behavior. Quick work needs direct targeted proof, but not a new
test for a trivial edit. Follow any stronger repository or approved-issue rule.

Apply the following quality rules to all new or materially changed tests.
They guide test selection and review without imposing a test count or written
justification for every assertion. Preserve required integration, E2E,
property-testing, mutation-analysis, runtime, and repository gates.

Prioritize critical behavior correctness, then meaningful failure and validation coverage, then naming and structural consistency.

### Behavior and Assertions

- Derive expectations from intended behavior or an independently established
  contract. Do not calculate expected results by duplicating production logic.
- Prefer assertions that survive changes to incidental implementation details.
  Do not add a test solely to repeat an internal constant, private structure,
  or production calculation. Direct checks of such details need a concrete
  reason, such as a value that is itself a published contract. Setup may still
  change when dependencies change.
- For an internal three-attempt limit, checking `MAX_ATTEMPTS === 3` does not
  prove enforcement. Check that the third attempt is allowed, the fourth is
  rejected, and rejection performs no protected action. A published protocol
  identifier can warrant a direct value check because the value is the promise.
- Assert required results and consequential side effects, including the absence
  of prohibited effects where relevant. Interaction assertions are appropriate
  when the interaction is part of the contract, such as requesting a charge
  exactly once; incidental helper calls and call order are not default contracts.

### Test Boundaries and Doubles

- Select boundaries from behavior and plausible failure modes, rather than
  requiring separate suites for every function, file, or layer. Use focused
  tests for meaningful local logic and wider tests for collaboration and wiring.
  Add overlapping coverage when it protects a distinct risk or provides useful
  fault isolation.
- Use test doubles at explicit dependency seams outside the behavior under
  test. Keep the relevant production logic running unchanged; do not replace
  the logic, collaboration, or mechanism the test claims to verify. Keep
  doubles consistent with relevant production contracts and distinguish
  simulated outcomes from real integration evidence.
- A controlled provider can expose a timeout or record charge requests. A fake
  repository returning "rolled back" cannot prove a real database transaction.
  Verify real boundaries when correctness depends on their behavior; the API
  integration requirement below remains a stronger specific rule.

### Cases and Reproducibility

- Choose cases from meaningful success and failure groups, relevant boundaries,
  and consequential combinations or action sequences. Cover applicable
  validation failures, error variants, and exhaustive outcome mappings. Check
  whether assertions reject plausible wrong behavior; preserving a sorted
  list's length alone accepts an unchanged unsorted list. Avoid redundant cases
  that add no distinct confidence. This does not require deliberately breaking
  code for every test. Triggered mutation analysis still applies.
- For debugging, build a deterministic repro loop and convert the minimized
  repro into a regression test before fixing when a valid seam exists.
  Demonstrate failure for the intended reason on the broken behavior when
  practical; otherwise state the limitation and alternative evidence.
- Make tests independently runnable and failures reproducible. Control relevant
  time, randomness, mutable state, and external responses according to the test
  boundary. Preserve varied property-test exploration and failure replay; do
  not eliminate exploration by permanently fixing one seed.
- Prefer bounded waits for observable conditions over arbitrary sleeps. Share
  infrastructure only when test isolation is preserved. Investigate flaky
  failures rather than routinely retrying until green. Any bounded retry for a
  known infrastructure transient must remain visible, with its reason and the
  original failure retained.

### Execution and Conventions

- When test-first work is required or selected and a useful seam exists: write the failing test first, implement the minimum to pass, then refactor with tests green.
- When test-first work is required but unsuitable, state the concrete reason and alternative proof before implementation.
- When setting up repo automation, prefer commit-time hooks for fast checks and push-time hooks for broader suites, while keeping CI as the authoritative full-environment validation.
- For API endpoints, test status codes, response payloads, and actionable error details.
- API integration tests are mandatory for endpoint changes. Exercise the real app wiring end-to-end through route, service, and persistence layers; mock only true external systems at the boundary.
- When work touches observability, resilience, auth/security, or frontend engineering/accessibility, apply the relevant companion skill and test those behaviors explicitly.
- Follow the repo's local test declaration and naming style. If none exists, use
  `test()` with multiline given/when/then test names.
- When a dedicated test file is useful, its naming should mirror production file naming, such as
  `[domain].route.test.ts`, `[domain].service.test.ts`, and
  `[domain].repository.test.ts`.
- Structural migrations must preserve behavior and prove that with tests.
- Organize tests around behavior and stable interfaces; keep them discoverable
  alongside the relevant modules without requiring one suite per module.

### Effect Testing

In new Effect backend/web projects prefer Effect test utilities and `@effect/vitest` for service and workflow tests; read `$engineering-effect` for Layer, scope, virtual-time, failure-channel, and property-test guidance. They complement real database/HTTP integration, the Migration Proof Harness, RTL, Expo/Jest, Playwright, Maestro, mutation analysis, runtime acceptance, and independent review. Preserve established test stacks. Diagnose flaky tests instead of routinely retrying until green.

### Property-Based Testing

Property-based tests assert a rule across generated inputs or action sequences.
Use them when changed behavior has an important invariant, a broad input space
or sequence of actions, and a practical test seam. Examples include allocation
and rounding rules, scoring bounds, normalization, serialization contracts,
and state transitions. Base the trigger on changed behavior, not a blanket
requirement for every Critical task. If the technique is unsuitable or another
technique provides sufficient assurance, record the concrete reason, relevant
input or sequence risks, and alternative proof before implementation. Cost
alone or a few passing examples does not establish sufficient assurance.

- Derive properties from the domain contract independently of the implementation.
  Do not duplicate production logic to calculate expected results. Review
  whether the properties would reject meaningful wrong behavior: preservation
  of an allocation total alone does not establish its distribution rules.
- Review generators alongside assertions. Exercise relevant boundaries,
  combinations, and valid inputs, plus invalid inputs where rejection is part
  of the contract. Avoid generators or excessive filtering that silently omit
  important cases; use explicit examples for known boundaries and regressions.
- Use shrinking to reduce failing inputs or action sequences, retain replay
  information supported by the tool, and preserve discovered defects as stable
  regression cases. If shrinking is unavailable, minimize failures explicitly.
  Control time, randomness, and external state enough to reproduce failures.
- Preserve established repository tools and budgets. Run the affected properties
  locally and in relevant pull-request checks with bounded execution. Expand
  exploration when impact, discovered failures, or input-coverage gaps warrant
  it; do not impose a universal case count or new scheduled suite. Avoid fixing
  one seed permanently for all exploration.
- Record affected properties, generator scope, execution budget, results, and
  replay information for failures in existing validation evidence. Generated
  cases provide evidence for the exercised space, not a proof for every input.
  Preserve required integration, runtime, mutation-analysis, and independent
  review gates.

### Runtime Acceptance

Every formal issue with observable runtime behavior requires Runtime Acceptance
proof from current real-boundary evidence. Read and follow
[Runtime Acceptance Pass](runtime-acceptance.md) when this applies.
Outside the formal issue workflow, Standard and Critical work should exercise
the assembled system through its real boundary when local checks cannot prove
the changed integration; Critical work must prove the affected high-impact
invariant. Quick may use the focused direct check from the rigor contract.
Record what each check proves and any remaining integration gap.

Complete the applicable scenarios locally before pull-request readiness. Repeat them against preview or
staging when that environment safely exposes the exact pre-merge candidate and
deployment is authorized. Treat post-merge-only staging proof as an explicit
downstream release gate. Re-run every scenario invalidated by a later change and
keep the work unverified while required issue-owned proof is missing or stale.

### Database Migration Proof

- Every database schema or data migration requires a dedicated Migration Proof Harness: an isolated disposable local database container or containerized test service that cannot target a shared or production database.
- Keep the harness local-only. Use a distinct Compose project, Testcontainers instance, database name, network, credentials, and teardown scope; do not include the proof service in production deployment manifests.
- Build the before-state from the prior released schema and representative persisted fixtures. Include an empty database plus boundary and legacy rows that exercise changed constraints, nullability, defaults, relationships, and transformed values.
- Run the exact production migration mechanism and generated artifacts. Do not replace it with hand-written setup SQL or direct schema synchronization.
- Assert the after-state: schema objects, retained and transformed data, constraints and indexes, application reads and writes, and any documented invariants.
- Verify the migration tool's supported repeat invocation or no-op behavior. Prove rollback only when rollback is part of the deployment contract; otherwise document the forward-recovery path.
- Record the prior-state fixture identity, migration command, assertions, container isolation, and results in the canonical issue or pull request.
- Do not require a database container for structural code migrations that do not change persisted schema or data.

### Mutation Analysis

- Use mutation analysis as a post-green adversarial audit. Start with
  behavior-first tests, reach green, then strengthen observable-contract
  assertions against meaningful survivors and rerun both ordinary and mutation
  suites; do not write mutant-specific implementation checks.
- Include applicable example and property tests. For property tests under
  mutation analysis, use a bounded, reproducible property-test configuration
  for the unmodified baseline and mutant runs; retain the configuration and
  verify failure replay with the chosen tool. Keep broader varied exploration separate
  and retain its discovered failures as stable regression cases. Investigate
  meaningful survivors for weak assertions or generator gaps. Property tests
  do not waive mutation triggers, survivor review, or required reruns. Their
  runtime cost does not justify silently excluding code or treating mutation
  timeouts as passes.
- Require it when a plausible small mutation could cause unauthorized access;
  incorrect money, entitlement, grading, ranking, or scoring; an invalid state
  transition or invariant; persisted-data damage; consequential validation
  failure; an incorrect public contract or exhaustive outcome mapping; or a wrong
  reusable domain decision.
- Base the trigger on impact, not directory, test type, or line count. Simple
  wiring, generated code, framework adapters, presentation-only code, and
  non-behavioral transformations are normally out of scope. When triggered, run
  the affected scope or record why the technique is unsuitable.
- Gate on reviewed-survivor completeness and non-regression, not a universal
  score. Establish a non-blocking baseline, prevent unjustified regression, and
  tighten it as meaningful survivors are eliminated.
- Classify every survivor. Meaningful survivors and uncovered mutants block
  completion; equivalent or irrelevant survivors require recorded justification.
  Require every survivor to be addressed, not every mutant to be killed.
- Store run-specific classifications in the canonical issue, pull request, or CI
  artifact, not a permanent mutant allowlist. Permit only narrow, justified,
  version-controlled structural exclusions; never exclude code or operators to
  improve the score.
- Treat timeouts, runner errors, and tool failures as inconclusive and fail closed
  until resolved or an alternative assurance technique is approved. Invalid
  mutants proven by compilation or type checking are not test gaps.
- Keep mutation analysis out of Git hooks. Run targeted local or agent validation
  and pull-request CI when triggered, plus a periodic clean full run to refresh
  the baseline and catch incremental-analysis blind spots.
- Preserve an established tool. Otherwise prefer StrykerJS for TypeScript and
  JavaScript, and the ecosystem-appropriate engine for other languages. Adapt its
  runner, monorepo, performance, and reporting configuration to the repository.
