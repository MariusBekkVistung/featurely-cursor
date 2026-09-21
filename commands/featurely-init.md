---
name: featurely-init
description: Add Featurely SDKs to this repo with the Featurely CLI
---

# Initialize Featurely in this project

Prefer the CLI over hand-wiring packages.

```bash
npx featurely-cli@latest init
```

If they only want MCP in other editors:

```bash
npx featurely-cli@latest mcp setup
```

After init:

1. Confirm `.env.local` has `NEXT_PUBLIC_FEATURELY_API_KEY` and `NEXT_PUBLIC_FEATURELY_PROJECT_ID` and is gitignored.
2. Open the generated provider/initializer and wire it into the app layout if the CLI didn't.
3. `featurely-logger` is never auto-injected — set it up with the `featurely-logger` skill if they asked for logs.
4. Use `featurely-docs` MCP for API details instead of guessing.

Node.js 18+ required. If `init` fails on auth, run `npx featurely-cli@latest login --force`.
