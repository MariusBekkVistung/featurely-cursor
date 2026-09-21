---
name: featurely-inbox
description: Triage inbox — unresolved user-reported bugs and active SDK errors that need attention
---

# Featurely inbox

Use the `featurely` MCP server.

1. `list_errors` with `status=new` (add `severity=critical` first if volume is high).
2. `list_bugs` for unarchived user-reported issues.
3. `list_features` with `type=bug` and statuses that are not `resolved`/`closed`/`done`.

Present a ranked inbox: severity, occurrence/affected users, recency, title, ID. Suggest a next action per item (investigate, duplicate, ignore) without mutating anything unless the user picks an item to work on.
