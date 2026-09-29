# Finder Angles

Use A and B on every relevant changed file; use other angles when their surface is affected. At High and Max effort, every non-generated changed file receives A and B coverage.

### Plan finder work packets

Treat the finder angles as coverage obligations, not worker identities. Apply
conditional angles only when their trigger exists, and build a work packet map
before starting finders.

Prefer these packet types:

- **Coverage shards**: coherent changed modules, domains, or file groups. Every shard receives line-by-line correctness, removed-behavior, and testing-quality coverage.
- **Contract tracing**: changed exports, endpoints, schemas, errors, database shapes, environment variables, consumers, tests, and deployment manifests.
- **Triggered specialist passes**: security, data integrity, resilience, observability, frontend, or performance when the changed surface activates that doctrine.
- **Design-audit specialist packet**: for design-backed frontend work, inspect
  the authoritative source and approved baseline, detect relevant design drift,
  run the applicable state-and-viewport matrix scope against the exact running
  candidate, and classify every required row before queue finalization. Use the
  checkpoint scope in Checkpoint Review Mode and the complete issue matrix for
  the final integration review or an ordinary full review.
- **Style and clarity pass**: validation contracts, named and visible data flow, control flow, comments, and consistency within the changed concern.
- **Global consistency pass**: reuse, simplification, sibling behavior, architectural ownership, and cross-shard invariants.

A work packet may cover several related angles, and an important angle may appear in several packets. Do not isolate a deletion from the old behavior it provided or isolate a contract from its callers and deployment consumers.

At High and Max effort, start genuinely independent packets concurrently when their benefit exceeds handoff and reconciliation effort and the selected execution mechanism permits. Require every non-generated changed file to receive Angle A and Angle B coverage regardless of how packets are divided.

### A. Line-by-line correctness

Inspect every hunk and its enclosing function or module. Look for inverted conditions, off-by-one behavior, missing awaits, falsy-zero or empty-value bugs, copy-paste errors, swallowed failures, invalid state transitions, wrong defaults, and incomplete branches.

Report unchanged code only when the change makes it newly reachable, changes its inputs or ordering, removes a protecting invariant, changes its lifecycle, or expands its trust boundary. Attach the candidate to the closest causal changed line when possible.

### B. Removed-behavior audit

For every meaningful deletion or replacement:

1. Name the validation, guard, cleanup, error mapping, ordering, side effect, or invariant the old code provided.
2. Locate where the new code re-establishes it.
3. Create a candidate when the behavior disappeared, weakened, or moved behind a narrower condition.

### C. Cross-file contract tracing

For each changed exported function, endpoint, event, schema, database shape, error contract, or public type:

- Inspect callers, consumers, callees, and relevant adapters.
- Compare old and new preconditions, return shapes, nullability, error behavior, timing, and side effects.
- Check sibling implementations for inconsistent updates.
- Check whether tests exercise the real integration boundary.
- For a changed environment or configuration schema, also trace it to every deployment manifest that assigns those variables: CI/CD workflow files, Compose/Kubernetes manifests, and other infrastructure-as-code. Confirm each supported deployment-environment value has at least one config value that satisfies the schema; a refinement that rejects every legal value under one environment is a boot-breaking defect, not a style issue. Separately check whether a manifest's variable-substitution syntax (for example `${VAR:-}`) can produce an empty string when a variable is merely unset — most schema validators treat an empty string differently from an absent one.

### D. Security and authorization

Inspect trust boundaries, authentication, authorization, tenant isolation, input validation, output sanitization, secrets, injection risks, unsafe deserialization, session or token handling, and privilege changes. Trace actor context and permission checks end to end rather than inferring safety from route names or UI restrictions.

### E. Data integrity and persistence

Inspect schemas, migrations, transactions, uniqueness, foreign keys, destructive writes, partial updates, backfills, ordering, serialization, precision, compatibility, and rollback behavior. Look for durable corruption or loss paths as well as immediate failures.

For operational or administrative tooling—such as migration, rotation, cleanup, or batch commands—that accepts a caller-supplied scope, verify it distinguishes an invalid or nonexistent scope from a valid scope with no remaining work. Establish scope existence before processing, or return distinct outcomes and exit statuses for `scope_not_found`, `already_complete`, and `completed`. For iterative tooling, also verify that zero matches or lack of progress cannot be mistaken for successful completion.

### F. Reliability and concurrency

Inspect retries, timeouts, cancellation, idempotency, queues, webhooks, cron, races, atomicity, resource cleanup, error recovery, and partial failure. Verify whether an operation can be repeated safely and whether failure leaves state recoverable.

### G. Reuse

Search the repository before claiming duplication. Name the existing helper, abstraction, or shared module and explain the material divergence or defect risk caused by reimplementation.

### H. Simplification and state modeling

Look for redundant or derivable state, near-duplicate branches, contradictory booleans, deep nesting, dead code, and unnecessary indirection. Create a candidate only when the complexity causes credible defect or maintenance risk; name the simpler form.

### I. Performance and resource lifetime

Inspect redundant work, sequential independent calls, unbounded queries or loops, hot-path and startup cost, repeated parsing or allocation, N+1 behavior, cache invalidation, memory retention, and closures that capture large scopes. Tie the concern to a realistic workload.

### J. Observability and operations

Check whether failures can be detected, attributed, and diagnosed. Inspect structured errors, logs, metrics, traces, correlation context, redaction, alerts, and recovery signals when the change alters operational behavior. Do not demand instrumentation unrelated to the changed risk.

### K. Testing quality

Check whether tests cover the changed contract, success paths, expected failures, boundaries, and regression scenario. Detect tests that mock away the disputed behavior, assert implementation details, pass vacuously, or omit real wiring. Treat missing tests as supporting evidence for a behavior risk, not automatically as a standalone finding.

For observable runtime changes, independently audit the applicable real-boundary
proof against accepted outcomes and important integration seams. Formal issues
require their full Runtime Acceptance Plan, including the primary journey and
targeted exploration. Check that evidence belongs to the reviewed revision and
environment, that automated scenarios directly assert the claimed outcomes,
that necessary proxies and design deviations are explicit, and that later
changes did not make the evidence stale. Do not infer runtime correctness from
a generic green suite or the implementer's completion summary.

### L. Architectural altitude and conventions

Look for special cases bolted onto shared infrastructure, workarounds that bypass existing abstractions, and fixes that leave the same invariant broken for sibling consumers. Name the deeper mechanism that should own the behavior. Report convention violations only when an applicable rule can be cited and the violation is material.

Use the required companions for their specialist finder and verification
passes. Also increase review depth automatically for migrations, destructive
persistence, public APIs, caching, hot paths, and other high-impact surfaces
that do not belong to a companion skill.

### M. Style and clarity

Run this angle for every review. Read and follow [Style and
Clarity](../../engineering-for-certainty/references/style-and-clarity.md). It is a
required finder and verification obligation, not a formatting pass or source of
subjective suggestions. Create a candidate only for an applicable convention
violation or a concrete clarity failure with a credible maintenance, misuse, or
defect consequence. Verify every survivor through the normal candidate
pipeline; broader advice is allowed only when the user explicitly asks for a
style-focused review.

### N. Design conformance and engineering integrity

Run this angle whenever an authoritative design applies to a changed frontend
surface. Follow [Design Conformance And
Audit](../../engineering-frontend/references/design-conformance.md) as the canonical
procedure.

- Independently re-fetch the exact design source and compare its relevant
  version or node signature with the approved issue baseline before judging the
  implementation.
- Inspect the running exact candidate, not only screenshots, generated
  HTML/Tailwind, component harnesses, or the implementer's summary.
- Complete the applicable Design Audit Matrix scope, including affected shared-
  component, token, asset, responsive-rule, and integration-seam coverage. In
  Checkpoint Review Mode, use the checkpoint-owned and risk-expanded scope from
  the canonical procedure; otherwise audit the complete issue matrix.
- Treat accessibility, required operational states, design-system contracts,
  and platform conventions as engineering integrity, not subjective redesign.
- Give each row one typed outcome. Only a verified, reachable, material
  `IMPLEMENTATION_MISMATCH` enters the normal finding queue. Route design drift,
  design conflict, missing evidence, and reference-export defects according to
  the reference instead of misreporting them as code defects.
- At High or Max effort, use an independent verifier to repeat the complete
  applicable audit scope, including rows initially marked conformant. Broad
  design-system, global-token, or responsive-rule changes default to High
  design-audit effort unless repository evidence safely bounds their impact.
