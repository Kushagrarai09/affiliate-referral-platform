# PartnerPulse — Affiliate & Referral Tracking Platform

Full-stack TypeScript portfolio project with React/Vite, Express, PostgreSQL, Redis/BullMQ worker, affiliate referral links, click tracking, signup attribution, CPA/revenue-share/hybrid commission calculations, ledger postings, admin approvals, payout batching, fraud alerts, and dashboards.

## Run locally
Prerequisites: Node.js 20+, npm, Docker Desktop.

1. Copy `.env.example` to `.env` and replace the secrets before anything beyond local development.
2. Start services: `docker compose up -d`
3. Install dependencies: `npm install`
4. Apply schema: `npm run db:migrate -w apps/api`
5. Seed administrator: `npm run db:seed -w apps/api`
6. Run all apps: `npm run dev`

Web: http://localhost:5173 · API: http://localhost:4000/health

Admin login is configured through `ADMIN_EMAIL` and `ADMIN_PASSWORD` in `.env`. Affiliates can register through the UI. Do not use the sample admin password outside local development.

## Key endpoints
- `POST /api/auth/register` and `POST /api/auth/login`
- `POST /api/referral-links` (affiliate JWT)
- `GET /r/:code` click tracking and redirect; optional `?campaign=<campaign UUID>`
- `POST /api/attributions/signup` with `{ "userId": "<UUID>", "referralToken": "<token>" }`
- `POST /api/conversions` with `X-Internal-Secret` and `{ "eventId": "unique-event-id", "userId": "<UUID>", "type": "signup|revenue|adjustment", "netRevenueMinor": 0 }`
- `GET /api/affiliate/dashboard`, `GET /api/affiliate/commissions`
- `GET /api/admin/commissions`, `POST /api/admin/commissions/:id/approve`
- `GET /api/admin/fraud-alerts`, `POST /api/admin/payouts` with `{ "commissionIds": ["..."] }`

## Commission rules
Amounts use integer minor units (paise). Signup events trigger CPA for CPA/hybrid rules. Revenue events calculate a share using basis points. Hybrid combines both. Commission creation, event idempotency, and ledger writes use PostgreSQL transactions and uniqueness constraints. Refunds should be separate negative adjustment events and must be reconciled deliberately; historical ledger entries should not be edited.

## Security and production caveats
This is a local learning MVP, not a production payment processor. The conversion endpoint requires `INTERNAL_CONVERSION_SECRET`; only a trusted backend should call it. The signup attribution endpoint is also intended for a trusted signup service and should be service-authenticated before deployment. Payout batches record approved commissions but do not move real money. Fraud heuristics can produce false positives; IPs are risk signals, not proof. Before production add rate limiting, CSRF strategy where applicable, email verification/password reset, secure secret management, audit/monitoring, privacy and cookie-consent controls, reconciliation, and security review.

## Useful commands
- `npm run build`
- `npm run test -w apps/api` (if tests are added)
- `docker compose down` to stop local services; `docker compose down -v` removes database data.
