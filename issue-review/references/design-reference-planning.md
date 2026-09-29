# Design Reference Planning

#### Design Reference Baseline

For design-backed frontend work, create the approved baseline before declaring
the issue implementation-ready. Do not leave baseline capture to the
Implementation Worker or first code reviewer.

During the interview, retrieve and inspect the source read-only and resolve the
manifest, matrix, artifact set, and limitations without writing partial issue
state. After shared understanding is confirmed, write the finalized baseline,
Evidence Bundle, canonical issue, and required planning updates together as the
one coherent final issue write.

- Discover and preserve the repository's existing evidence convention. When
  none exists, use `<canonical issue directory>/evidence/<canonical issue
  stem>/design/`; the full issue stem excludes the `.md` extension.
- Retrieve the exact authoritative source with the available design tooling and
  record the file, page, frame, component, and node identities plus the version
  or strongest approved snapshot and source-signature method.
- Create frozen images for every issue-owned designed state and viewport.
- Create and validate the self-contained HTML/Tailwind visual reference,
  including for native targets, and add only the platform-specific supplements
  needed for behaviour it cannot express faithfully.
- Create the Design Audit Matrix covering every accepted state, platform,
  viewport, variant, interaction, operational state, and affected shared-
  design seam.
- Record approved deviations and known source-signature or export limitations.
- Keep the core issue concise: link the manifest and bundle instead of pasting
  raw design data or generated markup into the issue.

The finalized manifest and evidence references belong to the approved issue
revision and its Approved Issue Commit. When repository policy stores large
artifacts in Git LFS or an external artifact service, keep durable resolvable
references under the issue evidence path. If the exact source, required nodes,
stable baseline, frozen images, HTML/Tailwind rendition, or matrix cannot be
obtained and validated, the issue is not implementation-ready.

Do not treat an in-process handler call, component harness, mock adapter by
itself, or Implementation Worker summary as runtime proof. A running mock-backed frontend
may prove only an explicitly frontend-only slice when the plan names the adapter
as a proxy and defers live backend integration proof. Keep the issue
`Needs Verification` while required issue-owned runtime evidence is missing,
failed, or stale.
