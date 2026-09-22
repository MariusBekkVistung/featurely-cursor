# Changelog

## 1.0.3

- Align product MCP docs with the live tool list: `list_projects`, per-project scopes, `query` on `list_features`, `list_workflow_statuses`, and `list_releases`.
- Treat `list_bugs` as board bugs (same as `list_features` with `type=bug`). Analytics takes `days`; do not guess status keys.

## 1.0.2

- Resolve one Featurely project first (`list_projects`); do not fetch every granted project in one run.
- Document comments, tasks, votes, event logs, and project analytics MCP tools (including `tasks:read` / `tasks:write` / `analytics:read`).
- Add `/featurely-projects` and `/featurely-tasks`. Health, inbox, and next now pin a single project.

## 1.0.1

- Use the official Featurely app icon as the plugin logo.
- Set plugin author email to support@featurely.no.

## 1.0.0

- Cursor plugin for Featurely: product MCP (OAuth) plus SDK docs MCP.
- Skills: live workspace (`featurely`), SDK docs, CLI, and all five SDK packages.
- Commands: `/featurely`, `/featurely-inbox`, `/featurely-next`, `/featurely-ship`, `/featurely-init`.
- Agent: `featurely-triage`.
- Rule: keep Featurely records in sync; never invent IDs or statuses.
