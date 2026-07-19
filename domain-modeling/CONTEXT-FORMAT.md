# CONTEXT.md Format

## Structure

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A concise description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account

## Relationships

- An **Order** produces one or more **Invoices**
- An **Invoice** belongs to exactly one **Customer**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No - an **Invoice** is only generated once a **Fulfillment** is confirmed."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** - resolved: these are distinct concepts.
```

## Rules

- **Be opinionated.** Pick the best word for a concept and list alternatives as aliases to avoid.
- **Flag conflicts explicitly.** Record ambiguities with their accepted resolution.
- **Keep definitions tight.** Use one sentence and define what a concept is, not what it does.
- **Show relationships.** Use bold canonical terms and express cardinality where it matters.
- **Include only context-specific concepts.** General programming concepts do not belong.
- **Group terms when useful.** Keep a flat list when the language is one cohesive set.
- **Write an example dialogue.** Demonstrate how related terms interact and where their boundaries lie.

## Single and multiple contexts

For a single context, keep one `CONTEXT.md` at the repository root.

For multiple contexts, keep a root `CONTEXT-MAP.md`:

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) - receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) - generates invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md) - manages warehouse picking and shipping

## Relationships

- **Ordering -> Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment -> Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering <-> Billing**: Shared types for `CustomerId` and `Money`
```

Infer the structure from existing documents. If neither exists, create a root `CONTEXT.md` lazily when the first term is accepted. In a multi-context repository, infer the relevant context from the topic and ask only when multiple plausible contexts remain.
