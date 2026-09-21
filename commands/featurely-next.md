---
name: featurely-next
description: Recommend what to build next from live votes, bugs, errors, and the Featurely roadmap
---

# What should we build next?

Use the `featurely` MCP server. Synthesise — don't dump lists.

1. `list_features` with `type=feature` (limit 100) — sort by `voteCount`.
2. `list_bugs` and `list_errors` with `status=new` — recurring pain.
3. `get_roadmap` — avoid recommending work already in progress unless it's blocked.

Recommend a short prioritised list. For each item: why (votes, severity, affected users), Featurely ID, and suggested status if they start it. Do not change statuses until they ask to work on a specific item.
