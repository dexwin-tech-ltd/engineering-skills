# Project Conventions

Use when starting a project, changing its tooling, planning formal issue branches or pull requests, or changing a public release contract. Preserve established repository conventions.

## Preferred Defaults

- Language: TypeScript.
- Package manager: pnpm.
- Web frontend: React + Vite + TanStack Router + Tailwind.
- Mobile: Expo React Native.
- Form UI library (web/mobile): TanStack Form.
- Server state and query orchestration (web/mobile): TanStack Query.
- Backend: Fastify.
- Database queries: Drizzle.
- Validation: Zod.
- Result library: neverthrow.
- Path aliases: for new TypeScript/JavaScript projects, use named `#...`
  aliases for private imports within an app or package, and reserve
  `@scope/package` specifiers for workspace or published packages. Prefer
  `package.json#imports` where supported, with matching TypeScript, bundler, and
  test configuration as required. Preserve established repository conventions
  unless the user requests a migration.

## Linting and Formatting

- For new projects or TypeScript/JavaScript projects that do not already standardize on another tool, prefer ESLint for linting and Prettier for formatting.
- Preserve the repo's existing toolchain when one already exists; this rule overrides the ESLint/Prettier default for established repos. Do not migrate a repo from Biome, dprint, Rome, Standard, or another established formatter/linter unless the user explicitly asks.
- Use ESLint for correctness, maintainability, and bug-prevention rules. Use Prettier for layout and whitespace only. Do not duplicate formatting rules in ESLint.
- Prefer ESLint flat config for new projects.
- For TypeScript projects, prefer `typescript-eslint` for parsing and TypeScript-aware rules.
- In monorepos, ESLint and Prettier should be wired to work consistently across apps and packages. Prefer shared root config or shared config packages over drifting per-package defaults.
- Prefer repo scripts that make the distinction explicit: `lint`, `lint:fix`, `format`, and `format:check`.

### Recommended ESLint Defaults

- `@typescript-eslint/no-unused-vars` with `_`-prefixed unused parameters/variables ignored when intentionally unused.
- `@typescript-eslint/no-floating-promises`.
- `@typescript-eslint/no-misused-promises`.
- `@typescript-eslint/consistent-type-imports`.
- `@typescript-eslint/switch-exhaustiveness-check`.
- `eqeqeq`.
- `curly`.
- `prefer-const`.

### Recommended Prettier Defaults

- Keep Prettier opinionated and minimal; avoid style bikeshedding.
- For new projects without an existing convention, default to:
  - `printWidth: 100`
  - `singleQuote: true`
  - `trailingComma: "all"`
  - `semi: true`
  - `arrowParens: "always"`
- If a repo already has an established formatting style, keep that style instead of reformatting unrelated code.

### Recommended Git Hook Defaults

- Prefer local git hooks or equivalent local automation for fast feedback when the repo supports them.
- Pre-commit should run formatting and linting against staged files, plus the unit-test command when the repo's unit suite is reliably fast enough for commit-time feedback.
- Pre-push should run the broader test gates: full unit tests, integration tests, and E2E tests when those commands already exist in the repo.
- Prefer explicit repo scripts for hook entry points, such as `test:unit`, `test:integration`, and `test:e2e`.
- Do not silently skip missing commands. Either wire hooks only to commands that exist or fail with a clear message so the repo's guarantees stay trustworthy.

## Version Control and Releases

Use explicit version-control semantics so project history communicates intent and release impact.

- Preserve the repo's established branch, commit, and release conventions when they exist.
- For new repos or repos without a stated convention, prefer Conventional Commits, conventional branch names, and SemVer.
- Treat commit messages, branch names, and version bumps as part of the engineering contract. They should help future maintainers understand what changed, why it changed, and whether consumers must react.

### Pull Request Scope

- Prefer one pull request per Smallest Coherent Slice: one coherent observable
  outcome that is independently implementable, testable, and reviewable.
- Do not put an entire feature into one pull request when it contains multiple
  coherent slices. Treat the feature as a parent issue pack and give each slice
  its own issue, branch, acceptance criteria, evidence, and pull request.
- Judge coherence by behavior, ownership, dependencies, and proof, not by a
  line-count limit. Do not split one meaningful behavior into technical
  micro-pull-requests that cannot be reviewed or left green independently.
- Base independent slices directly on the canonical branch. Use stacked pull
  requests only when a later slice genuinely depends on an earlier slice;
  record the base, head, preceding pull request, merge order, and retarget or
  rebase procedure.
- Keep every slice green and reviewable against its declared base. After all
  slices are integrated, validate and review the combined feature state so
  cross-slice defects are not hidden by individually green pull requests.

### Conventional Commits

- Use Conventional Commits for human-authored commits by default:
  `<type>(<scope>): <description>`.
- Prefer these commit types unless the repo defines another set:
  `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `perf`, `style`, `chore`, and `revert`.
- Use a scope when it clarifies ownership, such as `feat(auth): ...`, `fix(api): ...`, or `docs(issue-review): ...`.
- Keep the description imperative and concrete. Prefer "validate invite token expiry" over "updates auth stuff".
- Mark breaking changes explicitly with `!` after the type or scope and explain the impact in the body, for example `feat(api)!: require actor context`.
- Use commit bodies for rationale, migration notes, validation performed, and issue references when the subject alone is not enough.
- Do not hide behavior changes inside `chore`, `refactor`, or `style` commits. If runtime behavior changes, use the type that describes the user or system impact.

### Conventional Branching

- Use conventional branch names for new branches when the repo does not specify another pattern:
  `<type>/<short-kebab-description>`.
- Align branch type with the dominant intent: `feat/`, `fix/`, `docs/`, `refactor/`, `test/`, `build/`, `ci/`, `perf/`, `chore/`, or `release/`.
- Keep branch names short, lowercase, and kebab-case. Include a ticket or issue key only when the repo uses one, such as `fix/pm-123-token-expiry`.
- When a change mixes unrelated intents, split the work instead of using a vague branch such as `misc` or `updates`.
- When the active platform requires a branch prefix, preserve it and apply the
  conventional branch name after it.

### Worktree Isolation

- Default each planned issue implementation to its own dedicated linked git
  worktree, including single-threaded work. Preserve a different repository
  convention or the user's explicit direction to use the shared checkout.
- When issue review will flow directly into implementation, determine that
  intent from the initial request only when it is explicit; otherwise ask. After
  the exact Branch Contract is resolved and shared understanding is confirmed,
  finalize and commit the approved issue in its implementation worktree before
  production-code changes begin. Delivery reuses that worktree and branch.
- For backlog-only issue work, do not create an idle implementation worktree.
  Later implementation starts from a verified base containing the exact
  Approved Issue Commit and keeps later issue and code changes together.
- Create the worktree from the Branch Contract's declared pull-request base:
  the verified canonical branch for an independent slice, or the preceding
  pull-request branch for a stacked slice. Do not replace a stacked base with
  the default branch.
- Verify the base ref and resolved SHA before implementation. Do not call a
  remote-tracking ref such as `origin/main` current unless it was fetched or
  otherwise verified when that network action is necessary and authorized.
- Record the portable base ref and worktree isolation mode in the canonical
  issue. Record the machine-specific worktree path, resolved base SHA, and
  Approved Issue Commit SHA in the pre-work execution handoff; never store an
  absolute local path in the canonical issue.

### Semantic Versioning

- For packages, public APIs, plugins, CLIs, schemas, SDKs, and shared contracts, follow SemVer unless the repo has a documented alternative.
- Increment `MAJOR` for breaking changes, including removed APIs, incompatible schema changes, changed error contracts, changed CLI flags, or behavior that existing consumers reasonably depend on.
- Increment `MINOR` for backward-compatible features, new optional fields, new endpoints, new commands, or new capabilities.
- Increment `PATCH` for backward-compatible fixes, documentation corrections shipped with a package, internal refactors with no consumer-visible behavior change, and compatible dependency or build fixes.
- Treat database migrations, generated contracts, and exported TypeScript types as release-impacting surfaces when downstream consumers depend on them.
- Record migration guidance for every breaking change. The guidance should state who is affected, what they must change, and how to verify the migration.
- Do not bump versions mechanically. Choose the bump from the highest-impact change in the release.
- In monorepos, follow the repo's versioning model. If packages are independently versioned, bump only affected packages and any dependents whose published contract changes.
