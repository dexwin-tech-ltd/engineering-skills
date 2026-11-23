# Runtime Acceptance Pass

Use this reference for every formal issue that changes observable runtime
behaviour. Prove the assembled running system through the boundary a real caller
uses. An existing automated integration or end-to-end run may supply a scenario
when it exercises that exact boundary and candidate; reconcile its assertions
and revision before reusing it. Run an agent-operated scenario for any outcome
or integration seam that automation does not prove. Do not duplicate a passing
real-boundary run merely to repeat the same actions manually.

## Applicability And Sequence

- Require the pass for APIs, user interfaces, commands, workers, scheduled
  jobs, webhooks, migrations with runtime effects, and external integrations.
- A documentation-only or other non-runtime issue may record `Not applicable`
  with a concrete reason.
- Run applicable focused automated validation, then reconcile any current
  real-boundary scenario evidence and execute the remaining local scenarios
  against the final combined candidate.
- Repeat the pass against a preview or staging environment when it safely runs
  the exact candidate before merge and deployment is already automated or
  separately authorized.
- When staging receives only merged changes, record the staging pass as a
  mandatory downstream release gate with its owner and trigger. Do not imply
  that issue delivery authorizes deployment.

## Scenario Contract

The issue's Runtime Acceptance Plan must include:

- every accepted externally observable acceptance criterion;
- every materially distinct accepted outcome, such as success, validation
  rejection, expected failure, authorization denial, empty state, and retry;
- one complete primary user or system journey; and
- a targeted exploratory check of the changed area and its integration seams,
  automated when it directly covers the risk or agent-operated otherwise.

Automated tests retain exhaustive value combinations and low-level boundary
coverage. Do not manually repeat a large parameterized test matrix. For each
reused automated scenario, record the exact test, runtime boundary, candidate
revision, assertion, and observed result; a generic green suite is not enough.

## Real Boundary

- **API:** start the real application and send network requests through the
  public endpoint. Exercise routing, middleware, validation, authentication,
  serialization, and local persistence where applicable.
- **Web UI:** use browser control to navigate the running application. Check
  visible states and interactions, browser-console errors, and failed network
  requests.
- **Native or operating-system-dependent UI:** use computer, emulator, or
  device control appropriate to the supported platform.
- **Commands, jobs, webhooks, and scheduled work:** exercise the literal
  operational trigger when it is safely available.
- **External integrations:** use a provider sandbox or other real test boundary
  when available.

An in-process handler call, isolated component render, or mock adapter by itself
is not a Runtime Acceptance Pass. A running frontend exercised through a mock
adapter may prove an explicitly frontend-only slice when the adapter is named as
a proxy; it cannot prove backend integration. When the literal mechanism cannot
run safely, name the proxy, what it proves, its blind spot, and the later gate
that must exercise the real mechanism when the blind spot is material.

Apply `$engineering-auth-security` when authentication or disposable identities
are involved. Apply `$engineering-frontend` for UI execution and design
comparison. For design-backed frontend work, read its [Design Conformance And
Audit](../../engineering-frontend/references/design-conformance.md) reference
and require the approved baseline, Evidence Bundle, source-drift check, complete
Design Audit Matrix, and independent audit. Apply `$engineering-observability`
whenever a test message sink or telemetry boundary is involved.

### Reusable Project Verification Recipe

When a project lacks a reliable verification entry point or agents keep
rediscovering the setup, put a short recipe in its existing test documentation
or verification guide. Start with the project's launch scripts, automation,
and existing scenario tests. Use the recipe for the scenarios the current
change needs.

1. **Launch an isolated candidate.** Give the exact startup command, expected
   commit or build, environment, and deterministic test data. Explain how to
   choose an isolated port or workspace and how to identify the processes or
   resources the run creates. For authentication or messaging, follow the
   approved [Test Identity Plan and Test Message Sink rules](../../engineering-auth-security/SKILL.md#runtime-authentication-acceptance).
2. **Check readiness and identity.** Give the readiness probe, expected
   result, and time limit. Confirm that the usable instance and its inputs
   match the expected candidate before trusting its output. If it is
   unavailable or serving an older candidate, record what was observed,
   correct the startup or inputs, and rerun the affected checks.
3. **Exercise one focused scenario.** Name the real caller boundary, exact
   command or action, expected outcome or side effect, and the evidence to
   record in the [Evidence Ledger](#evidence-ledger). Reuse an existing run
   when it proves the same boundary, exact candidate, relevant environment
   and inputs, and required outcome. Run the missing scenario when existing
   evidence covers only part of it.
4. **Clean up the run.** Give the teardown command or other cleanup steps for
   the resources this run owns. Confirm they stopped or were removed and
   that unrelated resources are intact. Record any cleanup the run could
   not complete.

Update only the affected recipe steps when startup, readiness, selectors,
test data, or cleanup change. Reprove the affected scenarios under
[Staleness and Status](#staleness-and-status).

## Evidence Ledger

Record a concise evidence index rather than raw dumps:

| Scenario | Criteria | Revision and environment | Action or automated test | Expected and observed outcome | Result |
|---|---|---|---|---|---|
| `<stable ID>` | `<criterion IDs>` | `<commit/build + local/preview/staging>` | `<request, user actions, or exact test>` | `<sanitized outcome>` | `Pass` or `Fail` |

Add only the applicable supporting facts:

- preconditions and safe test-data setup;
- exact API method, path, sanitized request, status, and material response fields;
- UI viewport, platform, navigation steps, console and network result, and
  screenshots when visual proof matters;
- Test Identity Plan, Design Conformance Pass, Design Evidence Bundle and
  classified matrix, or proxy reference;
- cleanup performed; and
- unexpected observations, including non-blocking observations.

Never record passwords, OTPs, magic links, access or mailbox tokens, sessions,
cookies, private payloads, personal data, or unredacted logs.

## Staleness And Status

Tie every pass to the exact local commit or deployed build. A later production,
dependency, runtime-configuration, deployment, or test-data change invalidates
each scenario it could affect. Re-run those scenarios against the new head.
Documentation-only changes do not invalidate unrelated runtime evidence.

Keep the issue `Needs Verification` while required issue-owned runtime evidence
is missing, failed, or stale. A post-merge staging or other downstream release
gate may remain open only when the issue links it and names its owner and
trigger; that gate blocks the release rather than disguising missing issue-owned
proof.
