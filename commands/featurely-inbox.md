---
name: featurely-inbox
description: Triage inbox for one Featurely project — active SDK errors and open board bugs
---

# Featurely inbox

Use the `featurely` MCP server. **One project only.**

1. `list_projects`. Match the name the user said. If several are granted and none was named, list `id` + `name` + scopes and stop.
2. `list_errors` with `status=new`.
3. `list_bugs` for open board bugs (skip terminal statuses from `list_workflow_statuses` with `type=bug`).

Present a ranked inbox: severity, occurrence/affected users, recency, title, ID. Suggest a next action per item (investigate, duplicate, ignore) without mutating anything unless the user picks an item to work on.
