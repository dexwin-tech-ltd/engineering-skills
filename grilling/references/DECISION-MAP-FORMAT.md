# Decision Map Format

Use this compact state only when a grilling effort must span sessions or context windows.

```md
# {Destination}

{One or two sentences describing what will be true when the decision tree is resolved.}

## Decisions so far

- **{Decision name}**: {Accepted answer and short reason}

## Queue progress

- **Position**: Decision {current position} of {current total}; {remaining count} remain after the current item
- **Last total change**: {previous total} -> {current total} because {reason}; omit when the total has not changed

## Current focus

- **{Decision name}**: {The one decision being discussed, why it is now eligible, and its current state}

## Queued frontier

- **{Decision name}**: {Why it is eligible and any dependency that may reorder or remove it}

## Investigations

- **{Investigation name}**: {Evidence needed, owner, and linked artifact when available}

## Deferred

- **{Decision name}**: {Owner or trigger that will reopen it}

## Out of scope

- {Branch deliberately excluded from this effort}
```

Keep the map at low resolution. A decision or investigation has one detailed home; the map names it, records its status, and links to that detail without duplicating it.
