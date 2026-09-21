---
name: featurely-i18n
description: Install and configure featurely-i18n — dashboard-managed translations with ETag caching, ICU plurals, namespace lazy-loading, and missing-key telemetry. Use when adding Featurely translations or switching locales.
---

# featurely-i18n

Pull current API details from `featurely-docs` (`package: "i18n"`) before writing code. The client API is `init()` / `t()` / `setLocale()` — do not invent a `load()`-only flow from older snippets.

**npm:** `featurely-i18n`  
**Permission:** `public:read`  
**Docs:** https://docs.featurely.no/docs/sdks/i18n

## Install

```bash
npm install featurely-i18n
```

## Required config

```ts
import { FeaturelyI18n } from 'featurely-i18n';

const i18n = new FeaturelyI18n({
  apiKey: process.env.NEXT_PUBLIC_FEATURELY_API_KEY!,
  projectId: process.env.NEXT_PUBLIC_FEATURELY_PROJECT_ID!,
  locale: 'en',
  fallbackLocale: 'en',
});
await i18n.init();
i18n.t('nav.home');
```

## Checklist

- `locale` is required (BCP 47: `en`, `nb`, `de`, …).
- Call `init()` (or `setLocale`) before `t()`.
- Interpolation and ICU plurals: use current `get_api_reference` examples, not guessed formatters.
- Namespaces: `namespace` on construct or `loadNamespace()` for lazy loads.
- SSR / RSC: `createServerI18n()` or `initialData` — don't hit `localStorage` on the server.
- Missing keys: `reportMissingKeys` defaults true and posts to `/i18n-telemetry`.
- Repeat visits use localStorage + ETag (`304`). Don't disable caching unless asked.

Keys are managed in Featurely → Translations, not in JSON files in the app unless the user wants a fallback.
