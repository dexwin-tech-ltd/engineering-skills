# CodeRabbit `code-review` skill conflict check

Run once per `$coderabbit-second-eye` invoke (including when soft-loaded from
`$code-review`). Non-blocking: always continue the second-eye or skip path.

## Where to look

Check for a skill directory named `code-review` (with `SKILL.md`) under:

- `~/.agents/skills/code-review`
- `~/.claude/skills/code-review`
- `~/.cursor/skills/code-review`
- Project `.agents/skills/code-review`, `.claude/skills/code-review`,
  `.cursor/skills/code-review` when inside a git work tree

Stop at the first CodeRabbit match; one hint is enough.

## How to tell CodeRabbit's skill from ours

Read `SKILL.md` frontmatter and the first screen of the body.

**Ours (eng-for-certainty)** — keep silent:

- Describes Independent Reviewer, Candidate Standard, or
  `$engineering-for-certainty` / doctrine-backed review
- Does **not** center on `coderabbit review` / CodeRabbit CLI auth as the
  review engine

**CodeRabbit's** — treat as a conflict:

- Instructs running `coderabbit review`, `coderabbit auth`, or CodeRabbit CLI
  agent mode as the primary review workflow
- Frontmatter/description is about CodeRabbit CLI reviews, fix prompts, or
  CodeRabbit runbooks without Independent Reviewer / Candidate Standard

If unclear, do not hint.

## Hint template (once)

```text
Conflict: a CodeRabbit skill named code-review is installed at <path>.
It can shadow this family's $code-review.

What to do (engineer-owned):
- Remove or rename that skill directory, or
- Use the CodeRabbit marketplace plugin (namespaced, e.g. /coderabbit:code-review)
  instead of the bare skill.

$code-review and $coderabbit-second-eye continue. Second eye uses the
CodeRabbit CLI, not that skill.
```

## Rules

- Do not uninstall, rename, move, or edit the conflicting skill.
- Do not fail the review or second eye solely because of the conflict.
- Do not repeat the hint later in the same invoke.
- Namespaced plugin installs that are not a bare `code-review` skill directory
  are not a conflict.
