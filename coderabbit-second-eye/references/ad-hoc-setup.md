# Ad-hoc CodeRabbit setup (engineer-owned)

This family does **not** install CodeRabbit. When the second eye cannot run,
tell the developer once using this shape (adapt only the missing piece):

```text
CodeRabbit Second Eye was skipped: <CLI not found | not signed in | …>.

Why it matters: on Medium+ reviews it catches defects the agent finders may
miss, then feeds candidates into $code-review verify (it does not replace
$code-review).

Ad-hoc setup (you own this, outside eng-for-certainty):
1. Install and sign in to the CodeRabbit CLI — see CodeRabbit CLI docs.
   $coderabbit-second-eye uses the CLI; that is enough for the second eye.
2. Optional: install only non-colliding CodeRabbit skills, e.g.
   `npx skills add coderabbitai/skills --skill coderabbit-config`
   Do NOT install CodeRabbit's skill named `code-review` — it collides with
   this family's $code-review (same skill name / directory). Prefer the
   CodeRabbit marketplace plugin (namespaced, e.g. /coderabbit:code-review)
   if you want their interactive review skill.
3. Re-run the review (or $coderabbit-second-eye) after the CLI is usable.

This integration skill (if needed):
npx skills add seyofori/eng-for-certainty --skill coderabbit-second-eye
```

Rules:

- Say this when a qualified `$code-review` run skips the second eye, or when
  standalone `$coderabbit-second-eye` cannot start.
- Do not run installers or paste install-script curls unless the engineer
  explicitly asks you to help install.
- Do not recommend installing CodeRabbit's bare `code-review` skill alongside
  this family's `$code-review`.
- On Low effort, stay quiet about missing CodeRabbit unless they asked for
  the second eye.
