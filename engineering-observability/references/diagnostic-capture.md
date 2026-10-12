# Diagnostic Capture And Filtering

Read when choosing logged values, processing errors, or proving diagnostic
usefulness. The common privacy boundaries in `../SKILL.md` apply to logs,
trace events, audits, crash reporting, and every fallback/export route.

## Error processing

Keep one shared bounded diagnostic shape for message, stack frames, cause
chains, normalized error type/code, and omission markers. The processor may
take an unknown thrown value; sinks receive only its filtered output. Normalize
an unrecognized error class to a stable fallback without discarding its useful
message. Handle cycles, multiple/aggregate causes, non-Error thrown values,
and size/depth limits without crashing the business operation or logger.

Do not require a per-source allowlist of human-readable phrases. Retain useful
prose from unknown errors after common filtering. Unknown enumerable object
properties, attached requests, provider configuration, and arbitrary runtime
objects are not automatically part of the diagnostic shape.

Use field-aware exclusions for structured values, removal of known secret
values where available, and source-aware filtering for formats that embed
inputs. A regex scan alone cannot establish that arbitrary text is safe.
Filter messages, stack first lines, nested/aggregate causes, validation issues,
database detail, URLs, and provider response excerpts, not just top-level keys.
If a known format embeds protected content that cannot be separated reliably,
omit that affected text portion, mark the reason, and retain safe frames,
codes, field paths, or surrounding context. Never put the removed text or a
secret-derived identifier in the omission marker.

Encode output structurally. Preserve readable stack-frame boundaries while
escaping record separators and control characters. Apply the same processor
before primary sinks, stderr/console fallbacks, SDK export hooks, and any
logger failure reporting; exporting first and scrubbing later is insufficient.

## Relevant values and personal information

Select context that answers why this operation behaved as it did: relevant
inputs, calculated outputs, provider response sections, retry state, state
transitions, versions, and timing. Keep a typed event/context contract and
explicit bounds; a shared error slot provides flexibility for unfamiliar
failures without permitting arbitrary context spreads.

Quantities, selected options, order amounts, currency, payment status, and
resource IDs are not prohibited merely because a user supplied them or they
relate to a business transaction. Apply the content rule to the actual value:
an option containing health information or an ID containing an email is still
protected or personal. Query strings, schema issues, and SQL parameters may
contain useful values, but require selection and filtering rather than blanket
capture. Full request/response capture requires an operation-specific diagnostic
reason; a dump of the entire checkout is not justified by a stock mismatch.

For names, emails, phones, and addresses, document the investigative question
that requires the actual value. Prefer internal IDs, masked values, field
paths, or comparison results when they answer it. Apply the purpose to every
copy, including error text; do not mask a structured field while leaking the
same value through its error message. Necessary identifier capture follows the
project's normal scoped log access and retention, not a per-incident approval.

Private communications, document/file contents, health details, government
identifiers, and full payment credentials remain outside routine logging.
An explicit investigation decision for protected content must identify its
scope, readers, capture route, and deletion deadline; it cannot waive the
credential exclusion. Ordinary log access is not authorization for that extra
capture or for exporting customer information into a ticket or another tool.

## Worked investigation cases

These are fictional policy examples, not captured logs or executed runtime
tests. Illustrative output fields fit inside a consuming project's bounded
schema; omission markers never contain the removed values.

### Unfamiliar provider failure

Before: `provider_failure`, request ID, and deployment version locate the
incident but do not explain it.

After: retain the filtered message `Unsupported API version 2025-01`, the
calling stack, provider status/code, selected API version, retry count, and
trace/job/resource IDs. Remove credentials from headers, endpoints, and nested
causes. An unfamiliar SDK error does not require a new adapter before its
message can be retained.

Investigative outcome: identify the version mismatch and the responsible call
without collecting the complete provider request or response.

### Validation and identifier normalization

Default: `Invalid email "[REDACTED]" at customers.email` retains the validation
rule, field path, and `createCustomer (customer-service.ts:84)` frame. Do not
retain the email solely because the validator quoted it.

For an email-normalization failure, a contract may permit the submitted and
provider-bound addresses when masked values or comparison facts cannot explain
the mismatch. Capture only those necessary identifiers, the transformation
stage, and correlation IDs; passwords and authentication links remain excluded.

Investigative outcome: distinguish invalid input from an application
transformation, without routinely retaining all form fields.

### Database conflict with embedded values

Before: SQLSTATE `23505` identifies a uniqueness conflict but not necessarily
which invariant failed.

After: retain SQLSTATE, constraint/table/column identifiers, filtered message,
stack, cause, and relevant operation/resource IDs. A driver's detail that quotes
an unnecessary email has that value removed; if its text cannot be separated
safely, omit the detail and mark it while retaining the other evidence. Select
a query template or parameters only when they answer the investigation and
their contents meet the same policy. Do not dump the driver object.

Investigative outcome: identify the conflicting invariant and operation while
excluding echoed protected values.

### Calculation and state mismatch

For `order.stock_check_failed`, retain `productId`, `requestedQuantity: 3`,
`availableQuantity: 2`, and `inventoryVersion: 41`. For a pricing mismatch,
retain selected quantity, unit amount, currency, discount rule/version, and
computed total. These are relevant business values, not a reason to retain
the customer's address or complete checkout payload.

Investigative outcome: reconstruct the decision or calculation from the logged
values and distinguish stale state from incorrect arithmetic.

## Adoption proof

For materially changed logging behavior, exercise representative failures
through the actual changed boundary and inspect final emitted records. Prove
that the investigator can explain the failure, locate the code, follow causes,
reconstruct relevant decisions, and connect records and executions. Verify
omission/truncation markers and visibility of sampling or aggregate drops.
Exercise a fallback when it changes. Use seeded credentials and protected
content inside nested errors and payloads to test every affected export route.

Choose cases from the real sources and risks; the examples above are a starting
point, not a mandatory new suite for every unrelated code change. Policy review
and worked cases prove guidance coverage, not a project's runtime behavior.
Projects adopting the policy must verify their formats, SDK hooks, source maps
when needed, access, deletion behavior, and volume/cost for the declared
retention window. No universal regex or documentation check proves those facts.
