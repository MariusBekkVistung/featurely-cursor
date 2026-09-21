---
name: featurely-error-tracker
description: Install and configure featurely-error-tracker — automatic error tracking with breadcrumbs, session replay, device context, toasts, and PII scrubbing. Use when adding error tracking, Error Boundaries, or Featurely crash reporting.
---

# featurely-error-tracker

Pull current API details from `featurely-docs` (`get_api_reference` / `get_code_examples` with `package: "error-tracker"`) before writing code. This skill is the integration checklist.

**npm:** `featurely-error-tracker`  
**Permission:** `errors:write`  
**Docs:** https://docs.featurely.no/docs/sdks/error-tracker

## Install

```bash
npm install featurely-error-tracker
```

Or `npx featurely-cli@latest init` and select error-tracker.

## Required config

```ts
import { ErrorTracker } from 'featurely-error-tracker';

const tracker = new ErrorTracker({
  apiKey: process.env.NEXT_PUBLIC_FEATURELY_API_KEY!,
  environment: process.env.NODE_ENV === 'production' ? 'production' : 'development',
  appVersion: process.env.NEXT_PUBLIC_APP_VERSION ?? '1.0.0',
});
tracker.install();
tracker.setUser(user.id, user.email);
```

## Checklist

- Call `install()` once in a client entry / provider. Pair with `destroy()` on unmount.
- `setUser` after login.
- `reportError(error, severity, context)` for caught exceptions. Severity: `low` | `medium` | `high` | `critical`.
- Production: enable `privacy.scrubPII` and scrub query params (`token`, `apiKey`, `password`).
- Session replay needs `enableSessionReplay: true` **and** `projectId`.
- Next.js: `'use client'` provider; don't instantiate on the server.
- Optional: toasts, Web Vitals (`performance`), offline queue, `sampleRate`, `beforeSend` to drop health-check noise.

## Don't

- Don't send errors without `errors:write`.
- Don't put the live key in source. Use env vars.
- Don't leave `install()` in React without cleanup — it registers global handlers.
