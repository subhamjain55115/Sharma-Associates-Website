# Sharma & Associates Website — Design Spec

Date: 2026-10-14
Track: SDLC Lite
Status: Draft, awaiting user review

## Purpose

A public marketing website for Sharma & Associates, a Chartered Accountancy firm in Mumbai. The site presents the firm as trustworthy and modern, and converts visitors into consultation bookings.

## Users and goal

- Primary visitor: small business owners and startups, likely on mobile.
- Core loop: visitor lands, reads services, submits the consultation form, firm receives the request and confirms by reply.

## Success metric (V1)

- Deployed on a real domain.
- A test booking submitted through the form reaches the firm's email inbox.
- Lighthouse score of 90 or higher on mobile (performance, accessibility, best practices, SEO).

## Assumptions

- English only.
- No client portal, no login, no payments.
- Content (team, testimonials, copy) is placeholder until the firm supplies real material.

## Services listed

1. Tax and ITR filing
2. GST and compliance
3. Audit and assurance
4. Accounting and business setup

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Framework | Next.js (App Router), TypeScript | SSR/SSG for SEO, server actions for the form, native Vercel fit |
| Styling | Tailwind CSS | Fast mobile-first, consistent design tokens |
| Email | Resend | Simple transactional email API, no SMTP management |
| Validation | Zod | Schema validation at the server boundary |
| Hosting | Vercel | Zero-config deploys for Next.js |
| Tests | Vitest, Playwright | Unit tests for logic, one E2E for the booking flow |

## Pages

- Home: hero with "Book a consultation" CTA, services summary, why-us, testimonials (placeholder).
- Services: one section or page per service.
- About: firm background and team.
- Contact and Book: consultation form, address, phone, WhatsApp link.

## Booking flow

1. Form fields: name, phone, email, service, preferred date and time, short message.
2. A server action validates input with Zod. Invalid input is rejected with field-level errors.
3. Abuse protection: honeypot field and per-IP rate limiting.
4. On success, send two emails via Resend: one to the firm with the request details, one confirmation to the visitor.
5. The firm confirms the slot by replying to the visitor. No calendar integration in V1.
6. UI states: idle, submitting, success, error. Errors use clear, actionable messages and never expose internals.

## Non-negotiable rules

- Secrets (Resend API key, firm email address) live only in environment variables, never in source or the client bundle.
- Mobile-first layout, WCAG 2.2 AA, semantic HTML, visible focus states, 44px minimum touch targets.
- SEO basics: per-page metadata, sitemap, robots, LocalBusiness structured data for the Mumbai office.
- All form input is validated on the server regardless of client-side checks.

## Out of scope for V1

- Client portal and authentication
- Blog or CMS
- Payments
- Live calendar slot booking (candidate for V2: Cal.com embed)
- Multi-language support

## Testing

- Unit: Zod schema (valid, invalid, edge cases), booking server action with the email client mocked at the boundary.
- E2E: one Playwright test covering form submission through the success state.
- Quality checks: axe accessibility scan and Lighthouse run before ship.

## Phases (SDLC Lite)

| Phase | Tag | Covers |
|---|---|---|
| -1 Inception | inception | This spec, project CLAUDE.md, roadmap |
| 1 Setup | devops | Scaffold, env files, git |
| 2 Core Build | frontend | Validation schema, booking action, pages, styling |
| 3 Quality Pass | testing | Tests, a11y, security basics, Lighthouse |
| 4 Ship | release | Deploy to Vercel, domain, README |

## Inputs needed from the firm

- Logo and brand colours
- Real team and firm details
- Receiving email address for bookings
- Phone number and WhatsApp number
- Office address
- Domain name
