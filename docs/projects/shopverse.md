# Shopverse

- **URL:** <https://github.com/khaneliyas01/shopverse>
- **Visibility:** Private
- **Language:** TypeScript
- **Stack:** pnpm monorepo · Express · React 18 + Vite · Node >=20

## What it is

A full-stack commerce reference implementation: product catalog, search/filter,
product detail, cart, checkout, coupons, order lifecycle, an admin dashboard,
and a pluggable payment provider (deterministic test provider by default).

## Repo layout

Monorepo at `~/ecommerce` with three packages:

- **`shared`** (`@shopverse/shared`) — shared TypeScript contracts/types, consumed as source
- **`server`** (`@shopverse/server`) — Express REST API (TS strict, ESM), JWT auth, file-backed store, runs via `tsx`
- **`web`** (`@shopverse/web`) — React 18 SPA with storefront + admin panel

## Status (2026-08-18)

Release-readiness pass complete, all verification green:

- Server test suite: 55/55 passing (`NODE_ENV=test pnpm --filter @shopverse/server test`)
- All three packages typecheck (`pnpm -r exec tsc --noEmit`)
- Web production build works (`pnpm --filter @shopverse/web build` → `web/dist`) — was previously broken (missing `index.html`, missing `@shopverse/shared` dep, several tsc unused-import and type errors, fixed)
- `shared/tsconfig.json` added for standalone typecheck
- `pnpm audit --prod` clean: upgraded `react-router-dom` ^6.27.0 → ^7.18.0 (fixed 3 moderate advisories)
- DevOps scaffold: repo initialized (`git init -b main`), root `Dockerfile` (node:22-alpine, multi-stage, `pnpm deploy --prod`, non-root user, secrets injected at runtime), `.github/workflows/ci.yml` (typecheck + server tests + web build on push/PR to main), root `README.md`
- Boot smoke on :4000 verified: `/health` ok, 12 catalog items, admin login returns role `admin`

## Security posture

- JWT auth with fail-hard when `JWT_SECRET` missing in production (`server/src/config/index.ts`)
- Payment webhook verified via HMAC-SHA256 `x-payment-signature` header (`POST /api/v1/webhook/payment`); dev secret `test-webhook-secret` only
- Helmet, CORS allow-list, per-IP rate limiting (auth + global), bcrypt password hashing
- Dev-only seeded admin: `admin@shopverse.test` (credentials are dev defaults, not secrets)

## Notes

- Server runs via `tsx` (no compiled emit); `start` script uses `node --experimental-strip-types src/main.ts`
- Not yet pushed to a remote; first commit pending
