---
name: engineering-observability
description: Explicit observability doctrine for useful diagnostic logs, metrics, traces, audits, redaction, correlation, client ingestion, retention, and sink failure. Use when work touches operational telemetry, error capture, logging privacy or access, or secret-bearing test message sinks.
---

# Engineering Observability

Use this companion skill with `$engineering-for-certainty` when work touches operational telemetry. Apply the common signal-safety contract below and read the references triggered by the changed behavior before verification.

## Scope

Apply this skill to:

- centralized logger setup or configuration
- backend or frontend logs, metrics, traces, alerts, and audit records
- trace instrumentation, span attributes, context propagation, sampling, or export
- redaction, sanitization, retention, and log access
- correlation or request ID propagation
- database, provider, SDK, queue, job, or infrastructure failure logging
- frontend operational event or span emission and client-to-backend ingestion
- separation of authentication Test Message Sinks from logs and telemetry

## Centralized Configuration And Signal Boundaries

- Configure resource metadata, privacy policy, exporters, sampling, and health centrally while preserving signal-specific contracts.
- Send logs and audits through typed event schemas, metrics through named instruments with bounded attributes, and traces through an approved tracer provider, instrumentation, processors, and exporters.
- Inject the logger, tracer, meter, or a narrow signal-specific facade into services and repositories; do not import hidden telemetry singletons into domain code.
- When a bespoke logger, tracer, or facade object is hand-built for a script, CLI, or test harness instead of using the shared implementation, verify it forwards every parameter its declared interface accepts. A structurally-typed shim can satisfy an interface while silently discarding an argument, dropping all structured context from every call site that relies on it.
- Accept typed Safe Log Events with explicit diagnostic fields. A shared error processor may accept an unknown error as input; normalize and filter it before any sink, exporter, or fallback receives it. Never forward raw error or request objects, arbitrary context bags, or spread objects directly.
- Define a closed schema per event family, reusing shared diagnostic shapes rather than approving every error phrase or creating an adapter for every unfamiliar failure. Reject unknown envelope/context keys and bound accepted strings, arrays, nesting, cause depth, and total size. Give metrics and manual spans explicit attribute contracts.
- Keep logging out of business transactions and critical request completion. Logging failure must not change a successful business outcome unless the event is an explicitly authoritative audit requirement.
- When `NODE_ENV` is `production`, missing required configuration for the repository's selected logging sink or required tracing exporter is a startup error. When the repository selects Better Stack, this includes its required Better Stack configuration. In `development`, default to console logging and a local or no-op tracing exporter when remote sinks are absent. Treat an unset `NODE_ENV` as development only in a verified local development environment.
- If the primary sink fails, use a bounded sanitized stderr or console fallback and surface safe aggregate health or alert signals when the repo supports them. Never recursively log a logger failure through the failing path.

## Useful Diagnostic Evidence And Safe Log Events

Ordinary production logs must help explain unexpected failures, including ones
not anticipated when their event schemas were written. Preserve useful error
messages, stack frames, cause chains, relevant input/output values, and business
state after filtering and size limits. Codes and metadata complement that
evidence; they do not replace its explanation.

For error processing, value selection, and worked investigation cases, read
[Diagnostic Capture And Filtering](references/diagnostic-capture.md).

Prefer controlled fields such as:

```text
event, level, service, component, operation, stage, outcome,
errorClass, errorCode, correlationId, requestId, durationMs,
retryCount, environment, deploymentVersion, actorId, tenantId,
resourceId, traceId, spanId, jobId, error, context, diagnosticOmissions
```

- Use stable event and error codes alongside diagnostic explanations. Keep unfamiliar error messages through the shared processor; a missing source adapter is not reason to discard the explanation.
- Never persist credentials, API keys, bearer/session tokens or identifiers, passwords, OTPs, authentication cookies or headers, credential-bearing connection strings, or private encryption material. Extract a useful non-secret endpoint or configuration fact when needed rather than retaining the credential-bearing object.
- Keep private message bodies, document/file contents, health details, government identifiers, and full payment credentials out of routine logs. Capturing protected content needs a separate explicitly scoped investigation decision; this policy supplies no blanket permission for it.
- Routinely include relevant diagnostic values and selected request/response or provider payload sections. User-entered or form-supplied values are not automatically prohibited. Full payload capture needs a concrete diagnostic reason and the same filtering and limits; do not automatically dump headers, storage, console arguments, or object graphs.
- Prefer internal IDs or masked values for people. Names, emails, phone numbers, and addresses may be retained only when the actual value is necessary to answer a concrete investigative question; document that purpose in the event/capture contract, not through a fresh approval for each occurrence. Presence in an error is not sufficient reason. Treat an email used as a database key as personal information, not an opaque ID.
- Include relevant existing actor, tenant, and resource IDs routinely, plus execution IDs appropriate to the boundary. Derive authoritative context from trusted application state. Hashing IDs is optional when a project needs it, not a default investigation obstacle. Do not use these IDs or diagnostic prose as metric labels or span names.
- Filter sensitive portions rather than deleting whole explanations. When a known format cannot be separated safely, omit the affected portion and retain the remaining evidence. Mark filtering and truncation without reproducing the removed content. Free-text filtering is fallible; source-aware handling and realistic checks are required where errors echo protected values.
- Encode multiline messages and stacks as structured string fields with escaped control characters; prevent CR/LF or delimiters from forging records without destroying stack-frame readability.
- Real data keeps these privacy boundaries in every environment. Verified synthetic fixtures permit broader payload/state capture; usable credentials stay excluded. Development may enable debug detail and less sampling. Production still retains rich failure evidence under its declared capacity budget.

### Test Message Sinks Are Not Telemetry

An authentication Test Message Sink may expose an OTP, magic link, or test
message through a mail-catcher UI or API, provider test tool, or isolated
terminal output only under `$engineering-auth-security`'s non-production rules.
It is a delivery adapter, not an operational log or Safe Log Event.

- Keep its secret-bearing output out of logger, audit, metrics, tracing, error
  reporting, CI-log collection, and telemetry-export pipelines.
- Do not solve test-message retrieval by weakening redaction or allowing tokens,
  message bodies, mailbox credentials, sessions, or direct identity into a log
  schema.
- Disable or reject the sink in production and keep access and retention bounded
  in non-production environments.
- Runtime Acceptance evidence may record that the message was retrieved and
  accepted; it must not reproduce the secret or message body.

## Effect Integration

Apply `$engineering-effect` when wiring Effect logging and tracing. Configure logger/tracer services centrally through Layers. Route Cause/Exit, schema issues, SQL diagnostics, annotations, and query-logger output through the common diagnostic processor rather than serializing runtime objects directly. Preserve useful failure explanations, stack frames, mixed causes, and selected relevant values within the privacy and size contract. Verify automatic instrumentation and exporter behavior, keep sink failure separate from business success, and preserve controlled frontend ingestion and sampling.

## Source-Specific Failure Adapters

Use the shared processor for unfamiliar failures. Add small source-specific adapters for database, HTTP/provider, queue, job, validation, and platform errors when their formats need value-aware filtering or useful field extraction. Unknown object properties never pass through automatically; unfamiliar diagnostic prose can survive filtering in the shared error shape.

For a recognized PostgreSQL driver error, include these bounded normalized metadata fields when present, alongside its filtered diagnostics:

```text
db.sqlstate
db.schema
db.table
db.column
db.constraint
```

- Read `code`, `schema`, `table`, `column`, and `constraint` from machine-readable driver properties and normalize their log field names.
- Prefer machine-readable properties over parsing prose for database identifiers.
- Preserve filtered message, stack, cause, and useful positions. Select `detail`, `hint`, query templates, or parameters only when diagnostically relevant; database prose and values frequently echo personal or protected data and need source-aware filtering. Do not serialize the complete driver object or query context.
- Apply the personal-information rule to database object names derived from people; tenant/resource identifiers may be retained when relevant.
- Keep missing metadata normal. Preserve the explanation through the shared diagnostic shape rather than widening arbitrary driver-property capture.

Output-validation failures and unexpected infrastructure failures must produce a Safe Log Event through the shared processor and any applicable source adapter.

## Tracing And Correlation

Tracing is required when the work explicitly changes tracing; crosses services, processes, queues, webhooks, or other asynchronous boundaries; changes a critical multi-dependency path that logs and metrics cannot diagnose; or extends a path already traced by the repo. For tracing activation, span contracts, W3C propagation, or correlation ownership, read [Tracing And Correlation](references/tracing-and-correlation.md).

## Client Telemetry

For frontend operational events or spans, ingestion, or direct crash reporting, read [Client Telemetry](references/client-telemetry.md). Structured operational events and traces use controlled backend ingestion; the reference defines the narrow direct crash-reporting exception.

## Sampling, Delivery, Capacity, And Retention

For tracing or any changed sink, exporter, queue, sampling, capacity, timeout, or retention policy, read [Sampling And Delivery](references/sampling-and-delivery.md). Telemetry failure must not fail a business operation unless an audit event is explicitly authoritative.

## Testing And Review

- Test production versus development startup behavior for every required configured sink where a valid seam exists.
- Test event schemas, unknown-key rejection, length and size limits, log-injection sanitization, and correlation propagation.
- For material logging changes, verify representative investigations using final emitted records: what failed and where, the causal chain, relevant values/state, linked people/records/executions, and any filtering, truncation, sampling, or drops. Exercise the affected real pipeline and a changed fallback; schema checks alone do not prove usefulness. Policy-only changes use worked cases and consistent guidance; consuming projects must supply runtime proof when adopting it.
- Seed excluded credentials and protected content into nested errors, requests, database parameters, provider responses, and frontend payloads; prove they do not reach sinks, traces, audits, fallbacks, or exporter diagnostics. Verify unnecessary personal identifiers are masked/omitted and necessary permitted identifiers survive. Confirm filtering retains the useful explanation, including for unfamiliar errors.
- Test every source adapter's allowed and forbidden fields, including missing database metadata.
- When a Test Message Sink exists, prove it is unavailable in production and
  cannot forward secret-bearing content into logs, audits, metrics, traces,
  errors, CI artifacts, fallbacks, or exporters.
- Test frontend event and trace ingestion authentication, reduced anonymous contracts when present, forged identity and authority, excluded-data smuggling, schema/version rules, diagnostic filtering, span/event/attribute/link limits, batch and body limits, rate limits, and generic failures.
- Exercise the real changed HTTP, queue, job, webhook, or client-to-backend boundary where a valid integration environment exists. Prove W3C context injection/extraction, correct parent or link relationships, root-span creation, semantic names and kinds, bounded attributes, and the final exported trace.
- Test error and expected-outcome semantics, parent-based sampling, unsampled application correctness, and that client trace export reaches only the controlled backend.
- Load-test sustained traffic, bursts, malicious clients, repeated frontend-error loops, a full queue, sink timeouts, and provider outages against the Telemetry Budget.
- Test logger and trace-exporter failure, timeout, saturation, dropping, fallback, and bounded shutdown without recursion, unbounded memory growth, or business-operation failure.
- Verify per-signal retention, reader access, and authoritative audit ownership when the change affects them.
- Before completion, verify every triggered check or record its omission and alternative assurance in the `$engineering-for-certainty` handoff.
