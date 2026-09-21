# Lifecycle — always close the loop

Whenever the user asks you to **work on / fix** a Featurely item (feature, board bug, or error), keep the Featurely record in sync in **real time**. This is not optional.

## Status conventions

| Item | Starting work | Fixed / shipped |
|------|---------------|-----------------|
| Feature (`type=feature`) | `in-progress` | `done` (or `accepted`) |
| Board bug (`type=bug`) | `in-progress` | `resolved` (or `closed`) |
| Error (`error_reports`) | `investigating` | `resolved` (+ `solution`) |

Board "bug" items live in `features` with `type=bug`. User-reported widget bugs and SDK crashes live in `error_reports`. Marking a feature `done`/`accepted` or a board bug `resolved`/`closed` auto-creates an **unpublished** changelog draft — you must publish it separately.

## Step 1 — when you START (before coding)

Set the "starting work" status and post a note.

**Feature / board bug**

1. `update_feature_status` — `status: "in-progress"`, `actorName: "Cursor"`
2. `post_status_message` — `type: "update"`, message like `Investigating — reproduced locally, looking at <area>.`

**Error**

1. `update_error` — `status: "investigating"`, `actorName: "Cursor"`

## Step 2 — while working

Post short `post_status_message` updates (`type: "update"`) on meaningful progress: root cause found, fix in progress, in review.

## Step 3 — when the fix is done (after code is committed / merged)

Do **all** of these:

1. **Set the resolved status.**
   - Feature: `update_feature_status` with `status: "done"` (or `"accepted"`).
   - Board bug: `update_feature_status` with `status: "resolved"` (or `"closed"`).
   - Error: `update_error` with `status: "resolved"` and `solution` describing root cause + commit/PR.
2. **Post a final status message** (`type: "success"`) summarising the fix and linking the commit/PR.
3. **Publish the changelog** with `publish_changelog` (`published: true`) and a clear, user-facing `summary`. Skip this for errors. If changelog publish 404s, step 1 was not an announcement-worthy status — re-check.

After any write, tell the user what you synced (status → X, note posted, changelog published).

## Guardrails

- Reads are safe to run freely.
- Writes only for the specific item the user asked you to work on.
- Never bulk-mutate. Never DELETE.
- Closing the loop is part of "fixing".
- The data is live production data — be accurate and don't speculate beyond it.
