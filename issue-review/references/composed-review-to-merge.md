# Composed Review-to-Merge Authorization

### Composed Review-to-Merge Authorization

When an explicit review-to-merge workflow invokes this skill, that invocation
supplies write authorization and shared-understanding confirmation only for a
coherent issue update derived entirely from verified findings, the existing
approved issue meaning, and discoverable repository conventions. Do not ask for
a redundant confirmation before that mechanical write.

If the update would choose or change product meaning, acceptance, scope,
architecture, public contracts, schemas, migrations, permissions, security
policy, dependencies, test strategy, planning-system structure, or priority,
route it to `USER_DECISION`, use `$grilling`, and require explicit confirmation
of the resolved issue before writing.

Mechanically capturing an eligible deferred finding in the repository backlog
with its verified root cause, effect, evidence, and origin is authorized when
those facts have one coherent interpretation. Creating root `BACKLOG.md` when
the repository lacks a canonical backlog is part of that capture, not an
invented planning system. Choosing among plausible product outcomes, broadening
beyond that evidence, combining independent outcomes, or assigning priority
remains a user-owned scope decision. Prepare a full issue only when the
follow-up is selected for work.
