# Featurely MCP tools

Product data lives on the `featurely` MCP server (`https://www.featurely.no/api/mcp`).
SDK documentation lives on `featurely-docs` (`npx featurely-mcp@latest`).

## Always call first: `list_projects`

No parameters. Returns every project this connection can access, with its **name** and exactly which **scopes** are granted on that project (read, or read & write).

Use this to get real project IDs — **never guess one**. Never read IDs from `.env`, the repo, or memory.

1. Match the name the user said (e.g. “Featurely”) to a returned project.
2. If the grant has **exactly one** project, use it and say the name once.
3. If several are granted and none was named, list `id` + `name` + scopes and **stop**.
4. Pass that `projectId` on every later tool. Do not fan out the same fetch across every project.

Access is **per-project**. A grant can give different projects different levels. Every tool call is checked against that project's scopes. A project not in the grant, or a missing scope, returns a clear error — do not retry with a guessed id.

Reads: 100 / 5 min per connection. Writes: 60 / 5 min, and they count against the project owner's plan quota.

## Scopes

Checked **on that specific project**. If a tool fails, name the exact permission:

| Scope | Used by |
|-------|---------|
| `features:read` | `list_features`, `get_feature`, `get_roadmap`, `get_changelog`, `list_comments`, `get_feature_votes`, `list_workflow_statuses` (`feature`), `list_releases` |
| `features:write` | `update_feature_status`, `post_status_message`, `publish_changelog`, `post_comment` (features) |
| `bugs:read` | `list_bugs`, `list_features` (`type=bug`), `get_feature` (bugs), `list_comments` (bugs), `list_workflow_statuses` (`bug`) |
| `bugs:write` | board-bug status, comments on bugs |
| `errors:read` | `list_errors`, `get_error`, `list_event_logs` |
| `errors:write` | `update_error` |
| `tasks:read` | `list_tasks` |
| `tasks:write` | `create_task`, `update_task_status` |
| `analytics:read` | `get_project_analytics` |

`list_projects` needs a valid grant only.

## Read — `featurely`

### `list_features`

Features/bugs for **one** project.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | Required |
| `type` | `"feature"` \| `"bug"` | Optional |
| `status` | string | Optional. Use keys from `list_workflow_statuses` — do not guess |
| `query` | string | Optional free-text title search |
| `limit` | number | Optional |
| `offset` | number | Optional |

### `list_bugs`

Same as `list_features` but fixed to **board bugs**. Requires `projectId`. Optional `status`, `limit`, `offset`.

### `get_feature`

A single feature or board bug.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |

### `list_errors`

Error reports for **one** project.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | Required |
| `status` | string | Optional. Active inbox = `new` |

### `get_error`

A single error report.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `errorId` | string |

### `get_roadmap`

Feature roadmap (title, status, votes, comments). Requires `projectId`.

### `get_changelog`

Published changelog entries. Requires `projectId`.

### `list_comments`

Comment thread on a feature or board bug.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |

### `get_feature_votes`

Vote count for a feature.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |

### `list_tasks`

Internal project tasks. Scope: `tasks:read`.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `status` | string | Optional |

### `list_event_logs`

Recent event/error log stream. Scope: `errors:read`. This is the log stream, not the error inbox — use `list_errors` for triage.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `level` | string | Optional |
| `limit` | number | Optional |

### `get_project_analytics`

Aggregated usage: event count, unique sessions, top events. Scope: `analytics:read`.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | Required |
| `days` | number | Optional window |

Synthesise. Do not dump every row.

### `list_workflow_statuses`

The project's **actual** valid status keys and labels. Call this **before** `update_feature_status` or `update_task_status` — do not guess a status string.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `type` | `"feature"` \| `"bug"` |

### `list_releases`

Recent deployments — context when drafting a changelog. Optional `limit`. Requires `projectId`.

## Write — `featurely`

Need the `:write` counterpart **on that project**. Set `actorName` / `authorName` to `"Cursor"` when the tool accepts it. Writes only for the item the user named. Never bulk-mutate. Never DELETE.

### `update_feature_status`

Updates a feature or board bug's status/notes. Call `list_workflow_statuses` first and use a returned key.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `status` | string, optional |
| `developerNotes` | string, optional |
| `actorName` | string, optional |

Announcement-worthy status changes may create a changelog draft — publish it with `publish_changelog`.

### `post_status_message`

Team timeline note (not the public comment thread).

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `message` | string |
| `actorName` | string, optional |

### `publish_changelog`

Publishes a public changelog entry. Use `list_releases` for deploy context when drafting `summary`.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `summary` | string |
| `description` | string, optional |

### `update_error`

Updates an error report.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `errorId` | string |
| `status` | string, optional |
| `severity` | string, optional |
| `solution` | string, optional — required when marking resolved |
| `archived` | boolean, optional |

Errors have **no** changelog.

### `post_comment`

Public comment on a feature or board bug. Do not use this instead of `post_status_message` in the start/fix/ship loop.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `content` | string |
| `authorName` | string, optional | Use `"Cursor"` |

### `create_task`

Creates an internal task. Scope: `tasks:write`. Only when the user asked.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `title` | string |
| `description` | string, optional |
| `priority` | string, optional |

### `update_task_status`

Updates a task's status. Scope: `tasks:write`. Use a real status from the project — do not guess.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `taskId` | string |
| `status` | string |

## Docs — `featurely-docs`

| Tool | Use |
|------|-----|
| `list_packages` | Discover SDK packages |
| `get_package_docs` | Full docs for one package |
| `get_installation_guide` | Install + quick start |
| `get_api_reference` | Constructor, methods, types |
| `get_code_examples` | React / Next.js / vanilla examples |

`package` is one of: `error-tracker`, `feature-reporter`, `site-manager`, `i18n`, `logger`.
