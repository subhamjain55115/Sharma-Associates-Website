# Sharma Associates Website — Development Roadmap

## Project State

| Phase | Status | Completed | Notes |
|---|---|---|---|
| Phase -1 Inception | DONE | 2026-10-14 | Spec approved and review fixes applied |
| Phase 1 Setup | PENDING | — | — |
| Phase 2 Core Build | PENDING | — | — |
| Phase 3 Quality Pass | PENDING | — | — |
| Phase 4 Ship | PENDING | — | — |

## Project

| Field | Value |
|---|---|
| Name | Sharma Associates Website |
| Description | Public marketing website for Sharma & Associates, a Chartered Accountancy firm in Mumbai, built to convert visitors into consultation bookings |
| Tagline | A trustworthy, modern CA firm for growing businesses in Mumbai |
| Core Principle | Secrets live only in environment variables and all form input is validated on the server |
| V1 Scope Boundary | Inside V1: four service pages, about page, and a consultation form that emails the firm. Deferred: client portal, login, blog or CMS, payments, live calendar slots, multi-language |
| Architecture Overview | Next.js App Router site on Vercel. The consultation form submits to a server action that validates input with Zod, applies honeypot and per-IP rate limiting backed by Upstash Redis, then sends one email to the firm and one confirmation to the visitor through Resend. No application database. |
| Success Metric V1 | Deployed on a real domain, a test booking reaches the firm's inbox, and Lighthouse scores 90 or higher on mobile |

## Tech Stack

| Layer | Choice | Why |
|---|---|---|
| Framework | Next.js App Router with TypeScript | SSG and SSR for SEO, server actions for the form, native Vercel fit |
| Styling | Tailwind CSS | Fast mobile-first styling with consistent design tokens |
| Email | Resend | Simple transactional email API with no SMTP to manage |
| Validation | Zod | Schema validation at the server boundary |
| Rate limiting | Upstash Redis via Vercel Marketplace | In-memory counters do not work across serverless instances |
| Hosting | Vercel | Zero-config deploys for Next.js |
| Tests | Vitest and Playwright | Unit tests for logic and one E2E test for the booking flow |

## Brainstorm

| Field | Value |
|---|---|
| Problem | Sharma & Associates has no public web presence that convinces small business owners to trust the firm and book a consultation |
| Pain Intensity | Lite — not explored |
| Evidence | Lite — not explored |
| User | A small business owner or startup founder in Mumbai, browsing on a phone, looking for GST, accounting, audit or company setup help |
| Current Alternative | Lite — not explored |
| Insight | Lite — not explored |
| Core Loop | Visitor lands on the site, reads the services, submits the consultation form, and the firm receives the request and confirms by reply |
| What This Is Not | Not a client portal, no login, no blog or CMS, no payments, no live calendar booking, not multi-language |

## Plan

### Key Features V1
- Home page with a hero, a book-a-consultation call to action, services summary, why-us and placeholder testimonials
- Services pages for tax and ITR, GST and compliance, audit and assurance, and accounting and business setup
- About page covering the firm and team
- Contact and book page with the consultation form, address, phone and WhatsApp link
- Server-side validated booking action that emails the firm and confirms to the visitor
- SEO basics: per-page metadata, sitemap, robots and AccountingService structured data

### Key Constraints
- Secrets such as the Resend API key and the firm email address live only in environment variables
- All form input is validated on the server regardless of client checks
- Mobile-first layout that meets WCAG 2.2 AA
- No application database and no authentication in V1; Upstash Redis is used only for rate limiting
- Content is placeholder until the firm supplies real logo, team, address and contact details

## Phases

### Phase -1 — Inception [inception]
**Mission:** Get clarity on what is being built before any code exists
**Done when:** The design spec is approved by the user, the project CLAUDE.md exists, and this roadmap converts to JSON with exit code 0

| # | Task | CE Driver | Context Skills | Sub-op Plugins | Treatment |
|---|---|---|---|---|---|
| 1 | Answer the six Lite brainstorm questions | /ce-brainstorm | — | — | Name, problem, user, core loop, V1 features and success metric captured in conversation |
| 2 | Write and approve the design spec | /ce-plan | — | — | Spec lives at docs/superpowers/specs/2026-10-14-sharma-associates-website-design.md |
| 3 | Write the project CLAUDE.md | /ce-work | — | — | Short file covering what, stack and constraints |
| 4 | Write this roadmap and convert it to JSON | /ce-work | — | — | Run the converter until it exits 0 |

### Phase 1 — Setup [devops]
**Mission:** Anyone can clone the repo and see the site running in under ten minutes
**Done when:** A fresh clone with a filled-in .env runs with npm run dev and renders the home page, and npm audit --omit=dev reports no critical or high findings

| # | Task | CE Driver | Context Skills | Sub-op Plugins | Treatment |
|---|---|---|---|---|---|
| 1 | Scaffold the Next.js TypeScript Tailwind project | /ce-work | nextjs-mastery, typescript-advanced | /project-scaffold:scaffold | Use non-interactive create-next-app and keep the default App Router layout |
| 2 | Run npm audit right after scaffolding | /ce-work | security-hardening | /dependency-manager:audit-deps | Lessons.md 4: fix critical and high findings before any feature work; dev-only braces findings accepted, see Decision Log |
| 3 | Create .env.example and gitignore secrets | /ce-work | security-hardening | /env-manager:env-setup | Only keys the current code consumes are required, per lessons.md 11 |
| 4 | Initialise git with a branch strategy | /ce-work | git-advanced | /git-flow:flow-start | Check git status for stray files before staging, per lessons.md 1 |

### Phase 2 — Core Build [frontend]
**Mission:** Build the booking logic first, then the pages that use it, in dependency order
**Done when:** A booking submitted through the real form on the running site sends the firm email and shows the success state

| # | Task | CE Driver | Context Skills | Sub-op Plugins | Treatment |
|---|---|---|---|---|---|
| 1 | Define the booking Zod schema and unit tests | /ce-work | tdd-mastery, typescript-advanced | /test-writer:unit-test | Tests written alongside the schema covering valid, invalid and edge inputs |
| 2 | Build the booking server action with Resend | /ce-work | nextjs-mastery, security-hardening | — | Honeypot, Upstash per-IP rate limiting, two emails, structured error results; visitor email works once the sender domain is verified in Phase 4 |
| 3 | Build shared layout, navigation and design tokens | /ce-work | nextjs-mastery, styleseed | /frontend-developer:create-component | Mobile-first, semantic landmarks, 44px touch targets |
| 4 | Build Home and Services pages | /ce-work | frontend-excellence | /web-dev:add-page | Placeholder content marked clearly until the firm supplies real copy |
| 5 | Build About and Contact and Book pages | /ce-work | accessibility-wcag | /frontend-developer:create-component | Booking form wired to the server action with idle, submitting, success and error states |
| 6 | Add SEO metadata, sitemap, robots and LocalBusiness schema | /ce-work | nextjs-mastery | — | One metadata block per page and AccountingService JSON-LD for the Mumbai office |

#### Sub-steps

| Task | ID | Sub-step | Expected Output |
|---|---|---|---|
| 2 | 2a | Validate input with the Zod schema | Invalid input returns field-level errors |
| 2 | 2b | Add honeypot and Upstash rate limiting | Bot and repeat submissions are rejected |
| 2 | 2c | Send firm and visitor emails | Firm email arrives in a test inbox; visitor email is verified in Phase 4 |

### Phase 3 — Quality Pass [testing]
**Mission:** Verify the core logic, accessibility, security basics and performance before shipping
**Done when:** Unit and E2E tests pass, the axe scan reports no serious violations, and Lighthouse mobile scores are 90 or higher

| # | Task | CE Driver | Context Skills | Sub-op Plugins | Treatment |
|---|---|---|---|---|---|
| 1 | Write the Playwright booking flow E2E test | /ce-work | testing-strategies | /e2e-runner:run-e2e | Form submission through to the success state with email mocked at the boundary |
| 2 | Run the accessibility audit and fix findings | /ce-work | accessibility-wcag | /a11y-audit:run-audit | Keyboard-only pass plus axe scan |
| 3 | Security basics pass | /ce-code-review | security-hardening | /code-guardian:security-scan | No secrets in source, server validation confirmed, security headers set |
| 4 | Run Lighthouse and fix performance issues | /ce-optimize | performance-optimization | /lighthouse-runner:run-audit | Target 90 or higher on mobile for all four categories |
| 5 | Review the phase before committing | /ce-code-review | — | /double-check:verify | Multi-persona review with no unresolved blockers |

### Phase 4 — Ship [release]
**Mission:** Put the site live on the firm's real domain with a working booking flow
**Done when:** The site is live on the real domain and a test booking reaches the firm's inbox

| # | Task | CE Driver | Context Skills | Sub-op Plugins | Treatment |
|---|---|---|---|---|---|
| 1 | Deploy to Vercel with production environment variables | /ce-work | ci-cd-pipelines, security-hardening | /deploy-pilot:ci-pipeline | Set Resend key and firm email in Vercel, never in the repo |
| 2 | Connect the real domain and verify the Resend sender domain | /ce-work | — | — | DNS records for the site and for email deliverability |
| 3 | Run an end-to-end test booking on production | /ce-work | — | /double-check:verify | Confirm the firm inbox receives the request and the visitor receives the confirmation |
| 4 | Write the README | /ce-work | — | /readme-generator:generate-readme | Setup, environment variables and deploy steps |
| 5 | Run the launch checklist | /ce-work | — | /product-shipper:launch-checklist | Final gate before announcing the site |

## V2 Backlog

| Date | Idea | Why Deferred |
|---|---|---|
| 2026-10-14 | Calendar slot booking with a Cal.com embed | Form and email is enough for V1 |
| 2026-10-14 | Blog or CMS for articles and tax updates | Not in the V1 scope |
| 2026-10-14 | Client portal with login and document upload | Different product with its own security scope |
| 2026-10-14 | Hindi and Marathi language support | English only for V1 |
| 2026-10-14 | Online payments for consultation fees | Not needed to prove the booking flow |

## Decision Log

| Date | Decision | Reason | Phase |
|---|---|---|---|
| 2026-10-14 | Chose SDLC Lite track | Small marketing site with one integration and fewer than fifteen tasks | Phase -1 |
| 2026-10-14 | Booking via form and email instead of a calendar embed or custom system | Simplest option that proves the core loop, calendar embed deferred to V2 | Phase -1 |
| 2026-10-14 | Resend for transactional email | Simple API and good fit with Vercel, no SMTP to manage | Phase -1 |
| 2026-10-14 | Upstash Redis for rate limiting | In-memory counters do not work across serverless instances; no application data stored | Phase -1 |
| 2026-10-14 | Phase 2 done criterion relaxed to firm email only | Resend sends to arbitrary visitors only from a verified domain, which happens in Phase 4 | Phase -1 |
| 2026-10-14 | AccountingService schema type instead of LocalBusiness | More specific for a CA firm | Phase -1 |
| 2026-10-14 | Accept dev-only braces audit findings and gate on npm audit --omit=dev | No patched braces exists (advisory covers all versions through 3.0.3); chain is eslint-config-next lint tooling only, production audit is clean | Phase 1 |

## Phase Gate Checklist

Run before calling PHASE DONE on any phase. All four must be true.

- All tasks in the phase are complete
- The done criterion is met
- /ce-code-review passed with no unresolved blockers
- /ce-commit done

## Scope Lock

Ideas that surface mid-build go into docs/v2-backlog.md and the current phase continues unchanged. Full decision history lives in docs/decisions/log.md.
