---
name: featurely-tasks
description: List internal Featurely tasks for one project
---

# Featurely tasks

Use the `featurely` MCP server. **One project only.** Requires `tasks:read` on that project.

1. `list_projects`. Match the name the user said. If several are granted and none was named, list `id` + `name` + scopes and stop.
2. `list_tasks` for that `projectId` (optional `status`).
3. Group by `status`.

Do not create or update tasks unless the user asked. Status writes need `tasks:write` and a real status key — do not guess. If the tool fails with a missing-scope error, name `tasks:read` / `tasks:write` and stop.
