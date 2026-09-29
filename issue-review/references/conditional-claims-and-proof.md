# Conditional Claims And Proof

Read only the sections triggered by the issue. These details supplement the universal affected-surface, acceptance, dependency, and traceability gates in `../SKILL.md`.

## Platform Mechanisms And Runtime Configuration

When behavior depends on a platform or library mechanism, name the literal mechanism and its production owner. Do not accept abstractions such as "refetch," "refresh," "protect route exit," or "handle navigation" without the concrete API, hook, event, or interception point—for example, an imperative query call or History API interception—and the symbol or module that invokes it. Verify that the installed framework and version support the named mechanism.

When the issue introduces an environment variable that a runtime component reads directly, name the literal delivery mechanism, not only its presence in `.env` or `.env.example`. Trace the specific variable from its consumer—such as a workflow expression or `process.env` read—through every required injection point, such as a Compose `environment:` block or systemd `Environment=` entry. Do not infer its delivery from a superficially similar variable.

## Strong Acceptance Claims

When a criterion uses universal or negative language ("only", "all", "every", "never", "no other"), verify it cannot be satisfied by a weaker existential check. Either the issue enumerates the exact elements the claim covers and states that each must independently hold, or it says explicitly that the check is a spot-check and why that's acceptable. A criterion like "the file contains only placeholder values" is otherwise easy to implement as "at least one placeholder is present" — a check that passes even when most of the listed values are real.

When a criterion claims mutual exclusivity ("reachable from X, not from anyone else"), require independent verification in both directions: reject every disallowed side, and identify and accept the allowed side specifically rather than merely proving that some request succeeds. If the test method has a structural blind spot in either direction, state the blind spot and require a named compensating manual or automated verification step. For example, same-Docker-host traffic may use hairpin NAT and therefore cannot prove source-IP behavior across a genuine external network boundary.

When a criterion combines a formatted or rounded value shown to a user with a pass/fail or status indicator derived from the same raw quantity, require an explicit rule preventing disagreement at rounding boundaries. Derive both from the same displayed value, or state the precise reason divergence is intentional.

## Inherited Rules And Consumers

If this issue changes or extends a rule, convention, or domain concept that another issue file explicitly claims to inherit, match, or reuse (search sibling issue files for phrases like "match issue #N", "per issue #N", "same as #N", "inherits from #N"), or that is documented in `CONTEXT.md`/`CONTEXT-MAP.md`, open every referencing file now. Either reconcile them in this same review pass, or record an explicit follow-up issue to do so before this issue is marked ready — never leave a referencing issue or the glossary silently stale.

If the issue changes a config value, protocol, or URL with existing consumers, search for every consumer and inspect each one even when the expected conclusion is "no code change needed." State whether the change alters each consumer's failure modes, error classification, or default error state as well as its happy path. Pay particular attention to generic health checks, doctor commands, and catch-all error handlers, where a new expected failure can otherwise become a misleading operator message.

## Exact Mechanism And Integration Proof

When an acceptance criterion names a specific runtime mechanism (a "scheduled" job, a "background" retry, an "on reconnect" handler), the Test Approach must state whether verification exercises that literal mechanism or a named, justified proxy (e.g. a manual one-shot invocation of the same script the scheduler calls). An unstated substitution leaves a criterion looking tested when only an adjacent code path was actually exercised.

Apply the same rule to platform and library mechanisms named under Affected Surface: exercise the actual API, hook, event, or interception path, or name and justify the proxy and its blind spot.

When the issue introduces a new artifact covered by an existing repo-wide invariant, such as image pinning, network exposure, or secrets hygiene, inspect the shared test files or suites that encode that invariant. The Test Approach must name the existing assertions and explicitly extend them to the new service, image, port, secret, or other artifact rather than adding only scenario-specific tests.

When verification is split between standalone checks and a live or deployed instance, identify whether the issue-owned live-only portion contains the scenario most likely to expose an integration defect, such as cross-branch recombination, interacting failure paths, or multi-service behavior. If it does, the issue must remain explicitly unverified—use a status such as `Needs Verification`, not `Done` with a caveat—until that scenario has run successfully. When the environment can receive only merged changes, make the scenario an explicit downstream release gate with its owner and trigger; it blocks release rather than falsely claiming pre-merge proof.
