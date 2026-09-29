# Convention Discovery

## Convention Discovery

Before gate review, inspect only the relevant local sources:

- Repo guidance: `AGENTS.md`, `CONTEXT.md`, `CONTEXT-MAP.md`, `DESIGN.md`, `README.md`, `PRODUCT_DECISIONS.md`.
- Planning surfaces: `ROADMAP.md`, `ROADMAP.DONE.md`, `TODO.md`, `docs/`, `_features/`, `features/`, `issues/`.
- Similar completed or active issue files.
- Existing tests near the affected code.
- Schema/type files named by the issue or implied by the affected area.
- Architecture or engineering doctrine docs, including `engineering-for-certainty`-style local guidance when present.

Prefer the repo's current issue format and testing style over this skill's fallback structure. If the repo has multiple contexts, use `CONTEXT-MAP.md` or nearby context docs to select the right context before reviewing terminology.

### Issue File Naming

For pending issue files, require the default filename format
`NN-<conventional-type>-<kebab-case-name>.md`, for example
`28-feat-student-progress-dashboard.md` or
`29-fix-session-report-score-rounding.md`.

- Use a two-digit, zero-padded, stable issue reference (`00`, `01`, ...).
  Assign the next unused number from the repository's canonical issue index or
  issue directory. Do not renumber existing issues when priority changes.
- Use the Conventional Commit type that describes the work: `feat`, `fix`,
  `refactor`, `test`, `docs`, `chore`, `perf`, `build`, or `ci`.
- Keep the remaining name concise and kebab-case. Do not repeat the type or a
  redundant implementation verb in the name.
- If the repository defines a stricter compatible convention, follow it. If it
  defines a conflicting convention, report the conflict instead of silently
  renaming the issue.
- Completed historical issues may retain their existing names. Numbered
  sub-issue packs may retain an explicit pack-local numbering scheme.
- When the final review rewrites or renames an issue, update the canonical
  roadmap/index and every in-repository reference in the same write pass.

If the issue uses a domain term that conflicts with the local glossary or product docs, stop and ask the user to resolve the term before continuing.
