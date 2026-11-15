# RentReady

RentReady is a responsive, multi-tenant rental-management application for small Philippine event-equipment businesses. It uses a vanilla JavaScript/Vite frontend and a Supabase backend. Without Supabase environment variables it runs in an isolated local demo mode; with them it enables authentication, workspaces, cloud data, row-level tenant isolation, image storage, staff invitations, and transactional booking rules.

## Run locally

```powershell
npm.cmd install
npm.cmd start
```

Open the address Vite prints, normally `http://127.0.0.1:5173`. Do not run `node server.mjs`; RentReady uses Vite and no `server.mjs` file exists.

For a production-backed session, copy `.env.example` to `.env` and enter a Supabase project URL and browser-safe publishable key. Never put a database password, secret key, service-role key, or access token in `.env`.

## Quality checks

```powershell
npm.cmd run check
npm.cmd run build
npm.cmd run supabase:test
```

The database test requires Docker Desktop and a running local Supabase stack. See [Supabase setup](docs/SUPABASE_SETUP.md) and [deployment gates](docs/DEPLOYMENT.md).

## Main capabilities

- Email authentication, business onboarding, business switching, and owner/manager/staff roles
- Tenant-isolated equipment, customers, bookings, payments, members, and activity history
- Equipment add/edit/archive, maintenance stock, search, filtering, and private image storage
- Inclusive-date booking prices, item snapshots, discounts, delivery fees, and quotations
- Race-safe booking confirmation and per-day inventory availability in PostgreSQL
- Controlled rental status workflow and overpayment protection
- Dashboard, calendar/agenda, customer history, responsive navigation, and print-to-PDF quotation
- Empty browser-local workspace when Supabase is not configured

## Core rules

- A tenant is a rental business; that business's rental customers are records inside the tenant.
- Rental dates are inclusive. A same-day rental bills one day.
- Money is stored as integer Philippine centavos.
- Confirmed and Out for Delivery bookings reserve stock. Draft, Completed, and Cancelled bookings do not.
- Availability is checked for every calendar day. Database row locks prevent competing confirmations from overbooking.
- Item names and rates are copied into booking items, preserving historical quotations.
- Payments cannot exceed the booking total. Cancellation preserves the record and payment history.
- PostgreSQL Row Level Security—not hidden UI—is the tenant and role security boundary.

## Project structure

```text
RentReady/
├── index.html
├── css/styles.css
├── js/
│   ├── app.js                 # Application composition and event routing
│   ├── core/                  # Shared business concepts
│   ├── data/supabase/         # Authenticated repositories and workspace loading
│   ├── features/              # Feature-specific views
│   ├── ui/                    # Safe UI helpers
│   ├── availability.js        # Demo-mode reservation logic
│   ├── calculations.js        # Dates, currency, and totals
│   └── storage.js             # Empty local-workspace persistence
├── supabase/
│   ├── migrations/            # Version-controlled schema, RLS, triggers, and RPCs
│   ├── functions/             # Server-side staff invitation
│   └── tests/database/        # pgTAP schema/policy tests
├── tests/                     # Node unit and repository-contract tests
├── docs/                      # Architecture, setup, deployment, and phase status
└── .github/workflows/         # Frontend and database CI
```

## Production status

Repository implementation for phases 1–7 is complete. A real launch still requires user-owned infrastructure, credentials, domain configuration, staging checks, and legal/business approval. The exact boundary is documented in [phase status](docs/PHASE_STATUS.md).

## Supported browsers

Current Chrome, Edge, Firefox, and Safari releases.
