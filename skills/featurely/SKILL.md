---
name: featurely
description: Query the live Featurely workspace — feature requests, bugs, user-reported issues, errors/crashes, roadmap and changelog — to discuss what to build, triage active errors, and report on product health. Use when the user asks about Featurely, feature requests, active errors, bugs, roadmap, changelog, product health, or "what should we build next". Also use when starting or finishing work on a tracked Featurely item so status, notes, and changelog stay in sync.
---

# Featurely — production data access

Read the user's **live Featurely project** through the Featurely MCP server
(`featurely`, OAuth). Answer questions like:

- "What are the active/critical errors right now?"
- "What are the most requested / top-voted features?"
- "What bugs have users reported this week?"
- "What's on the roadmap? What shipped recently (changelog)?"
- "Given the open requests and errors, what should we build next?"

Do **not** invent Featurely IDs, vote counts, statuses, or stack traces. Query MCP first.

For tool names, arguments, and status vocabularies, read [tools.md](tools.md).
For the mandatory start/fix/ship loop, read [lifecycle.md](lifecycle.md).

## Auth

Cursor authenticates against `https://www.featurely.no/api/mcp` with OAuth.
The user picks projects and scopes in the Featurely consent screen.

- If MCP returns unauthenticated / missing grant, tell them to complete **Authenticate with Featurely** and grant the needed project + scopes.
- If a tool fails with a missing-scope error, name the exact permission (`features:read`, `features:write`, `bugs:read`, `bugs:write`, `errors:read`, `errors:write`).
- Never ask the user to paste an API key into chat. Never hardcode keys, project IDs, or tokens in the repo or in generated code.
- `actorName` on writes: `"Cursor"`.

## What to fetch

Fetch only what the question needs. Don't dump raw JSON — synthesise and cite titles, counts, severities, and IDs.

| Question | Tools |
|----------|--------|
| Active errors | `list_errors` with `status=new` (optional `severity=critical`). Rank by `occurrenceCount` × `affectedUserCount`. Include a representative stack-trace line and a likely cause. |
| Top requested features | `list_features` with `type=feature`, then sort by `voteCount`. Add `status=open` (or `planned` / `in-progress`) when they want a subset. |
| User-reported bugs | `list_bugs` (widget/SDK submissions). Optional `status`, `severity`. |
| Board bugs | `list_features` with `type=bug`. These are **not** the same as `list_bugs`. |
| Roadmap | `get_roadmap` |
| What shipped | `get_changelog` |
| What to build next | Features (by votes) + bugs + new errors + roadmap. Recommend a shortlist. |
| Product health | Counts across features (by status), bugs, errors (new vs resolved), plus latest changelog. |

**Board bugs vs user-reported bugs:** `features` with `type=bug` are kanban items. `list_bugs` / `error_reports` with `userReported=true` are widget submissions. Don't mix them.

## Writes

Writes are allowed **only** as part of the lifecycle in [lifecycle.md](lifecycle.md) — when the user asked you to work on or fix that specific item. Never bulk-mutate items you weren't asked to. Never DELETE.

A fix is not complete until Featurely status, status note, and (for features/bugs) changelog are updated.

## SDK work

Installing or configuring Featurely packages is a different skill. Use `featurely-sdk-docs` and the package skills (`featurely-error-tracker`, `featurely-feature-reporter`, `featurely-site-manager`, `featurely-i18n`, `featurely-logger`, `featurely-cli`). Prefer `featurely-docs` MCP tools over guessing APIs.
