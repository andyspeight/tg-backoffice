# tg-backoffice

Travelgenix **back office** — the single source of truth for **bookings, financials and
suppliers**. A Next.js (App Router, TypeScript) app over the Supabase `backoffice` schema in
the **Travelgenix CRM** project (`iexryjynfaktfbvzlwlx`).

It connects to **Postgres directly, server-side** (the `backoffice`/`contracts` schemas are not
exposed to PostgREST), matching how `contract-loader` works. The schema itself is defined in the
sibling `db/backoffice/*.sql` migrations (already applied to Supabase).

## Pages
- **Overview** — KPIs: bookings, sales value, margin, AR balance due, AP payables.
- **Bookings** — `backoffice.v_booking_finance`.
- **Payments & AR** — `backoffice.v_receipts_due` (deposit/balance schedule).
- **Suppliers & AP** — supplier master + outstanding payables.

## Run locally
```bash
npm install
cp .env.example .env.local   # set DATABASE_URL (Supabase transaction pooler, service role)
npm run dev
```

## Env
| Var | What |
|-----|------|
| `DATABASE_URL` | Supabase **transaction pooler** connection string (port 6543), service/postgres role. See `.env.example`. |

## Deploy (Vercel)
1. Create a Vercel project from this repo (framework: Next.js, Node 24.x — same as `travelgenix-crm`).
   The Next.js app is at the **repo root**, so the project's Root Directory is the default (`./`).
2. Add `DATABASE_URL` as an environment variable (Production + Preview).
3. Deploy.

## Repo layout
```
.            Next.js app root (package.json, next.config.mjs, src/)
src/app      App Router pages (overview, bookings, payments, suppliers)
src/lib      db.ts — server-only postgres.js client
db/backoffice  SQL mirror of the applied Supabase migrations (source of truth = Supabase history)
docs         design & build log
HANDOVER.md  full context for picking the project back up
```
