# Verification Details

Use the relevant section when designing, adding, materially changing, or reviewing tests, or when work changes API endpoints, observable runtime behavior, persisted schema or data, important invariants across broad input spaces or action sequences, or a high-impact invariant that may need mutation analysis.

## Testing Doctrine

Use test-first development for high-impact invariants and material failures where
a failing test can express the requirement. Otherwise prefer it when it clarifies
behavior. Both modes require direct proof; reuse adequate existing coverage and
add tests where they give lasting protection, not merely to restate a trivial edit. Follow any stronger repository or approved-issue rule.

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
  code for every test. Apply the selective Mutation Analysis policy below.
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
requirement for every Extensive task. If the technique is unsuitable or another
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
  Preserve required integration, runtime, and independent review gates, plus
  any applicable explicit mutation-analysis gate under the policy below.

### Runtime Acceptance

Every formal issue with observable runtime behavior requires Runtime Acceptance
proof from current real-boundary evidence. Read and follow
[Runtime Acceptance Pass](runtime-acceptance.md) when this applies.
In either mode outside the formal issue workflow, exercise the assembled system
through its real boundary when local checks cannot prove the changed integration.
Extensive work must additionally prove affected high-impact invariants. Fast may
use focused direct checks when they establish the requested outcome.
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

Mutation testing is optional by default. Require it only for an applicable
explicit user, repository, or approved-issue gate, or a concrete unresolved gap
in whether tests detect consequential wrong behavior that needs mutation
analysis to establish sufficient assurance. Name the affected rule, suspected
wrong behavior, and what ordinary tests or review have not established. Base
the gap on actual assertions, a regression, or review evidence; imagining that
code could be mutated is not enough. If adequate direct tests or another
suitable technique resolve that gap, no extra
mutation gate is needed. Merely changing reusable logic, state transitions,
public contracts, money, authorization, or data integrity does not trigger it;
retain their required behavioral, integration, security, and review proof.
Extensive assurance does not require every testing technique.

Honor current project pauses and user overrides before selecting an audit; do
not resume mutation testing or add its tooling while a pause applies. A pause
does not waive required behavioral proof. Do not silently waive an applicable
explicit gate that has not been superseded by an authorized instruction.

- **Select and bound the audit:** state the rule, representative faults, affected
  tests, and an execution budget proportionate to the remaining uncertainty in
  the existing task or PR. Run after the unmodified baseline is green. Target
  the changed rule and relevant interactions; do not expand to a whole package
  or application without a concrete dependency or coverage reason. Preserve an
  established tool. Introduce tooling only when the expected value justifies
  setup; prefer StrykerJS for JavaScript/TypeScript when a new engine is useful.
- **Reuse valid evidence:** reuse prior results across stages and agents only
  after checking the relevant production code, tests and helpers, configuration,
  toolchain/dependencies, and environment. Use incremental analysis where
  supported, but do not assume its cache detects every invalidator. Rerun the
  affected scope after relevant changes; unrelated documentation or bookkeeping
  does not require repeating the audit. Run a clean broader audit only when a
  concrete concern, cache limitation, or explicit project policy warrants it;
  do not introduce a mandatory periodic full run or schedule by default.
- **Exercise behavior:** include applicable example and property tests in the
  selected scope. Use bounded, reproducible property-test settings for the
  baseline and mutants, and verify failure replay. Keep broader varied
  exploration separate. Strengthen observable-contract assertions or generators
  against meaningful findings, then rerun affected ordinary tests and the
  selected mutation checks; do not add implementation checks for specific
  mutants.
- **Review useful findings:** filter proven equivalent or irrelevant mutants
  early using transparent tool analysis or concise group-level reasoning.
  Review relevant survivors and uncovered behavior against the selected rule.
  A confirmed defect or unresolved required assurance gap still blocks under
  the governing proof and review gates. An unfinished optional sample does not
  create a blanket completion gate. Do not require a handwritten classification
  of every irrelevant mutant or a universal score; never suppress meaningful
  findings or exclusions merely to improve a score. Invalid mutants rejected
  by compilation or type checking are not test gaps.
- **Stop proportionately:** treat timeouts, runner errors, and exhausted budgets
  as inconclusive, never as passed. Stop an optional audit at its budget and
  record its partial evidence and remaining uncertainty without repeatedly
  rerunning the entire audit. A required audit or essential behavioral proof
  remains incomplete until resolved through affected revalidation or suitable
  alternative assurance; changing an explicit gate requires its authorized
  decision path. Known defects and unresolved consequential risks cannot be
  waved through because an optional audit ran out of time.
- **Keep evidence small:** record scope, budget, outcome, meaningful findings,
  reused evidence, and incomplete checks in the existing task, issue, or PR.
  Link a tool report when useful; do not create per-mutant documents, duplicate
  evidence bundles, or permanent mutant allowlists. Keep mutation analysis out
  of Git hooks. Add local or PR checks only for the selected audit or an explicit
  project requirement, without automatically making optional audits merge gates.
