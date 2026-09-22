---
name: featurely
description: Product health snapshot for one Featurely project — features by status, open bugs, active errors, latest changelog
---

# Featurely product health

Use the `featurely` MCP server (OAuth). **One project only.** Fetch only what you need, then present a concise dashboard-style summary — not raw JSON.

1. `list_projects`. Match the name the user said. If several are granted and none was named, list `id` + `name` + scopes and stop.
2. `list_features` for that `projectId` — count by `status` (split `type=feature` vs `type=bug` if both are present).
3. `list_bugs` — open board bugs (use `list_workflow_statuses` with `type=bug` if you need the non-terminal keys).
4. `list_errors` with `status=new`.
5. `get_roadmap` — planned vs in progress.
6. `get_changelog` — what shipped recently.

Do **not** call comments, tasks, analytics, event logs, or releases unless the user asked. Cite titles and IDs.

If auth fails, tell the user to complete Featurely OAuth. Scopes are per project (`features:read`, `bugs:read`, `errors:read`).
