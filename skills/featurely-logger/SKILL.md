---
name: featurely-logger
description: Install and configure featurely-logger — structured event logging (error/warn/info/debug/trace) with batching and flush-on-exit. Use when adding Featurely logs, replacing console.log in server code, or sending logs from workers/lambdas.
---

# featurely-logger

Pull current API details from `featurely-docs` (`package: "logger"`) before writing code.

**npm:** `featurely-logger`  
**Permission:** `logs:write` (not created by default CLI auto-init — set this up manually)  
**Docs:** https://docs.featurely.no/docs/sdks/logger

## Install

```bash
npm install featurely-logger
```

## Required config

```ts
import { createLogger } from 'featurely-logger';

export const logger = createLogger({
  apiKey: process.env.FEATURELY_API_KEY!,
  projectId: process.env.FEATURELY_PROJECT_ID!,
  source: 'server',
  minLevel: process.env.NODE_ENV === 'production' ? 'info' : 'debug',
});

logger.info('User signed in', { category: 'auth', userId: 'u_123' });
logger.error('Stripe webhook failed', { category: 'billing', data: { statusCode: 500 } });
```

Prefer a singleton module. `createLogger` and `new FeaturelyLogger` are both valid.

## Checklist

- Server/worker keys should **not** use the `NEXT_PUBLIC_` prefix.
- Levels: `trace` < `debug` < `info` < `warn` < `error`. Entries below `minLevel` are dropped client-side.
- Options: `category`, `data`, `userId`, `fingerprint` (dedup / occurrence count).
- Batching: `batchSize` default 10, `flushInterval` default 5000ms, `autoFlushOnExit` default true.
- Serverless: `await logger.flush()` in a `finally` before the isolate exits.
- Logs show in Featurely → Event Logs, alongside error-tracker events.

Don't call `POST /api/public/v1/logs` by hand unless the user is writing a non-JS client. The SDK already batches.
