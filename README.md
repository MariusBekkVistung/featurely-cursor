# Featurely for Cursor

Official Cursor plugin for [Featurely](https://www.featurely.no) — customer feedback, feature management, error tracking, and site operations.

The plugin connects Cursor to your Featurely workspace over MCP (OAuth) and ships skills, commands, and SDK docs so the agent can query live product data and install Featurely packages correctly.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **Featurely**.
3. Click **Install**, then complete **Authenticate with Featurely**. Pick the project(s) and scopes (read, or read + write lifecycle).

Or run `/add-plugin featurely` in chat.

Until the Marketplace listing is live, load the plugin from this repo:

```bash
git clone https://github.com/MariusBekkVistung/featurely-cursor.git
```

Then add it as a local plugin from **Cursor Settings → Plugins → Add from repo**, or copy the clone into `~/.cursor/plugins/local/featurely`.

## What you get

### MCP

| Server | Transport | Purpose |
|--------|-----------|---------|
| `featurely` | HTTP `https://www.featurely.no/api/mcp` | Live features, bugs, errors, roadmap, changelog. OAuth 2.1. |
| `featurely-docs` | `npx featurely-mcp@latest` | Accurate SDK docs for every Featurely package. |

Auth for the product server is OAuth. Cursor opens Featurely; you grant projects and scopes. Revoke anytime in Featurely → **Settings → Connected Apps**.

Never put API keys in this repo. Client SDK keys still live in the app's `.env.local` (`NEXT_PUBLIC_FEATURELY_API_KEY`, `NEXT_PUBLIC_FEATURELY_PROJECT_ID`) when you install packages.

### Skills

| Skill | When |
|-------|------|
| `featurely` | Live product data + mandatory start/fix/ship lifecycle |
| `featurely-sdk-docs` | Look up SDK APIs through `featurely-docs` MCP |
| `featurely-cli` | `npx featurely-cli@latest init` |
| `featurely-error-tracker` | Crash reporting SDK |
| `featurely-feature-reporter` | In-app feedback widget |
| `featurely-site-manager` | Flags, maintenance, banners, analytics |
| `featurely-i18n` | Translations |
| `featurely-logger` | Structured logs |

### Commands

| Command | What it does |
|---------|----------------|
| `/featurely` | Product health snapshot |
| `/featurely-inbox` | Active errors + unresolved bugs |
| `/featurely-next` | What to build next from votes, bugs, errors, roadmap |
| `/featurely-ship` | Mark done/resolved, post a note, publish changelog |
| `/featurely-init` | Add Featurely SDKs to this repo |

### Agent

`featurely-triage` — reads live Featurely data, recommends what to build or fix, and keeps records in sync when work starts or ships.

## Product MCP tools

Read: `list_features`, `list_bugs`, `list_errors`, `get_roadmap`, `get_changelog`

Write (needs write scopes): `update_feature_status`, `post_status_message`, `publish_changelog`, `update_error`

Lifecycle (mandatory when the user asks you to work on an item):

1. Start → `in-progress` / `investigating` + a status note
2. Progress notes as you go
3. Ship → `done` / `resolved` + success note + published changelog (features/bugs only)

Board bugs (`features` + `type=bug`) are not the same as user-reported widget bugs (`list_bugs`).

## SDK docs MCP tools

`list_packages`, `get_package_docs`, `get_installation_guide`, `get_api_reference`, `get_code_examples`

Packages: `error-tracker`, `feature-reporter`, `site-manager`, `i18n`, `logger`.

## Local development

```bash
node scripts/validate.mjs
```

Requires Node.js 18+. The validator checks `plugin.json`, `mcp.json`, logos, and frontmatter on skills, rules, agents, and commands.

## Docs

- Product: https://www.featurely.no
- API & SDKs: https://docs.featurely.no
- Publish a Cursor plugin: https://cursor.com/docs/reference/plugins

## License

MIT
