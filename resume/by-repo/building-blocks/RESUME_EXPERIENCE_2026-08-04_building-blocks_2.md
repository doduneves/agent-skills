# Engineering highlights (CV source)

> **Purpose:** Personal draft material for resume / LinkedIn bullets.  
> **Scope:** One repository — Building Blocks (`vintasoftware/building-blocks`).  
> **Primary sources:** 8 merged PRs by `@doduneves` + 81 commits by `eduardo.neves@vinta.com.br` (2026-04-02 – 2026-04-24).  
> **Status:** Filled 2026-08-04 (refresh `_2` with ATS / LinkedIn export blocks).

---

## Role / context

| Field | Value |
| ----- | ----- |
| Company / product | Vinta Software — Building Blocks (FHIR-native healthcare product template) |
| Repository | `vintasoftware/building-blocks` |
| Your role | Software Engineer (full-stack, intake onboarding + shared UI/libs) |
| Date range | 2026-04 – 2026-04 |
| Areas / surfaces you owned | `@vinta-bb/ui` design system, `@vinta-bb/mail`, `@vinta-bb/intake-form` pre-wizard flows, `web-intake-app` auth/routing |
| GitHub username | `@doduneves` |
| Git author email(s) | `eduardo.neves@vinta.com.br` |

**One-line context (for CV header under the job):**  
Built shared UI, passwordless intake login, and transactional email foundations for a FHIR-native healthcare monorepo used across Vinta client projects.

---

## Features built

> One feature ≈ one merged PR (or a small PR stack). Prefer outcome language over file lists.

### Feature: Shared shadcn/ui design system (`@vinta-bb/ui`)

- **Summary:** Bootstrapped the monorepo’s shared component library on shadcn/ui + Tailwind, with TypeScript build output and Biome exemptions for vendored components so every app can import a single UI package.
- **Outcome bullets (CV-ready):**
  - Established `@vinta-bb/ui` as the cross-app design system (shadcn components, shared globals, dist build pipeline).
  - Configured monorepo tooling (Biome overrides, tsconfig, package README) so UI components ship consistently to intake, patient, and provider apps.
- **Scope:** shared package / design system / DX
- **Surfaces:** all web apps consuming `@vinta-bb/ui`
- **PRs:** #8
- **Notes:** Foundation work; later PRs (#15) layered theme tokens and intake-specific layout components on top. Merged 2026-04-06.

### Feature: Passwordless intake landing + OTP verification UI

- **Summary:** Shipped the first two steps of patient intake onboarding — email capture landing and 6-digit OTP entry — as reusable `@vinta-bb/intake-form` pages with tests, privacy copy, and responsive layout.
- **Outcome bullets (CV-ready):**
  - Delivered landing and OTP verification screens for passwordless patient login, including email validation, resend affordances, and compliance messaging.
  - Added Vitest coverage for landing/OTP pages and email validation utilities to lock in UX and edge cases before backend wiring.
- **Scope:** frontend / shared intake package / tests
- **Surfaces:** web-intake-app, `@vinta-bb/intake-form`
- **PRs:** #13
- **Notes:** UI-first; server send/verify landed in follow-up PRs (#25, #30). Merged 2026-04-08.

### Feature: Theme standardization and intake layout components

- **Summary:** Aligned intake flows with the shared design tokens in `@vinta-bb/ui` — updated globals, button variants, and reusable intake shells (`IntakeFormBox`, info panel, privacy note) for consistent branding.
- **Outcome bullets (CV-ready):**
  - Standardized color/spacing tokens and component styling across intake wizard, landing, and OTP pages.
  - Introduced reusable intake layout primitives so future steps inherit the same visual system without one-off CSS.
- **Scope:** design system / frontend
- **Surfaces:** `@vinta-bb/ui`, `@vinta-bb/intake-form`
- **PRs:** #15
- **Notes:** Paired with #8; reduced drift between app-level and package-level styling. Merged 2026-04-09.

### Feature: TanStack Router migration (web-intake-app)

- **Summary:** Replaced legacy React Router setup in the intake app with TanStack Router file-based routes, preserving existing navigation behavior while aligning with the monorepo’s TanStack Start direction.
- **Outcome bullets (CV-ready):**
  - Migrated intake app routing to TanStack Router with no functional regressions (verified via manual QA screencast).
  - Updated AGENTS.md / README routing docs so the team’s file-based route conventions match deployed Vite `base` paths.
- **Scope:** routing / app shell / docs
- **Surfaces:** web-intake-app
- **PRs:** #22
- **Notes:** Prerequisite for server functions and OTP routes wired in later PRs. Merged 2026-04-14.

### Feature: Transactional mail package (`@vinta-bb/mail`)

- **Summary:** Created a workspace mail package wrapping Mailgun with env-driven configuration, typed helpers, and build/tsconfig setup for server-side email from TanStack Start functions.
- **Outcome bullets (CV-ready):**
  - Built `@vinta-bb/mail` with Mailgun client factory, fail-closed env validation, and package README for local/prod setup.
  - Documented root `.env.example` entries so intake server functions can send mail without duplicating provider logic per app.
- **Scope:** shared library / integrations
- **Surfaces:** server functions (web-intake-app), reusable across apps
- **PRs:** #24
- **Notes:** Established Mailgun as the transactional email path for intake OTP. Merged 2026-04-15.

### Feature: 6-digit email OTP send and resend (Better Auth + Mailgun)

- **Summary:** Implemented server-side intake login: collect email on landing, send styled HTML/text OTP via Mailgun, support resend, and wire TanStack Start server functions with Zod schemas and unit tests.
- **Outcome bullets (CV-ready):**
  - Shipped passwordless email OTP delivery for intake login using `@vinta-bb/mail`, HTML/text templates, and TanStack Start server functions.
  - Added test coverage for login helpers, email body generation, and server handlers; integrated `escape-html` for safe template rendering.
- **Scope:** backend (server functions) / email / auth integration
- **Surfaces:** web-intake-app server layer, intake landing + verify pages
- **PRs:** #25
- **Notes:** Verification step completed in #30; forms refactor in #31. Merged 2026-04-22.

### Feature: OTP verification with Better Auth

- **Summary:** Connected the 6-digit OTP input to Better Auth verification so patients who enter the emailed code reach the intake wizard; invalid codes surface inline errors.
- **Outcome bullets (CV-ready):**
  - Integrated Better Auth OTP verification end-to-end — successful verify redirects into the intake wizard; failures show actionable error states.
  - Extended server function and page tests for verify/resend flows to guard auth regressions.
- **Scope:** auth / server functions / frontend
- **Surfaces:** web-intake-app, `@vinta-bb/intake-form` verify page
- **PRs:** #30
- **Notes:** Completes the passwordless login story started in #13 and #25. Merged 2026-04-23.

### Feature: TanStack Form migration (pre-wizard intake forms)

- **Summary:** Refactored landing email and OTP forms from ad-hoc state to TanStack Form + Zod schemas, splitting presentational pages from form components with dedicated tests and package README updates.
- **Outcome bullets (CV-ready):**
  - Migrated intake landing and OTP forms to TanStack Form with Zod validation, async submit handlers, and shared `FieldError` patterns.
  - Extracted `IntakeLandingForm` / `IntakeVerifyOtpForm` components with co-located tests, simplifying page shells and improving validation error UX.
- **Scope:** frontend / forms / tests / docs
- **Surfaces:** `@vinta-bb/intake-form`
- **PRs:** #31
- **Notes:** Depends on Zod + `@tanstack/react-form` added in same effort; aligns pre-wizard flows with wizard form patterns. Merged 2026-04-27.

<!-- Duplicate the block above per feature. -->

---

## Tech used

> Group for the CV “tech stack” line; keep only what you actually used in this repo.

### Languages & runtime

- TypeScript (strict)
- Node.js (≥22; monorepo tooling)

### Frontend

- React
- TanStack Router (file-based routes)
- TanStack Form + Zod
- shadcn/ui, Radix UI, Tailwind CSS
- Tabler icons (intake UI)

### Backend / services

- TanStack Start server functions (web-intake-app)
- Better Auth (email OTP plugin)

### Data / storage

- Medplum / FHIR (repo context; primary persistence for intake data — not primary focus of authored PRs)

### Integrations / third parties

- Mailgun (`@vinta-bb/mail` / `mailgun.js`)
- Better Auth

### Tooling & quality

- Vitest + Testing Library (page and server function tests)
- Biome (lint/format)
- pnpm workspaces

### Infra / delivery

- Vite (workspace app bundler)
- Vercel-deployed apps (monorepo context — independent projects per SPA)

**Short CV stack line (draft):**  
`TypeScript · React · TanStack Router/Form/Start · Better Auth · Mailgun · shadcn/ui · Tailwind · Zod · Vitest · pnpm monorepo`

---

## Migrations / one-off scripts

> Prefer scripts you authored or owned. Note safety patterns (dry-run by default, batching, backups, idempotency) when relevant.

| Script / migration | Purpose | Approx. records / users affected | Env (staging → prod) | PR / path | Notes |
| ------------------ | ------- | -------------------------------- | -------------------- | --------- | ----- |
| — | No migrations or operational scripts authored in this repo slice | — | — | — | Greenfield auth/UI work |

**CV-ready bullets (draft):**

- _(none evidenced — intake auth work was greenfield server functions + UI, not data backfills)_

**How to estimate impact (when git doesn’t say):**

- Dry-run / commit logs from the script run
- Plan or PR description counts
- Ops / analytics / product follow-up
- If unknown, write `unknown — needs estimate` and do not invent numbers

---

## Impact / metrics

> Only include numbers you can defend. Leave blank rather than guess.

| Metric | Value | Source |
| ------ | ----- | ------ |
| Users / customers / accounts affected | unknown — needs estimate | template/dev Medplum project |
| Entities touched (orders, records, jobs, …) | unknown — needs estimate | no production rollout metrics in PRs |
| Teams / tenants / environments in scope | Single-project template (Vinta internal + client reuse) | README / AGENTS.md |
| Error rate / support tickets reduced | — | — |
| Latency / cost / perf improvement | — | — |

**CV-ready impact lines:**

- _(defer until production adoption metrics available)_

---

## Architecture / domain

> High-signal domain ownership — strong for senior-leaning bullets.

- **System shape:** pnpm monorepo — `apps/` (TanStack Start SPAs) + `packages/` (shared features/libs)
- **Domain focus:** healthcare patient intake onboarding, passwordless auth, transactional email
- **Cross-cutting concerns you owned:** shared UI package, mail abstraction, intake pre-wizard UX, app routing modernization
- **Your focus areas:** `@vinta-bb/ui`, `@vinta-bb/mail`, intake landing/OTP flows, web-intake-app auth server functions

**CV-ready bullets:**

- Contributed foundational layers (design system, mail, passwordless login) for a FHIR-native healthcare template reused across Vinta engagements.
- Separated presentational intake pages from form logic and server auth handlers to keep `@vinta-bb/intake-form` portable across host apps.

---

## Hard problems solved

> Depth markers: correctness under edge cases, scale, consistency, security, migrations, ambiguous product rules, etc.

| Problem | What was hard | Approach | Outcome |
| ------- | ------------- | -------- | ------- |
| Safe OTP email rendering | User-supplied content in HTML templates | `escape-html` + structured text/HTML templates in `@vinta-bb/mail` flow | Send path covered by unit tests; templates separated from React UI |
| Fail-closed mail config | Missing Mailgun env vars could fail silently in dev | Throw on missing env in `mailgunConfigFromEnv` / client factory | Misconfiguration surfaces at startup/server call time |
| Form validation UX across login steps | Email + OTP needed consistent async validation and error display | TanStack Form `onSubmitAsync`, Zod schemas, `FieldError` replacement for custom error component | Unified patterns in #31 with dedicated form component tests |
| Router migration without UX regression | Intake app had legacy routing while monorepo standardized on TanStack | File-based TanStack Router routes, parity QA | Merged #22 with screencast verification |

**CV-ready bullets:**

- Hardened OTP email delivery with HTML escaping and explicit Mailgun env validation before production send paths.
- Unified intake login validation under TanStack Form + Zod after shipping Better Auth verification, improving error messaging without changing auth semantics.

---

## Quality & delivery

- Automated tests (unit / integration / e2e) for the paths you owned
- Feature flags or gradual rollout for risky changes
- Staging (or equivalent) before production
- Clear PR / stacked delivery when work was phased

**CV-ready bullets:**

- Added Vitest coverage for intake landing/OTP pages, login server functions, and TanStack Form components across #13, #25, #30, #31.
- Phased delivery: UI (#13) → theming (#15) → routing (#22) → mail lib (#24) → send OTP (#25) → verify OTP (#30) → form refactor (#31).
- Documented package usage in `@vinta-bb/ui`, `@vinta-bb/mail`, and `@vinta-bb/intake-form` READMEs plus AGENTS.md routing notes.

---

## Ownership & process

> Spec → plan → implement → review → ship (only claim what you did).

- Authored or drove: ClickUp-linked implementation for OTP send (#25), verify (#30), TanStack Form migration (#31)
- Reviewed: _(not evidenced in this export — add if you reviewed peer PRs)_
- Partnered with: product/design via ClickUp cards referenced in PR bodies
- Led rollout: feature PRs merged to main; production deploy via Vercel monorepo (no personal rollout comms evidenced)

**CV-ready bullets:**

- Delivered a sequenced PR stack from shared UI through passwordless login, each merge building on the prior layer without blocking parallel work.

---

## Cross-cutting work

> Often under-counted on resumes; call out if you owned them.

| Area | Examples you touched | CV-worthy? |
| ---- | -------------------- | ---------- |
| Auth / sessions / permissions | Better Auth email OTP send/verify, server functions | Y |
| Observability (logging, metrics, error tracking) | — | N |
| Notifications (email / SMS / push) | `@vinta-bb/mail`, intake login templates | Y |
| Payments / billing | — | N |
| Background jobs / bots / workers | — | N |
| Design system / shared UI | `@vinta-bb/ui` bootstrap + intake theming | Y |
| Developer experience / CI / tooling | Biome UI overrides, package build/tsconfig, AGENTS.md routing docs | Y |
| Security / compliance / privacy | HTML escaping for emails, privacy compliance note on landing | Y |

**CV-ready bullets:**

- Built reusable Mailgun mail package and intake OTP email templates used by TanStack Start auth handlers.
- Bootstrapped the monorepo shadcn/ui package consumed by healthcare SPAs in the template.

---

## Polished resume bullets (draft)

> Internal draft — refine here, then copy into **ATS export** and **LinkedIn export** below.  
> Aim for action + scope + outcome (+ metric when real).

1. Bootstrapped `@vinta-bb/ui`, a shared shadcn/ui design system (Tailwind tokens, monorepo build pipeline) used across Vinta’s FHIR-native healthcare template apps.
2. Shipped passwordless patient intake login: email landing, Mailgun OTP delivery, Better Auth verification, and wizard entry — with Vitest coverage for UI and server functions.
3. Created `@vinta-bb/mail`, a typed Mailgun wrapper with fail-closed env validation, enabling transactional email from TanStack Start server functions without per-app duplication.
4. Migrated the intake app from legacy React Router to TanStack Router and refactored pre-wizard forms to TanStack Form + Zod, standardizing validation and error UX.
5. Delivered reusable intake layout components (form shell, privacy/compliance copy, OTP input) in `@vinta-bb/intake-form` so host apps embed onboarding without rewriting UI.

---

## ATS export (single role — copy to master CV)

> **Do not submit this file to an ATS.** Copy the block below into your consolidated CV under **Experience**.  
> Plain text only: no tables, PR numbers, repo paths, code, or markdown formatting in the filled export.

```
Vinta Software (Building Blocks) | Software Engineer | Apr 2026 – Apr 2026

Built shared UI, passwordless intake login, and transactional email foundations for a FHIR-native healthcare monorepo template.

• Bootstrapped a shared shadcn/ui design system with Tailwind tokens and a monorepo build pipeline used across intake, patient, and provider apps.
• Shipped passwordless patient intake login end-to-end: email landing, Mailgun OTP delivery, Better Auth verification, and wizard entry with Vitest coverage.
• Created a typed Mailgun mail package with fail-closed env validation for transactional email from TanStack Start server functions.
• Migrated the intake app to TanStack Router and refactored pre-wizard forms to TanStack Form with Zod validation and clearer error UX.
• Delivered reusable intake layout components so host apps can embed onboarding flows without rewriting UI.

Skills: TypeScript, React, TanStack Router, TanStack Form, TanStack Start, Better Auth, Mailgun, shadcn/ui, Tailwind CSS, Zod, Vitest, pnpm
```

**Rules when filling:**

- 3–6 bullets max — prioritize highest-signal outcomes from this repo.
- Use a consistent date format across all repo exports (e.g. `Jan 2024 – Present`).
- Metrics only when evidenced in **Impact / metrics** above.
- **Skills** line: comma-separated keywords from **Tech used** (what you actually used here).

---

## LinkedIn export (Experience entry — copy to profile)

> Copy fields below into LinkedIn → Experience. Slightly more narrative than ATS is fine; still no PR numbers or repo paths.

**Title:** Software Engineer  
**Company:** Vinta Software  
**Dates:** Apr 2026 – Apr 2026  
**Location:** Remote

**Description:**

Contributed foundational layers for Building Blocks, Vinta’s FHIR-native healthcare product template — shared design system, transactional email, and passwordless patient intake login on a TanStack Start monorepo.

• Bootstrapped a shared shadcn/ui design system with Tailwind tokens and a monorepo build pipeline used across intake, patient, and provider apps.  
• Shipped passwordless patient intake login end-to-end: email landing, Mailgun OTP delivery, Better Auth verification, and wizard entry with Vitest coverage.  
• Created a typed Mailgun mail package with fail-closed env validation for transactional email from TanStack Start server functions.  
• Migrated the intake app to TanStack Router and refactored pre-wizard forms to TanStack Form with Zod validation and clearer error UX.  
• Delivered reusable intake layout components so host apps can embed onboarding without rewriting UI.

**Skills to tag (optional):** TypeScript, React, TanStack Router, TanStack Form, Better Auth, Mailgun, shadcn/ui, Tailwind CSS, Zod, Vitest

---

## What not to put on the CV

- Tiny chore-only commits (config noise, typo fixes) unless they unblock a real story
- “Used Cursor / AI” as a skill bullet (optional elsewhere; not as impact)
- Internal PR numbers without product language
- Invented user counts or metrics
- Confidential customer, patient, or proprietary detail

---

## Raw sources (reference only — not for the CV)

### How to fill this file from any repo

```bash
# Commits by you
git log --author='eduardo.neves@vinta.com.br' --oneline --no-merges

# Merged PRs by you (needs gh auth; run inside the repo)
gh pr list --author=@me --state merged --limit 100

# Optional: commits touching migrations / operational scripts
git log --author='eduardo.neves@vinta.com.br' --oneline -- scripts/ migrations/ db/
```

### Key PRs / commit ranges

| PR | Title | Merged | Feature cluster |
| -- | ----- | ------ | --------------- |
| #8 | Setup: @vinta-bb/ui package | 2026-04-06 | Shared shadcn/ui design system |
| #13 | Feat/otp verification | 2026-04-08 | Passwordless intake landing + OTP UI |
| #15 | Refactor: Standarlize theme and UI components | 2026-04-09 | Theme standardization |
| #22 | refactor(intake): Migrate TanStack Router for Intake App | 2026-04-14 | TanStack Router migration |
| #24 | Feat/mail package | 2026-04-15 | `@vinta-bb/mail` (Mailgun) |
| #25 | Feat: Send 6 digit email OTP using Better-Auth | 2026-04-22 | OTP send/resend |
| #30 | Feat: 6-digit OTP validation | 2026-04-23 | Better Auth OTP verify |
| #31 | Refact: Migrate pre-wizard forms to Tanstack Form lib | 2026-04-27 | TanStack Form migration |

**Commit range:** 81 commits, `be9624fc` (2026-04-02) → `ff45dfef` (2026-04-24)

### Other repositories

> Copy this template (or add a section below) per repo/company.

- [x] `vintasoftware/building-blocks` — done (this file)
- [ ] _other repos_ — not started

---

## Changelog of this doc

| Date | Change |
| ---- | ------ |
| 2026-08-03 | Origin-specific draft created |
| 2026-08-04 | Generalized into repo-agnostic template |
| 2026-08-04 | Added ATS export and LinkedIn export sections (per-repo copy blocks) |
| 2026-08-04 | Filled / refreshed from commits/PRs for `vintasoftware/building-blocks` — identity @doduneves / eduardo.neves@vinta.com.br; 8 merged PRs; 81 commits; 8 feature clusters (regeneration `_2`) |
