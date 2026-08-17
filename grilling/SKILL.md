---
name: grilling
description: Stress-test a plan, decision, or idea through thorough, self-contained, dependency-aware questioning. Use when the user wants their thinking challenged, asks to be grilled, or when another skill needs a reusable interview discipline that separates discoverable facts, user-owned decisions, and empirical unknowns.
---

# Grilling

Investigate the whole decision tree, but deliberate with the user on one unresolved material decision at a time. Do not make the user confirm obvious or discoverable facts. Provide a recommended answer and clear reasoning for every decision.

## Classify before asking

Classify each unresolved item before putting it to the user:

- **Discoverable fact**: inspect the filesystem, code, documentation, tools, or supplied artifacts. State the conclusion and evidence instead of asking.
- **User-owned decision**: ask only when at least two plausible answers remain and the choice materially affects the outcome.
- **Empirical unknown**: identify the evidence needed. Perform safe read-only investigation when it is in scope; otherwise recommend a bounded research task or prototype and keep the dependent decision blocked rather than asking the user to guess.

Recommendations are proposals, not decisions. Do not settle even a safe or reversible default until the user accepts it. Batch low-consequence defaults only when they are genuinely independent and the user explicitly asks for a faster batch.

## Make every question self-contained

Default to thorough explanations. Make every decision understandable and answerable without requiring the user to ask what the question means. Become concise only when the user explicitly requests it; even then, preserve the decision, recommendation, and material trade-offs.

For every question, explain:

- **What the user is deciding** in plain language.
- **Why the decision is needed now**, including the dependency that made it part of the current frontier.
- **What materially changes** depending on the answer.
- **The recommended answer and its reasoning**, grounded in available evidence.
- **Each genuinely plausible alternative and its concrete trade-off**.
- **A brief example** when the decision would otherwise remain abstract.

Define unfamiliar terms inline. Put context shared by several questions in the current item or explicit batch introduction instead of repeating it. Do not repeat settled context or add detail that does not help the user choose. If a question cannot yet be explained clearly, reclassify it as a discoverable fact or empirical unknown and investigate it instead of asking an unclear question.

## Work one frontier decision at a time

Map unresolved decisions by dependency. The current frontier contains decisions whose prerequisites are already settled.

- Investigate and map the full frontier before choosing the current item. This prevents a locally reasonable answer from conflicting with a later-known dependency.
- Ask one material frontier decision, then keep discussing it until the user accepts, rejects, revises, defers, or requests specific evidence. Do not introduce the next decision merely because the first received a partial answer.
- Keep all other eligible decisions in a dependency-aware queue. On every response that presents or continues the current item, show `**Progress: Decision <position> of <total> - <remaining> remain after this**`. Calculate position as prior dispositions plus the current item; calculate total as prior dispositions plus the current and queued items. Do not show a remaining count alone. A percentage may appear only as secondary information.
- Recompute and reorder the queue after every disposition. Drop or rewrite items invalidated by the answer. If recomputation changes the total, state `**Queue revised: <old> -> <new>.** <reason>` before presenting the next item. Never silently change the denominator or stable decision IDs.
- Never ask hypothetical downstream questions for branches the user has not selected.
- Use a batch of at most 10 only when every item is independent and low consequence and the user explicitly requests batch treatment. A generic request to review, discuss, or be grilled is not a request for batching.
- Order the queue by dependency first, then consequence and uncertainty.
- Give every decision a stable reference and readable name, such as `R2.3 - Cleanup trigger`. Retain the reference if the decision is reopened.

Use this format:

```markdown
## Decision R2.1 - Canonical owner

Reply `A`, `B`, `discuss`, `defer`, or tell me what evidence you need.

**What you are deciding:** Which domain owns an order after checkout begins.

**Why this is needed now:** Retention and cleanup rules depend on one domain owning the complete lifecycle.

**What changes:** Orders ownership keeps checkout, fulfilment, and cleanup rules together. Cart ownership splits the lifecycle across two domains and requires coordination for every later transition.

**Recommend: A - Orders context.** It already owns the lifecycle invariant, so it can enforce every transition without a cross-domain handoff.

- **A. Orders context (recommended):** One owner for the complete lifecycle; requires the cart to hand off at checkout.
- **B. Cart context:** Keeps early checkout state near the cart; introduces shared ownership after checkout.

**Progress: Decision 1 of 5 - 4 remain after this**
```

Keep each item decision-shaped: one choice, its recommendation, and only genuinely plausible alternatives. The labels above are semantic requirements, not rigid wording; combine or rename them when the same understanding is clearer with less repetition. Do not turn observations into questions or pad an item or explicit batch.

## Process responses

- Give the current item an explicit disposition: accepted, rejected, revised, deferred, or blocked on named evidence.
- Treat `agree to all`, `yes to all`, and equivalent wording as acceptance of every item only when the user explicitly requested a batch. Let stable-reference answers override a batch response.
- Resolve accepted items without asking for confirmation again.
- Keep follow-up on the current item when an answer opens a new branch. Do not mix that branch with the next queued item.
- Keep unaddressed items pending when the response is partial and does not include batch acceptance.
- Recompute the frontier after every response. Drop or rewrite downstream questions invalidated by an answer.
- Briefly state the current disposition by name, then present the next eligible item. Do not repeat the full history.
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
