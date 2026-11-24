# Thin CodeRabbit Second Eye integration

CodeRabbit already ships installable CLI skills (`coderabbitai/skills`), including
a skill also named `code-review`. Duplicating that stack inside this family would
fight Independent Reviewer ownership and create a painful vendor swap later.
We ship `$coderabbit-second-eye` as a disposable thin integration: `$code-review`
soft-loads it after its own finders on Medium or higher effort, maps vendor
output to Candidate Standard fields, and verifies every item before the Review
Queue. Engineers install and authenticate the CodeRabbit CLI (and optional
CodeRabbit skills) themselves; this skill only detects what is already usable.
Repository configuration is ensured through tooling already on the machine
after analysis and approval. Uninstalling the skill leaves `$code-review`
intact; a future vendor needs a new thin skill plus a one-line hook rename.
