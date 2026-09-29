# Change Control During Implementation

Use when a discovery may alter an approved issue or plan.

## Mid-Implementation Discoveries

During implementation, do not silently expand the plan. Classify discoveries as
mechanical, material, or blocking.

Mechanical discoveries may be handled without user confirmation when they
preserve the issue's intent, behavior, architecture, and scope. Examples include
import fixes, local naming alignment, formatting, adapting to an existing
equivalent helper, or adding a narrowly required test fixture.

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
repo doctrine, or cannot satisfy its acceptance criteria as written.

For material or blocking discoveries:

1. Stop implementation at the smallest coherent point.
2. Report the discovery, why it changes the plan, affected files/tests, and 1-2
   concrete options.
3. Recommend one option.
4. Wait for user direction before continuing.
5. After approval, update the canonical issue or plan when one exists before
   implementing the revised scope.

Do not continue implementing a revised plan from memory or implication alone.
