# Affiliate & Referral Tracking Platform

A full-stack portfolio platform for affiliate link generation, click/signup attribution, CPA/revenue-share/hybrid commissions, ledger-based accounting, fraud signals and partner/admin dashboards.

**Stack:** React + Vite + TypeScript, Node.js + Express, PostgreSQL, Redis + BullMQ, Docker Compose and Vitest.

## Local setup
1. Copy `.env.example` to `.env` and replace the development secrets.
2. Start services: `docker compose up -d`
3. Install: `npm install`
4. Apply schema: `npm run db:migrate -w apps/api`
5. Seed admin: `npm run db:seed -w apps/api`
6. Start apps: `npm run dev`

Web: http://localhost:5173 · API: http://localhost:4000/health

See this README and source files for routes, architecture and development notes. This is a learning MVP, not a production payment system. Configure trusted conversion authentication and review security/privacy requirements before deployment.