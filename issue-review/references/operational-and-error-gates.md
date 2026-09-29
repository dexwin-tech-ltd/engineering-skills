# Operational And Error Gates

Apply only the sections whose affected behavior is present. The universal issue gates and triggered engineering companions still apply.

### 9. Operational And Migration Safety

If the issue changes persisted data, generated artifacts, imports, migrations, deployment config, background jobs, or external integrations, state how the change is applied, rolled back or retried, and verified.

Every database schema or data migration must use the `$engineering-for-certainty` Migration Proof Harness. Require an isolated disposable local database container, the prior released schema, representative legacy and boundary fixtures, the exact production migration mechanism, after-state schema and data assertions, application read/write proof, and supported repeat invocation or no-op behavior. Require rollback proof only when rollback is part of the deployment contract; otherwise name forward recovery. Structural code migrations do not trigger the database harness.

If the issue adds persisted state alongside existing persisted state already enumerated in documentation, update every affected table, destructive-operations warning, backup or restore runbook, and blast-radius description. Documenting the new state in isolation does not pass when existing operational guidance would become false or incomplete. For example, a warning that says an operation destroys "both" named volumes becomes false when a third volume is added.

### 10. Observability And Errors

If the issue changes runtime behavior, state expected error behavior and any logging, metrics, audit events, or user-visible messages needed by the repo's conventions. If none are needed, say why.

When telemetry changes, require typed default-deny Safe Log Events. The issue must name each event family, allowed fields, source adapter, correlation ownership, retention and reader access, and tests proving that raw errors, arbitrary context, secrets, direct PII, and forbidden source-specific fields cannot reach logs, metrics, traces, audits, or fallbacks.

For frontend operational logging, require the client emission boundary and condition, deduplication or aggregation rule, production sampling or debug policy, bounded queue and delivery behavior, backend ingestion schema, authentication or reduced anonymous event set, rate and payload limits, non-recursive failure path, and Telemetry Budget. Product analytics and authoritative backend security or audit events remain separate.

When errors use a discriminated union or coded envelope, require a code-to-details matrix that names each error code, its compatible `details` shape, its producer, and each consumer's behavior. The Test Approach must cover every valid mapping plus missing envelopes, malformed envelopes, unknown codes, and code-incompatible `details`. State the fallback behavior for invalid combinations; do not let consumers trust `details` based on shape alone or a code alone.
