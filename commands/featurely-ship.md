---
name: featurely-ship
description: Close the loop on a Featurely item — mark done/resolved, post a success note, publish changelog
---

# Ship a Featurely item

Requires write scopes. Follow the `featurely` skill lifecycle. Ask which `featureId` / `errorId` if it isn't in the conversation.

**Feature**
1. `update_feature_status` → `done` (or `accepted`), `actorName: "Cursor"`
2. `post_status_message` → `type: "success"` with commit/PR + one-line summary
3. `publish_changelog` → `published: true` with a user-facing `summary`

**Board bug**
Same as feature, but status `resolved` (or `closed`).

**Error**
`update_error` → `status: "resolved"` plus `solution`. No changelog.

Then tell the user exactly what was synced. If changelog publish 404s, the status wasn't announcement-worthy — fix status first.
