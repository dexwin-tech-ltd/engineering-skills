---
name: domain-modeling
description: Build and sharpen a project's domain model by resolving terminology, testing boundaries with scenarios, and maintaining CONTEXT.md and ADRs. Use when the user wants a ubiquitous language or architectural decision recorded, when domain terms conflict with code or docs, or when another skill needs active domain-model maintenance.
---

# Domain Modeling

Actively change the domain model: challenge terms, test relationships, and record accepted language and durable decisions. Merely reading `CONTEXT.md` for vocabulary does not require this skill.

## Locate the domain documents

Most repositories have one context:

```text
/
|-- CONTEXT.md
|-- docs/adr/
`-- src/
```

If `CONTEXT-MAP.md` exists at the root, use it to locate context-specific `CONTEXT.md` and `docs/adr/` directories. System-wide ADRs remain under the root `docs/adr/`.

Create files lazily. Create `CONTEXT.md` only when the first domain term is accepted and `docs/adr/` only when the first qualifying ADR is accepted.

## Sharpen the model

- **Challenge the glossary**: surface any conflict between the user's wording and existing `CONTEXT.md` immediately.
- **Sharpen fuzzy language**: propose one precise canonical term when language is vague or overloaded.
- **Use concrete scenarios**: invent edge cases that expose unclear relationships, cardinality, ownership, or boundaries.
- **Cross-reference code**: inspect the implementation when the user describes current behavior. Surface contradictions rather than choosing silently.
- **Separate fact from decision**: inspect facts. When composed with `$grilling`, feed unresolved terminology and boundary decisions into its current frontier instead of starting a second interview loop.

## Update `CONTEXT.md` after acceptance

Use [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md).

- Update the glossary after processing each user response. When a round is accepted, capture every glossary decision resolved by that round together.
- Record only accepted terms and relationships. Do not write unresolved options or speculative downstream branches.
- If a later answer changes a term, revise the glossary and reopen decisions that depended on the old meaning.
- Keep `CONTEXT.md` completely free of implementation detail. It is a glossary, not a specification, plan, scratchpad, or decision map.
- Link to investigation artifacts when useful; do not copy their contents into the glossary.

## Offer ADRs sparingly

Offer an ADR only when all three conditions hold:

1. **Hard to reverse**: changing the decision later has meaningful cost.
2. **Surprising without context**: a future reader would reasonably ask why this path was chosen.
3. **Real trade-off**: genuine alternatives existed and were rejected for specific reasons.

If any condition is missing, skip the ADR. Use [ADR-FORMAT.md](ADR-FORMAT.md) and write it only after the decision is accepted.

Documentation maintenance performed here records the grilling outcome; it does not authorize implementation of the resulting plan.
