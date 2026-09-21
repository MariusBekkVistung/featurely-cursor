---
name: featurely-sdk-docs
description: Look up Featurely SDK documentation via the featurely-docs MCP server. Use when installing, configuring, or debugging featurely-error-tracker, featurely-feature-reporter, featurely-site-manager, featurely-i18n, featurely-logger, or featurely-mcp, or when the user asks how a Featurely package works.
---

# Featurely SDK docs

Prefer `featurely-docs` MCP tools over guessing APIs or fetching docs.featurely.no.

## Tools

Call in this order:

1. `list_packages` — if you don't know which package
2. `get_installation_guide` — setup
3. `get_api_reference` — constructor, methods, types
4. `get_code_examples` — React / Next.js / vanilla
5. `get_package_docs` — full corpus when you need everything

`package` must be one of: `error-tracker`, `feature-reporter`, `site-manager`, `i18n`, `logger`.

## Package map

| Package | npm | Permission | Skill |
|---------|-----|------------|--------|
| Error tracking | `featurely-error-tracker` | `errors:write` | `featurely-error-tracker` |
| Feedback widget | `featurely-feature-reporter` | `features:write` | `featurely-feature-reporter` |
| Flags / maintenance / analytics | `featurely-site-manager` | `public:read` | `featurely-site-manager` |
| Translations | `featurely-i18n` | `public:read` | `featurely-i18n` |
| Structured logs | `featurely-logger` | `logs:write` | `featurely-logger` |

For first-time install across a repo, use `featurely-cli` (`npx featurely-cli@latest init`).

## Rules

- Client keys go in `NEXT_PUBLIC_FEATURELY_API_KEY` / `NEXT_PUBLIC_FEATURELY_PROJECT_ID`. Never commit secrets.
- Match key permissions to the package. Missing permission is `403`.
- Don't uninstall `featurely-*` packages unless the user asks.
- After installing, keep using MCP docs rather than training-data APIs — the packages change.
