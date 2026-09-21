---
name: featurely-site-manager
description: Install and configure featurely-site-manager — maintenance mode, feature flags, A/B tests, status banners, version checks, heatmaps, and analytics. Use when adding remote config, flags, maintenance pages, or Featurely analytics.
---

# featurely-site-manager

Pull current API details from `featurely-docs` (`package: "site-manager"`) before writing code.

**npm:** `featurely-site-manager`  
**Permission:** `public:read`  
**Docs:** https://docs.featurely.no/docs/sdks/site-manager

## Install

```bash
npm install featurely-site-manager
```

## Required config

```ts
import { SiteManager } from 'featurely-site-manager';

const manager = new SiteManager({
  apiKey: process.env.NEXT_PUBLIC_FEATURELY_API_KEY!,
  projectId: process.env.NEXT_PUBLIC_FEATURELY_PROJECT_ID!,
  userId: user?.id,
  userEmail: user?.email,
});
await manager.init();
```

Always `await init()`. Call `destroy()` on unmount (stops polling, flushes analytics).

## Capabilities

| Job | How |
|-----|-----|
| Feature flags | `isFeatureEnabled(key)`, `getFeatureVariant(key)`, `getEnabledFeatures()` |
| Maintenance | `isInMaintenanceMode()`, `onMaintenanceEnabled` / `onMaintenanceDisabled` |
| Status banners | Auto-injected unless `autoInjectBanners: false` |
| Analytics | `trackEvent`, `trackRevenue(name, amountInCents, currency)` |
| Version | `enableVersionCheck`, `appVersion`, `onUpdateRequired` / `onUpdateAvailable` |
| Heatmaps / rage click / scroll / perf | opt-in flags on the constructor |
| Debug overlay | `debugMode` or `?ft_debug=<debugSecret>` |

## Checklist

- Pass `userId` for flag bucketing and analytics identity.
- Pass `userEmail` so maintenance whitelist works.
- SSR: `bootstrapFlags` to avoid layout shift; `environment` when hostname isn't available.
- Don't poll more aggressively than the SDK default unless asked.
- Custom banner UI requires `autoInjectBanners: false` plus `onMessageReceived`.
