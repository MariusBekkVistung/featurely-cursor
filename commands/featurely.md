---
name: featurely
description: Product health snapshot — features by status, open bugs, active errors, latest changelog
---

# Featurely product health

Use the `featurely` MCP server (OAuth). Fetch only what you need, then present a concise dashboard-style summary — not raw JSON.

1. `list_features` — count by `status` (and split `type=feature` vs `type=bug` if both are present).
2. `list_bugs` — open / unarchived user-reported bugs, highlight `severity` and `occurrenceCount`.
3. `list_errors` with `status=new` — rank by severity and `occurrenceCount` × `affectedUserCount`.
4. `get_roadmap` — what's planned vs in progress.
5. `get_changelog` — what shipped recently.

Cite titles and IDs. If auth fails, tell the user to complete Featurely OAuth and grant read scopes (`features:read`, `bugs:read`, `errors:read`).
