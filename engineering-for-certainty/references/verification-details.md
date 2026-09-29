# Verification Details

Use the relevant section when work changes tests, API endpoints, observable runtime behavior, persisted schema or data, or a high-impact invariant that may need mutation analysis.

## Testing Doctrine

Use test-first development for Critical invariants and material behavior where a
failing test can express the requirement. For Standard work, prefer it when it
clarifies the behavior. Quick work needs direct targeted proof, but not a new
test for a trivial edit. Follow any stronger repository or approved-issue rule.

Apply testing rules in this order: critical behavior correctness first, then failure and validation coverage, then naming and structural consistency.

- When test-first work is required or selected and a useful seam exists: write the failing test first, implement the minimum to pass, then refactor with tests green.
- When test-first work is required but unsuitable, state the concrete reason and alternative proof before implementation.
- When setting up repo automation, prefer commit-time hooks for fast checks and push-time hooks for broader suites, while keeping CI as the authoritative full-environment validation.
- Cover success paths, failure paths, validation failures, error variants, and exhaustive mapping.
- For API endpoints, test status codes, response payloads, and actionable error details.
- API integration tests are mandatory for endpoint changes. Exercise the real app wiring end-to-end through route, service, and persistence layers; mock only true external systems at the boundary.
- When work touches observability, resilience, auth/security, or frontend engineering/accessibility, apply the relevant companion skill and test those behaviors explicitly.
- Follow the repo's local test declaration and naming style. If none exists, use
  `test()` with multiline given/when/then test names.
- For debugging: first build a deterministic repro loop. Convert the minimized repro into a regression test before fixing when a valid seam exists.
- Test file naming should mirror production file naming, such as
  `[domain].route.test.ts`, `[domain].service.test.ts`, and
  `[domain].repository.test.ts`.
- Structural migrations must preserve behavior and prove that with tests.
- Keep test structure aligned with the real module structure.

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
