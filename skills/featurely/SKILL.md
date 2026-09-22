---
name: featurely
description: Query the live Featurely workspace — one project at a time — for feature requests, bugs, comments, tasks, votes, errors, event logs, analytics, releases, roadmap, and changelog. Use when the user asks about Featurely, product health, what to build next, or when starting or finishing work on a tracked item.
---

# Featurely — production data access

Read the user's **live Featurely workspace** through the Featurely MCP server
(`featurely`, OAuth). Always resolve **one project**, then fetch only what the
question needs.

Answer questions like:

- "Which Featurely projects can I see?"
- "Triager Featurely-prosjektet"
- "What are the active/critical errors right now?"
- "What are the most requested / top-voted features?"
- "What did users comment on this request?"
- "What tasks are open? What's on the roadmap?"
- "What's on the event log / analytics for this project?"
- "Given the open requests and errors, what should we build next?"

Do **not** invent Featurely IDs, vote counts, statuses, or stack traces. Query MCP first.

For tool names, arguments, and status vocabularies, read [tools.md](tools.md).
For the mandatory start/fix/ship loop, read [lifecycle.md](lifecycle.md).

## Auth

Cursor authenticates against `https://www.featurely.no/api/mcp` with OAuth.
On Featurely's **Authorize** page the user picks projects and, **per project**,
read vs read & write.

- If MCP returns unauthenticated / missing grant, tell them to complete **Authenticate with Featurely**.
- If a tool fails with a missing-scope error, name the exact permission **and** the project (`features:read`, `features:write`, `bugs:read`, `bugs:write`, `errors:read`, `errors:write`, `tasks:read`, `tasks:write`, `analytics:read`).
- Never ask the user to paste an API key into chat. Never hardcode keys, project IDs, or tokens in the repo or in generated code.
- `actorName` / comment `authorName` on writes: `"Cursor"`.

## Project first

1. Call `list_projects` (no params). It returns name + scopes per project.
2. Pin **one** `projectId` from that list (match the name they used, or the only granted project).
3. If several are granted and none was chosen, list them and stop.
4. Pass that `projectId` on every later tool. Do not run health/inbox/roadmap for every project in parallel.

## What to fetch

Fetch only what the question needs. Don't dump raw JSON — synthesise and cite titles, counts, severities, and IDs.

| Question | Tools |
|----------|--------|
| Which projects? | `list_projects` |
| Active errors | `list_errors` with `status=new`. Rank by impact. Include a representative stack-trace line from `get_error` when useful. |
| One error | `get_error` |
| Top requested features | `list_features` (`type=feature`, optional `query` / `status`). Sort by votes. |
| Vote count | `get_feature_votes` |
| Discussion thread | `list_comments` (optional `post_comment` when they asked you to reply) |
| Board bugs | `list_bugs` (same data as `list_features` with `type=bug`) |
| One board item | `get_feature` |
| Internal tasks | `list_tasks` |
| Event / log stream | `list_event_logs` |
| Usage summary | `get_project_analytics` (`days` optional) |
| Valid statuses | `list_workflow_statuses` — **before** any status write |
| Recent deploys | `list_releases` — when drafting changelog |
| Roadmap | `get_roadmap` |
| What shipped | `get_changelog` |
| What to build next | Features (by votes) + bugs + new errors + open tasks + roadmap. Recommend a shortlist. |
| Product health | Counts across features (by status), bugs, errors, plus latest changelog — **one project**. |

**Board bugs vs SDK errors:** `list_bugs` / `list_features` (`type=bug`) are kanban bugs. `list_errors` are error reports. Don't mix them.

**Comments vs status notes:** `post_comment` is the public thread. `post_status_message` is the team timeline used in the lifecycle.

## Writes

Writes are allowed **only** as part of the lifecycle in [lifecycle.md](lifecycle.md) — when the user asked you to work on or fix that specific item — or when they explicitly asked to comment, create a task, or change a task. Never bulk-mutate items you weren't asked to. Never DELETE.

Call `list_workflow_statuses` before setting a feature/bug status. A fix is not complete until Featurely status, status note, and (for features/bugs) changelog are updated.

## SDK work

Installing or configuring Featurely packages is a different skill. Use `featurely-sdk-docs` and the package skills (`featurely-error-tracker`, `featurely-feature-reporter`, `featurely-site-manager`, `featurely-i18n`, `featurely-logger`, `featurely-cli`). Prefer `featurely-docs` MCP tools over guessing APIs.
