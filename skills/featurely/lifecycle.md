# Lifecycle — always close the loop

Whenever the user asks you to **work on / fix** a Featurely item (feature, board bug, or error), keep the Featurely record in sync in **real time**. This is not optional. Resolve `projectId` with `list_projects` (or the project already pinned this chat) before any write.

Public `post_comment` is not a substitute for `post_status_message` in this loop. Internal `create_task` / `update_task_status` only when they asked for a task.

## Status conventions

Do **not** guess status strings. Call `list_workflow_statuses` with `type=feature` or `type=bug` and use a returned key. Typical defaults (only if they appear in that list):

| Item | Starting work | Fixed / shipped |
|------|---------------|-----------------|
| Feature (`type=feature`) | `in-progress` | `done` (or `accepted`) |
| Board bug (`type=bug`) | `in-progress` | `resolved` (or `closed`) |
| Error (`list_errors`) | `investigating` | `resolved` (+ `solution`) |

Board bugs live in `list_bugs` / `list_features` with `type=bug`. SDK crashes live in `list_errors`. Marking a feature or board bug to an announcement-worthy status may create a changelog draft — publish it with `publish_changelog`. Use `list_releases` when drafting the summary so it matches a real deploy.

## Step 1 — when you START (before coding)

Set the "starting work" status and post a note.

**Feature / board bug**

1. `list_workflow_statuses` for the item type
2. `update_feature_status` — starting-work key from that list, `actorName: "Cursor"`
3. `post_status_message` — message like `Investigating — reproduced locally, looking at <area>.`

**Error**

1. `update_error` — `status: "investigating"`

## Step 2 — while working

Post short `post_status_message` updates on meaningful progress: root cause found, fix in progress, in review.

## Step 3 — when the fix is done (after code is committed / merged)

Do **all** of these:

1. **Set the resolved status** (keys from `list_workflow_statuses`).
   - Feature: shipped/done key.
   - Board bug: resolved/closed key.
   - Error: `update_error` with `status: "resolved"` and `solution` describing root cause + commit/PR.
2. **Post a final status message** summarising the fix and linking the commit/PR.
3. **Publish the changelog** with `publish_changelog` and a clear, user-facing `summary`. Skip this for errors. Glance at `list_releases` so the copy matches a real deployment.

After any write, tell the user what you synced (status → X, note posted, changelog published).

## Guardrails

- Reads are safe to run freely (100 / 5 min per connection).
- Writes only for the specific item the user asked you to work on (60 / 5 min, plus plan quota).
- Never bulk-mutate. Never DELETE.
- Closing the loop is part of "fixing".
- The data is live production data — be accurate and don't speculate beyond it.
