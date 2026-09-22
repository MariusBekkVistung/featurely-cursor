---
name: featurely-ship
description: Close the loop on a Featurely item — mark done/resolved, post a success note, publish changelog
---

# Ship a Featurely item

Requires write scopes **on that project**. Follow the `featurely` skill lifecycle. Resolve `projectId` via `list_projects` (or the project already pinned this chat). Ask which `featureId` / `errorId` / `taskId` if it isn't in the conversation.

Call `list_workflow_statuses` before setting a feature/bug status. Glance at `list_releases` when drafting changelog copy.

**Feature**
1. `update_feature_status` → shipped/done key from the workflow list, `actorName: "Cursor"`
2. `post_status_message` with commit/PR + one-line summary
3. `publish_changelog` with a user-facing `summary`

**Board bug**
Same as feature, but the resolved/closed key from `list_workflow_statuses` (`type=bug`).

**Error**
`update_error` → `status: "resolved"` plus `solution`. No changelog.

Then tell the user exactly what was synced.
