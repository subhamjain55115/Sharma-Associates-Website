# Sharma & Associates Website

Public marketing website for Sharma & Associates, a Chartered Accountancy firm in Mumbai. Goal: convert small business owners and startups into consultation bookings. Track: SDLC Lite.

## Stack

| Layer | Choice |
|---|---|
| Framework | Next.js App Router, TypeScript |
| Styling | Tailwind CSS |
| Email | Resend |
| Validation | Zod |
| Rate limiting | Upstash Redis (Vercel Marketplace) |
| Hosting | Vercel |
| Tests | Vitest, Playwright |

## Constraints

- Secrets (Resend key, firm email) live only in environment variables.
- Validate all form input on the server with Zod. Honeypot and rate limiting on the booking action.
- Mobile-first, WCAG 2.2 AA.
- No application database, no auth, no payments, no CMS in V1. Upstash Redis is for rate limiting only. Ideas go to `docs/v2-backlog.md`.
- Run `npm audit` right after scaffolding.

## Where to look

- Roadmap and project state: `docs/development-roadmap.md` (read the Project State table first)
- Design spec: `docs/superpowers/specs/2026-10-14-sharma-associates-website-design.md`
- Backlog: `docs/v2-backlog.md`
- Decisions: `docs/decisions/log.md`
