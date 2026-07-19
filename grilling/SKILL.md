---
name: grilling
description: Stress-test a plan, decision, or idea through relentless dependency-aware questioning. Use when the user wants their thinking challenged, asks to be grilled, or when another skill needs a reusable interview discipline that separates discoverable facts, user-owned decisions, and empirical unknowns.
---

# Grilling

Resolve every material branch of the decision tree without making the user confirm obvious facts one at a time. Provide a recommended answer and brief reason for every decision.

## Classify before asking

Classify each unresolved item before putting it to the user:

- **Discoverable fact**: inspect the filesystem, code, documentation, tools, or supplied artifacts. State the conclusion and evidence instead of asking.
- **User-owned decision**: ask only when at least two plausible answers remain and the choice materially affects the outcome.
- **Empirical unknown**: identify the evidence needed. Perform safe read-only investigation when it is in scope; otherwise recommend a bounded research task or prototype and keep the dependent decision blocked rather than asking the user to guess.

Recommendations are proposals, not decisions. Do not settle even a safe or reversible default until the user accepts it. Put low-consequence defaults into a round so they can be accepted together.

## Work the decision frontier

Map unresolved decisions by dependency. The current frontier contains decisions whose prerequisites are already settled.

- Ask only frontier decisions. Never ask hypothetical downstream questions for branches the user has not selected.
- Ask numbered rounds of at most 7 questions. Aim for 3-7 when enough independent decisions are eligible.
- If the frontier is larger, split it into coherent sub-rounds and keep the rest queued. Do not imply that one sub-round exhausts the frontier.
- Use a smaller round when fewer decisions remain or one genuine fork determines which questions exist next.
- Order by dependency first, then put likely-to-be-accepted defaults first.
- Give every decision a stable reference and readable name, such as `R2.3 - Cleanup trigger`. Retain the reference if the decision is reopened.

Use this format:

```markdown
## Round 2 - Persistence boundary

Reply `agree to all`, or list exceptions such as `R2.2: B` or `R2.4: discuss`.

1. **R2.1 - Canonical owner** - Recommend: the Orders context. It already owns the lifecycle invariant.
2. **R2.2 - Retention** - Recommend: 30 days. This matches the existing cleanup policy.
   - A. 30 days (recommended)
   - B. 90 days
```

Keep each item decision-shaped: one choice, its recommendation, and only genuinely plausible alternatives. Do not turn observations into questions or pad a round.

## Process responses

- Treat `agree to all`, `yes to all`, and equivalent wording as acceptance of every item in the current round only.
- Let stable-reference answers override a batch response.
- Resolve accepted items without asking for confirmation again.
- Discuss only the named exceptions. Use one-at-a-time follow-up when an exception opens a new branch or the user asks to focus on it.
- Keep unaddressed items pending when the response is partial and does not include batch acceptance.
- Recompute the frontier after every response. Drop or rewrite downstream questions invalidated by an answer.
- Briefly state the newly fixed decisions by name, then present the next round. Do not repeat the full history.
- Periodically test accumulated decisions with a concrete scenario. Reopen only the affected decision and its dependants when a contradiction appears.

## Preserve state only when needed

For a genuinely multi-session effort, a session nearing context limits, or an explicit user request, maintain a compact decision map using [references/DECISION-MAP-FORMAT.md](references/DECISION-MAP-FORMAT.md).

Prefer an existing canonical issue or plan. Otherwise use the repository's established scratch convention, falling back to `.scratch/<topic>/decision-map.md`. Link to research or prototypes instead of copying their contents. Do not create a state artifact for an ordinary single-session grill.

## Finish deliberately

The frontier is empty only when every material item is one of:

- an evidence-backed conclusion;
- an accepted decision;
- a deliberately deferred decision with an owner or trigger;
- an explicitly out-of-scope branch.

When the frontier is empty, present a concise closeout containing accepted decisions, evidence-backed conclusions, required investigations, deferred decisions, out-of-scope branches, and the recommended next artifact or workflow. Ask the user to confirm that shared understanding has been reached; do not infer this from acceptance of the final round.

Do not implement or otherwise enact the plan during grilling. Decision-map maintenance and documentation explicitly authorized by a composing skill are allowed, but execution begins only after the user confirms shared understanding and authorizes the next step.
