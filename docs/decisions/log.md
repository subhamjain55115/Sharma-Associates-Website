# Decision Log

Implementation decisions too small for an ADR but too important to re-derive each session.

| Date | Decision | Reason | Phase |
|---|---|---|---|
| 2026-10-14 | Chose SDLC Lite track | Small marketing site with one integration and fewer than fifteen tasks | Phase -1 |
| 2026-10-14 | Booking via form and email instead of a calendar embed or custom system | Simplest option that proves the core loop, calendar embed deferred to V2 | Phase -1 |
| 2026-10-14 | Resend for transactional email | Simple API and good fit with Vercel, no SMTP to manage | Phase -1 |
| 2026-10-14 | Upstash Redis for rate limiting | In-memory counters do not work across serverless instances; no application data stored | Phase -1 |
| 2026-10-14 | Phase 2 done criterion relaxed to firm email only | Resend sends to arbitrary visitors only from a verified domain, which happens in Phase 4 | Phase -1 |
| 2026-10-14 | AccountingService schema type instead of LocalBusiness | More specific for a CA firm | Phase -1 |
| 2026-10-14 | Accept dev-only braces audit findings and gate on npm audit --omit=dev | No patched braces exists (advisory covers all versions through 3.0.3); chain is eslint-config-next lint tooling only, production audit is clean | Phase 1 |
