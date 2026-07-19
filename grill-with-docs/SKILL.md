---
name: grill-with-docs
description: Run an explicit docs-backed grilling session that challenges a plan against the existing domain model, sharpens terminology, and records accepted glossary and architectural decisions. Use when the user invokes grill-with-docs or asks to stress-test a plan while maintaining CONTEXT.md and ADRs.
---

# Grill With Docs

Load and follow `$grilling` and `$domain-modeling` before asking any questions.

- Use `$grilling` as the sole interview protocol and decision frontier.
- Feed unresolved terminology, ownership, relationship, and boundary decisions from `$domain-modeling` into that frontier. Do not start a second interview loop.
- Update domain documents only after the corresponding decisions are accepted.
- Include documentation changed in the final grilling closeout.
- Do not implement the resulting plan until the user confirms shared understanding and authorizes execution.

If either dependency is unavailable, stop and tell the user to install `grill-with-docs` together with `grilling` and `domain-modeling`; do not reconstruct partial behavior from memory.
