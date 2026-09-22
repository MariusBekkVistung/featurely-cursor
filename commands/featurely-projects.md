---
name: featurely-projects
description: List Featurely projects granted to this Cursor session, with per-project scopes
---

# Featurely projects

Use the `featurely` MCP server.

1. Call `list_projects` (no other tools).
2. List each granted project: name, id, and whether that project is read or read & write (the scopes it returned).
3. If there is exactly one project, say it is the default for later commands.
4. If there are several, ask which one to use (name is enough). Do not fetch features, bugs, errors, roadmap, or changelog in this command.

If auth fails, tell the user to complete **Authenticate with Featurely**.
