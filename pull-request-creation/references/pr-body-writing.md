# PR Body Writing Guide

Use when composing or revising a PR description on either publication path.
Keep the description minimal while preserving applicable repository template
fields. Scope, proof, and readiness requirements still govern publication.

## Minimum useful description

Write a short summary of the problem and resulting behavior in the project's
own terms, followed by brief validation and material caveats when relevant.
Add an issue link when one exists. Omit empty sections and routine implementation
detail. Do not require diagrams, code tours, screenshots, or lengthy evidence
sections merely to explain the change.

State checks actually run and their results. Keep exact commands, tested revision,
environment, and detailed evidence in the existing handoff, issue, or CI record;
link them when useful or required. Never invent observations or hide required
missing proof behind a caveat. A minimal description does not lower validation,
review, merge, or repository gates.

Include a known limitation, affected dependency, or recovery constraint when it
materially affects review or use. Explain what reverting cannot undo when data
or external effects make that consequential. Avoid speculative risk lists.

Illustrative description; use only results supported by the actual handoff:

> Repeated export clicks created duplicate jobs. Matching requests now reuse the
> active job.
>
> Validation: export tests passed.
>
> Limitation: this does not address simultaneous requests.

If simultaneous requests are part of the approved scope, that missing proof or
behavior blocks readiness; mentioning it does not waive the requirement.

## Human review after merge

For selected work under
[Delivery and Human Review](../../engineering-for-certainty/references/delivery-and-human-review.md),
include the selection reason briefly. Keep the repository queue entry to its PR
link, brief description, and selection reason or review focus.

The human chooses how to understand the PR at review time. Support conversation
from the original diff and verified evidence; provide visuals or other artifacts
when requested or agreed useful. Do not automatically create a review webpage or
require a prepared explanation package. Existing walkthrough files are historical
artifacts with no ongoing hosting, repair, or preservation obligation.

The original PR owns the explicit human outcome. Record merge identity and known
release state there as required by the delivery policy; never invent deployment
evidence. Agent checks, explanations, and elapsed time do not complete human review.
