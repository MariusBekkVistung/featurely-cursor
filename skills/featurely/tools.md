# Featurely MCP tools

Product data lives on the `featurely` MCP server (`https://www.featurely.no/api/mcp`).
SDK documentation lives on `featurely-docs` (`npx featurely-mcp@latest`).

Pass `projectId` when the grant covers more than one project and the user has not already pinned one. If only one project is granted, the server may infer it — still send `projectId` when you have it.

## Read — `featurely`

### `list_features`
List board items.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | Required when ambiguous |
| `type` | `"feature"` \| `"bug"` | Omit for both |
| `status` | string | `open`, `planned`, `in-progress`, `in-review`, `accepted`, `done`, `declined` (bugs also use `resolved`, `closed`) |
| `limit` | number | Max 100 |
| `offset` | number | Pagination |

Returns `{ features, count, limit, offset }`. Fields include `id`, `title`, `description`, `status`, `type`, `voteCount`, `commentCount`, `createdAt`.

### `list_bugs`
User-reported bugs (SDK / feedback widget). Stored as `error_reports` with `userReported=true`.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | |
| `status` | string | Error statuses below |
| `severity` | `"low"` \| `"medium"` \| `"high"` \| `"critical"` | |
| `includeArchived` | boolean | Default false |
| `limit` / `offset` | number | Max 100 |

Returns `{ bugs }` with `title`, `message`, `severity`, `status`, `occurrenceCount`, `createdAt`.

### `list_errors`
SDK-captured error reports.

| Argument | Type | Notes |
|----------|------|--------|
| `projectId` | string | |
| `status` | `"new"` \| `"triaged"` \| `"investigating"` \| `"resolved"` \| `"closed"` \| `"ignored"` | Active = `new` |
| `severity` | `"low"` \| `"medium"` \| `"high"` \| `"critical"` | |
| `userReported` | boolean | |
| `includeArchived` | boolean | Default false |
| `limit` / `offset` | number | Max 100 |

Returns `{ errors }` with `title`, `message`, `errorType`, `stackTrace`, `severity`, `status`, `occurrenceCount`, `affectedUserCount`, `lastOccurrenceAt`, `solution`.

### `get_roadmap`
Items grouped by status column. Optional `status` (comma-separated) and `limit` (max 200).

### `get_changelog`
Published changelog entries, newest first. Optional `published`, `limit`, `offset`.

## Write — `featurely`

Required scopes: `features:write` / `bugs:write` / `errors:write`. Always set `actorName` to `"Cursor"`.

### `update_feature_status`
`PATCH` equivalent for a feature or board bug.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `status` | string |
| `developerNotes` | string, optional, max 2000 |
| `priority` | `"critical"` \| `"high"` \| `"medium"` \| `"low"` \| null |
| `actorName` | string |

Announcement-worthy statuses (`accepted`/`done` for features, `resolved`/`closed` for board bugs) auto-create an **unpublished** changelog draft, write activity, email followers, and fire webhooks.

### `post_status_message`
Public timeline note on a feature/bug.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `message` | string, 1–5000 |
| `type` | `"info"` \| `"warning"` \| `"success"` \| `"update"` (default `update`) |
| `actorName` | string |

### `publish_changelog`
Publish or edit the changelog draft (`id == featureId`). Draft exists only after an announcement-worthy status change.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `featureId` | string |
| `published` | boolean |
| `summary` | string, user-facing, max 500 |
| `description` | string, optional, max 2000 |

### `update_error`
Resolve or retarget an error report.

| Argument | Type |
|----------|------|
| `projectId` | string |
| `errorId` | string |
| `status` | `"new"` \| `"triaged"` \| `"investigating"` \| `"resolved"` \| `"closed"` \| `"ignored"` |
| `solution` | string, optional, max 5000 — required when marking resolved |
| `severity` | optional |
| `archived` | boolean, optional |
| `actorName` | string |

Resolving stamps `resolvedAt` / `resolvedBy` and fires `error.resolved`. Errors have **no** changelog.

## Docs — `featurely-docs`

| Tool | Use |
|------|-----|
| `list_packages` | Discover SDK packages |
| `get_package_docs` | Full docs for one package |
| `get_installation_guide` | Install + quick start |
| `get_api_reference` | Constructor, methods, types |
| `get_code_examples` | React / Next.js / vanilla examples |

`package` is one of: `error-tracker`, `feature-reporter`, `site-manager`, `i18n`, `logger`.
