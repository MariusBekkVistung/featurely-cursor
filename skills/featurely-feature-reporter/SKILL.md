---
name: featurely-feature-reporter
description: Install and configure featurely-feature-reporter — the embeddable feedback widget for feature requests and bug reports. Use when adding a feedback button, feature-request form, or in-app bug reporter.
---

# featurely-feature-reporter

Pull current API details from `featurely-docs` (`package: "feature-reporter"`) before writing code.

**npm:** `featurely-feature-reporter`  
**Permission:** `features:write`  
**Docs:** https://docs.featurely.no/docs/sdks/feature-reporter

## Install

```bash
npm install featurely-feature-reporter
```

## Required config

```ts
import { FeatureReporter } from 'featurely-feature-reporter';

const reporter = new FeatureReporter({
  apiKey: process.env.NEXT_PUBLIC_FEATURELY_API_KEY!,
  projectId: process.env.NEXT_PUBLIC_FEATURELY_PROJECT_ID!,
  position: 'bottom-right',
  color: 'blue',
});
reporter.init();
reporter.setUser(user.id, user.email, user.name);
```

Widget renders into `document.body`. React components should return `null` and `destroy()` on unmount.

## Checklist

- `projectId` is required (unlike some other packages).
- Positions: `bottom-right` | `bottom-left` | `top-right` | `top-left` | `right-center` | `left-center`
- Colors: `blue` | `green` | `purple` | `red` | `orange` | `pink`
- Icons: `comment` | `lightbulb` | `bug` | `message` | `plus` | `star`
- Toggle `enableFeatureRequests` / `enableBugReports` instead of shipping a half widget.
- `showDashboardLink` exposes the public board.
- Next.js App Router: `'use client'` singleton in `useEffect`.

Submissions land in Featurely as features (`features:write`) and user-reported bugs. After install, product questions go through the `featurely` skill / MCP, not this package.
