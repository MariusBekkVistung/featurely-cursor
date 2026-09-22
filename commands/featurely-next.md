---
name: featurely-next
description: Recommend what to build next for one Featurely project from votes, bugs, errors, tasks, and the roadmap
---

# What should we build next?

Use the `featurely` MCP server. Synthesise — don't dump lists. **One project only.**

1. `list_projects`. Match the name the user said. If several are granted and none was named, list `id` + `name` + scopes and stop.
2. `list_features` with `type=feature` — sort by vote count. Use `get_feature_votes` only for the shortlist if you need a live total.
3. `list_bugs` and `list_errors` with `status=new` — recurring pain.
4. `list_tasks` (skip if `tasks:read` is missing on this project) — open internal work.
5. `get_roadmap` — avoid recommending work already in progress unless it's blocked.

Recommend a short prioritised list. For each item: why (votes, severity, affected users), Featurely ID, and suggested status if they start it (`list_workflow_statuses` first). Do not change statuses until they ask to work on a specific item.
