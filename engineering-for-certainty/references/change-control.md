# Change Control During Implementation

Use when a discovery may alter an approved issue or plan.

## Mid-Implementation Discoveries

During implementation, do not silently expand the plan. Classify discoveries as
mechanical, eligible for the [Follow-Up Inbox](follow-up-inbox.md), material, or
blocking. Verify the discovery and its relationship to the approved issue
before changing code or selecting a route.

Mechanical discoveries may be handled without user confirmation when they
preserve the issue's intent, behavior, architecture, and scope. Examples include
import fixes, local naming alignment, formatting, adapting to an existing
equivalent helper, or adding a narrowly required test fixture.

An eligible follow-up is a confirmed, independently deferrable finding outside
the issue's promise. Keep a brief evidence note, continue the approved issue,
and file the deduplicated finding in the repository backlog at closeout under
the [Follow-Up Inbox](follow-up-inbox.md) contract. Promptly flag conspicuous
user-visible findings with a recommendation while continuing independent work.
Frequency or apparent rarity does not decide whether a finding can be deferred.

Material discoveries require pausing before further implementation. Pause when
the work would:

- Change acceptance criteria, user-visible behavior, public API contracts,
  schemas, migrations, permissions, auth behavior, error contracts,
  observability behavior, dependency choices, or test strategy.
- Touch modules, files, domains, or workflows outside the issue's affected
  surface.
- Add a new architectural pattern, new dependency, new persistence behavior, or
  new cross-domain orchestration.
- Convert a scoped issue into a broader refactor or product decision.
- Require choosing between multiple plausible product or domain meanings.

Blocking discoveries require stopping until the plan is corrected. Stop when
the issue contradicts current code, depends on missing prerequisites, violates
applicable repository doctrine for the current change, or cannot satisfy its
acceptance criteria as written. Correct an applicable required doctrine
violation in changed code when the fix is mechanical and in scope; use the
material decision path when correction would change approved meaning or scope.
An unrelated existing violation may be deferred only when the current issue
remains independently releasable. An optional style preference without a
concrete consequence is not a finding.

For material or blocking discoveries:

1. Stop implementation at the smallest coherent point.
2. Report the discovery, why it changes the plan, affected files/tests, and 1-2
   concrete options.
3. Recommend one option.
4. Wait for user direction before continuing.
5. After approval, update the canonical issue or plan when one exists before
   implementing the revised scope.

Do not continue implementing a revised plan from memory or implication alone.
