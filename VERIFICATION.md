# Verification checklist

Verified on 14 November 2026 in `E:\RENT READY`.

## Automated repository proof

| Check                      | Result                       |
| -------------------------- | ---------------------------- |
| Prettier formatting        | Pass                         |
| ESLint static analysis     | Pass                         |
| Node unit/repository tests | Pass: 14/14                  |
| Vite production build      | Pass: 65 modules transformed |
| npm high-severity audit    | Pass: 0 vulnerabilities      |
| Supabase CLI               | Pass: 3.1.0 available        |

The tests cover inclusive pricing, integer-centavo calculations, date overlap, per-day capacity, maintenance stock, non-reserving statuses, edit exclusion, status transitions, safe HTML, relational mappers, authentication input, and repository error propagation.

Reproduce the frontend proof with:

```powershell
npm.cmd run check
npm.cmd run build
npm.cmd audit --audit-level=high
```

## Database proof boundary

The versioned pgTAP suite checks the primary tenant tables, keys, RPCs, and operation-specific RLS policies. It was not executed on this machine because Docker is not installed and no linked hosted Supabase project was supplied.

After Docker Desktop is running, execute:

```powershell
npm.cmd run supabase:start
npm.cmd run supabase:reset
npm.cmd run supabase:test
```

Do not treat the production database as approved until those commands pass and staging verifies two-business isolation, owner/manager/staff permissions, simultaneous booking confirmation, payment limits, invitation/account recovery, and private image access.

## Manual browser acceptance

Before launch, test both a narrow mobile viewport and desktop: sign up/sign in, create and switch businesses, invite roles, create equipment/customer/booking, confirm and advance a rental, record a payment, refresh persistence, upload an image, print/save a quotation, sign out, and verify a second tenant cannot access the first tenant's IDs or files.
