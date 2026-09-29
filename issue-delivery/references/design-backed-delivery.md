# Design-Backed Delivery

Read for issue delivery governed by an authoritative design. Follow the referenced frontend design procedure as well.

For design-backed frontend work, read and follow `$engineering-frontend`'s
[Design Conformance And
Audit](../../engineering-frontend/references/design-conformance.md). Before judging
the implementation, re-fetch the authoritative source and compare its relevant
version or node signature with the approved issue baseline. Do not overwrite the
baseline merely because the source changed. Route relevant source changes as
`DESIGN_DRIFT` and pause for the issue revision or user decision required by the
contract.

Exercise every applicable Design Audit Matrix row against the running exact
candidate. Capture the implementation, perform the required side-by-side and
overlay or image-diff comparisons, exercise designed interactions and
accessibility, and record one typed outcome per row. Regenerate a defective
image or HTML/Tailwind reference only when the approved design proves that the
artifact is wrong; never change production code to match a defective export.
Keep the issue `Needs Verification` while any required source check, artifact,
row, or comparison is missing, failed, inaccessible, or stale.
