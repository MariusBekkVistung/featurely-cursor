---
name: featurely-cli
description: Install and configure Featurely SDKs with the Featurely CLI. Use when the user wants to add Featurely to a project, run featurely-cli init, log in, scan dependencies, or set up MCP for an editor.
---

# featurely-cli

The CLI is the default way to add Featurely to an app. Prefer it over hand-wiring packages unless the user asks for a manual install.

```bash
npx featurely-cli@latest init
```

Requires Node.js 18+. Docs: https://docs.featurely.no/docs/cli

## Commands

| Command | Auth | What it does |
|---------|------|----------------|
| `init` | Session | Guided setup: sign-in, pick packages, create API key, inject initializer, write `.env.local`, optional Vercel + MCP |
| `login` | — | Browser sign-in. `--force` to re-auth |
| `logout` | — | Drop the local session |
| `whoami` | Session | Account, user ID, token expiry |
| `status` | — | Auth, `.env.local`, installed packages, CLI version |
| `scan` | API key | Upload the project's dependency list. Reads `NEXT_PUBLIC_FEATURELY_API_KEY` or `--api-key` |
| `mcp` / `mcp setup` | — | Register `featurely-mcp` for Claude Desktop, Cursor, VS Code, or Windsurf |

This Cursor plugin already ships both MCP servers. `mcp setup` is for other editors or for projects that are not using the plugin.

## What `init` does

1. Browser sign-in
2. Package picker — `featurely-mcp` is registered as an MCP server, not installed as a local dependency
3. API key with the right permissions
4. Framework detection and `featurely-providers.tsx` (logger is manual)
5. Writes `NEXT_PUBLIC_FEATURELY_API_KEY` and `NEXT_PUBLIC_FEATURELY_PROJECT_ID` to `.env.local`
6. Optional Vercel env sync when `.vercel/` exists
7. Optional MCP config for detected editors

## Frameworks

Next.js App Router (`app/`), Next.js Pages Router (`pages/`), React, Vue, Nuxt, Node.js fallback. Package manager from lockfile: npm, yarn, pnpm, bun.

## After init

- Confirm `.env.local` is gitignored
- Use package skills / `featurely-docs` MCP for code that `init` did not generate (`featurely-logger` especially)
- Don't duplicate the widget or tracker if `init` already injected them
