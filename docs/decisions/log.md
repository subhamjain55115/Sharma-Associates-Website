# Decision Log

Implementation decisions too small for an ADR but too important to re-derive each session.

| Date | Decision | Reason | Phase |
|---|---|---|---|
| 2026-10-14 | Chose SDLC Lite track | Small marketing site with one integration and fewer than fifteen tasks | Phase -1 |
| 2026-10-14 | Booking via form and email instead of a calendar embed or custom system | Simplest option that proves the core loop, calendar embed deferred to V2 | Phase -1 |
| 2026-10-14 | Resend for transactional email | Simple API and good fit with Vercel, no SMTP to manage | Phase -1 |
