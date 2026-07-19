# Decision Map Format

Use this compact state only when a grilling effort must span sessions or context windows.

```md
# {Destination}

{One or two sentences describing what will be true when the decision tree is resolved.}

## Decisions so far

- **{Decision name}**: {Accepted answer and short reason}

## Current frontier

- **{Decision name}**: {What must be decided and why it is now eligible}

## Investigations

- **{Investigation name}**: {Evidence needed, owner, and linked artifact when available}

## Deferred

- **{Decision name}**: {Owner or trigger that will reopen it}

## Out of scope

- {Branch deliberately excluded from this effort}
```

Keep the map at low resolution. A decision or investigation has one detailed home; the map names it, records its status, and links to that detail without duplicating it.
