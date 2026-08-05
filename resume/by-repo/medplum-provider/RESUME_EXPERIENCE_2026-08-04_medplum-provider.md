# Engineering highlights (CV source)

> **Purpose:** Personal draft material for resume / LinkedIn bullets.  
> **Scope:** One repository — Everself Medplum provider monorepo (`everself/medplum-provider`).  
> **Primary sources:** 302 commits by `Eduardo Neves` / `eduardo.neves@vinta.com.br` (2025-11-24 – 2026-03-31); PR attribution via merge-commit diff analysis (`gh pr list --author=@me` empty — org visibility).  
> **Status:** Filled 2026-08-04.

---

## Role / context

| Field | Value |
| ----- | ----- |
| Company / product | Everself — non-surgical weight-loss care provider platform (scheduling, patient chart, partner coordination, telephony) |
| Repository | `everself/medplum-provider` |
| Your role | Software Engineer (contract / Vinta) |
| Date range | 2025-11 – Present |
| Areas / surfaces you owned | Provider web app (`packages/provider-app`), Medplum bots (`packages/bots`), shared lib, FHIR access policies, Twilio call routing |
| GitHub username | @doduneves |
| Git author email(s) | `eduardo.neves@vinta.com.br` |

**One-line context (for CV header under the job):**  
Built and shipped FHIR-backed provider tooling for Everself's non-surgical weight-loss care platform — scheduling, patient/partner timelines, lab orders, telephony, and Healthie procedure-stage automation.

---

## Features built

> One feature ≈ one merged PR (or a small PR stack). Prefer outcome language over file lists.

### Feature: Scheduling page — timezone-aware calendar & filters

- **Summary:** Delivered a full scheduling experience with timezone selection, date picker, location/status/type filters, per-provider availability windows, role-based practitioner filtering, and day/week calendar views — replacing ad-hoc date handling with tested timezone utilities (dayjs).
- **Outcome bullets (CV-ready):**
  - Built timezone-aware scheduling UI with US-prioritized timezone picker, Sunday-first date picker, and accurate appointment popover display across time zones.
  - Implemented multi-dimensional calendar filters (location, appointment status including rescheduled/cancelled, type, show/hide availability) and per-provider availability rendering on day and week views.
  - Added role-based practitioner filtering and availability window merging so multi-provider calendars stay readable without overlapping background events.
  - Filtered patient treatment summaries to booked/fulfilled appointments only for clearer clinical context.
- **Scope:** React UI, react-big-calendar, FHIR Appointment/Slot/Schedule reads, Vitest
- **Surfaces:** provider app — SchedulingPage, AppointmentDetailPopover
- **PRs:** #894, #911, #913, #916, #920, #928, #932, #934, #944, #765
- **Notes:** Largest recent commit cluster (BAR-1420, BAR-1935, BAR-1948, BAR-2005, BAR-2011, BAR-2017); replaced moment.js with dayjs in appointment popover.

### Feature: Appointment status lifecycle & booking validation

- **Summary:** Shipped appointment status changes from the patient timeline, rescheduled-status handling with cancelationReason preservation, role-based edit/delete permissions, and overlap/participant-availability checks at booking time.
- **Outcome bullets (CV-ready):**
  - Enabled in-timeline appointment status updates with centralized status options, deduplication, and patch-based FHIR updates that keep the chart visible during edits.
  - Added rescheduled appointment workflow using cancelationReason (replacing reasonCode) with regression tests for status transitions.
  - Implemented booking-time overlap and participant availability validation for providers and patients, including IV-therapy edge cases.
  - Introduced SUPERVISING_PHYSICIAN role and role-scoped appointment edit/delete permissions.
- **Scope:** React hooks, FHIR Appointment patches, access policies, Vitest
- **Surfaces:** provider app — patient timeline, appointment builder, scheduling
- **PRs:** #836, #846, #849, #771, #880, #899
- **Notes:** BAR-1896, BAR-1916, BAR-1520, BAR-1946, BAR-1953.

### Feature: Patient procedure stages & Healthie sync bot

- **Summary:** Built the update-procedure-status Medplum bot and supporting services to derive bariatric procedure stage and payment state from Healthie appointments, surfaced states across patient timeline/faxing UI, and added an ops page to bulk-update patient payment states.
- **Outcome bullets (CV-ready):**
  - Authored update-procedure-status bot with HealthieService refactor for appointment filtering, procedure-stage logic, and payment-state retrieval.
  - Surfaced bariatric procedure stage and payment-state labels on patient timeline, guest selector, and faxing workflows.
  - Delivered admin UI to bulk-update patient payment states with error handling for undefined procedure stages and payment states.
  - Extended Healthie patient import to use MEDICAL_RECORD_ID identifiers and telecom.use on phone imports.
- **Scope:** Medplum bots, Healthie GraphQL integration, React UI, Vitest
- **Surfaces:** provider app, bots package, internal ops page
- **PRs:** #589, #775, #803, #808, #813
- **Notes:** BAR-1497; record counts unknown — needs estimate from bot dry-run logs.

### Feature: Labs write access control & restricted content

- **Summary:** Implemented role-gated lab order creation with FHIR access policies for Communication, ServiceRequest, and DiagnosticReport; integrated RestrictedContentManager for break-glass visibility of restricted lab results on timelines.
- **Outcome bullets (CV-ready):**
  - Added LABS_WRITE_ROLES, LabsWritePolicy, and canWriteLabs permission gating the lab order button and Labs section by role.
  - Enforced Restricted confidentiality on DiagnosticReport and lab-order Communication resources with timeline break-glass filtering.
  - Updated access policies with criteria/readonly attributes so lab reads and writes follow least-privilege across roles.
- **Scope:** FHIR access policies, React permission providers, timeline UI
- **Surfaces:** provider app — patient timeline, lab orders
- **PRs:** #710, #712, #754
- **Notes:** BAR-1525.

### Feature: Health Gorilla lab orders — error handling & rollback

- **Summary:** Hardened lab order creation against Health Gorilla API failures with user-facing error formatting, resource rollback before external submission, and contact-card UX improvements for patient identifiers.
- **Outcome bullets (CV-ready):**
  - Implemented rollback of lab order FHIR resources when Health Gorilla submission fails, with unit tests for success and failure paths.
  - Added JSON-aware Health Gorilla error formatting and consistent user-facing messages for lab order validation failures.
  - Enhanced patient identifier management (MEDICAL_RECORD_ID coding) and contact label editing with type-safe unified contact list components.
- **Scope:** React forms, FHIR ServiceRequest/DiagnosticReport, Vitest
- **Surfaces:** provider app — lab order form, contact cards
- **PRs:** #719, #732
- **Notes:** BAR-1530.

### Feature: Partner timeline platform

- **Summary:** Created the partner timeline from core page through shared timeline architecture — phone/SMS/calls for organizations, documents, filters, standalone notes, and a unified timeline context reused by patient and partner views.
- **Outcome bullets (CV-ready):**
  - Built partner timeline core with organization info header, editable partner metadata, documents panel, and optimized filter/search performance.
  - Extended telephony and messaging bots/UI to support organization recipients — phone list menus, caller identification, and partner-specific SMS success flows.
  - Refactored timeline into shared components, hooks, and services (TimelineItems, TimelineFilters, DocumentsCard, message box) supporting patient, partner, and optional care-team contexts.
  - Fixed partner timeline query limits and separated patient vs organization phone call routing.
- **Scope:** React UI, Twilio integration, Medplum bots, shared lib
- **Surfaces:** provider app — PartnersPage, PartnerTimelinePage
- **PRs:** #579, #567, #603, #610, #614, #616, #620, #634, #638, #649, #659, #648, #611
- **Notes:** BAR-1480, BAR-1483, BAR-1542; largest structural refactor in repo.

### Feature: Partner timeline access policies

- **Summary:** Added PartnerTimelinePolicy and permission-loading UX so partner timeline access is enforced by role with clear denial alerts for unauthorized users.
- **Outcome bullets (CV-ready):**
  - Authored PartnerTimelinePolicy with Communication criteria and DocumentReference access rules for partner-scoped timeline data.
  - Added permissions loading state to CurrentUserProvider and user-facing access-denial alerts on partner pages.
  - Expanded provider-policy-subscription tests to cover partner timeline access including Patient Coordinator role.
- **Scope:** FHIR access policies, React auth context, Vitest
- **Surfaces:** provider app — partner pages
- **PRs:** #689
- **Notes:** BAR-1584.

### Feature: Twilio patient call routing & Care Manager fallback

- **Summary:** Improved inbound patient call routing by raising PractitionerRole search limits and adding Care Manager fallback when primary routing targets are unavailable.
- **Outcome bullets (CV-ready):**
  - Increased PractitionerRole search limits to 10,000 across call-routing services so Care Manager pools are fully discoverable.
  - Added fallback routing to Care Managers in patient call handling when primary practitioner roles are unavailable.
  - Refactored TwilioPhoneProvider types and call-handling structure for clearer routing logic.
- **Scope:** Medplum bots, Twilio webhooks, FHIR PractitionerRole search
- **Surfaces:** bots — telephony routing
- **PRs:** #622, #888
- **Notes:** BAR-1552, BAR-1952.

### Feature: Encounter draft auto-save on timeline

- **Summary:** Added auto-save for encounter/chart drafts in the encounter timeline component so clinicians do not lose in-progress notes when navigating away.
- **Outcome bullets (CV-ready):**
  - Implemented draft auto-save on EncounterTimelineComponent with focus-based persistence for in-progress chart entries.
  - Fixed focus event handling (focusin vs focusout) to reliably trigger saves without premature draft loss.
- **Scope:** React timeline component, FHIR Encounter/Composition drafts
- **Surfaces:** provider app — patient timeline encounters
- **PRs:** #874
- **Notes:** BAR-1943.

### Feature: Related contacts, telecom & access policies

- **Summary:** Enhanced related-contact management with telecom use attributes, RelatedPerson access policies, and sidebar/layout improvements for patient documents and contacts.
- **Outcome bullets (CV-ready):**
  - Added RelatedPerson resource type to access policies and telecom.use handling for related contact phone/email fields.
  - Built related-contacts list and partner-sidebar document layout improvements for faster care-team navigation.
  - Fixed related-contact handling to exclude deleted organization partners and improved gone-resource error UX.
- **Scope:** FHIR RelatedPerson/Organization, React sidebar, access policies
- **Surfaces:** provider app — patient timeline sidebar, related contacts
- **PRs:** #565, #564, #542, #527, #858, #864, #679
- **Notes:** BAR-1924, BAR-1442.

### Feature: Global search & patient procedures UI

- **Summary:** Improved global search to surface bariatric procedure stage with updated layout and added a PatientProcedures component with appointment hooks for procedure-centric views.
- **Outcome bullets (CV-ready):**
  - Redesigned global search results to show procedure stage with improved alignment and readability.
  - Delivered PatientProcedures component and useAppointments hook for managing procedure-related appointments on patient charts.
- **Scope:** React UI, FHIR Appointment reads, search indexing
- **Surfaces:** provider app — global search, patient chart
- **PRs:** #821, #823, #819
- **Notes:** BAR-1846, BAR-1881.

### Feature: Medplum bot deployment tooling

- **Summary:** Added selective bot deployment and updated bundle generation so engineers can deploy individual bots without full-bundle churn.
- **Outcome bullets (CV-ready):**
  - Implemented bot selection for deployment with updated bundle generation for targeted Medplum bot releases.
  - Improved bots deployment limits and logging for safer CI/local deploy workflows.
- **Scope:** deploy scripts, esbuild bundle, Medplum CLI
- **Surfaces:** bots package — deploy tooling
- **PRs:** #886, #608
- **Notes:** Complements ongoing bot registry maintenance (#803).

---

## Tech used

> Group for the CV “tech stack” line; keep only what you actually used in this repo.

### Languages & runtime

- TypeScript, Node.js 22, ES modules

### Frontend

- React 19, Vite, Mantine 8, react-big-calendar, react-router 7, TipTap, dayjs, Vitest, Testing Library

### Backend / services

- Medplum bots (FHIR server), Express local bot server, Apollo GraphQL (Healthie), esbuild

### Data / storage

- FHIR R4 (Medplum) — Appointment, Schedule, Slot, PractitionerRole, Communication, ServiceRequest, DiagnosticReport, RelatedPerson, Organization, Encounter

### Integrations / third parties

- Medplum, Healthie, Health Gorilla, Twilio Voice/SMS, Customer.io, PostHog, Sentry, AWS (deploy SSO)

### Tooling & quality

- pnpm workspaces, Vitest, ESLint (@medplum/eslint-config), Husky/lint-staged, TypeScript strict checking

### Infra / delivery

- Medplum bot deploy scripts, Cloudflare Workers (wrangler/vitest-pool-workers), ngrok for local Twilio webhooks

**Short CV stack line (draft):**  
`TypeScript · React · Vite · Mantine · Medplum/FHIR · Vitest · Twilio · Healthie · Health Gorilla · pnpm monorepo`

---

## Migrations / one-off scripts

> Prefer scripts you authored or owned. Note safety patterns (dry-run by default, batching, backups, idempotency) when relevant.

| Script / migration | Purpose | Approx. records / users affected | Env (staging → prod) | PR / path | Notes |
| ------------------ | ------- | -------------------------------- | -------------------- | --------- | ----- |
| update-procedure-status bot | Derive bariatric procedure stage and payment state from Healthie appointments | unknown — needs estimate | staging signed off, then prod | `packages/bots/` — BAR-1497 | Triggered bot; impact depends on patient cohort synced from Healthie |
| deploy-bots (selective) | Deploy individual Medplum bots instead of full bundle | N/A (deploy tooling) | dev → staging → prod | `packages/bots/deploy/` — #886 | Reduces deploy risk for targeted bot changes |

**CV-ready bullets (draft):**

- Authored Healthie-driven procedure-status automation bot with Vitest coverage and defensive handling for undefined payment/procedure states.
- Improved Medplum bot deploy tooling with per-bot selection to shorten release cycles for telephony and integration bots.

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
| Users / customers / accounts affected | unknown — needs estimate | provider app user base not in repo |
| Entities touched (orders, records, jobs, …) | unknown — needs estimate | procedure-status bot run logs |
| Teams / tenants / environments in scope | Everself provider org (staging + prod) | repo deploy scripts |
| Error rate / support tickets reduced | _ | _ |
| Latency / cost / perf improvement | Partner timeline query-limit fixes reduced over-fetching | PR #634 |

**CV-ready impact lines:**

- Reduced partner timeline over-fetching by fixing request limits and optimizing document/filter queries (BAR-1483).
- _(Add patient/procedure counts from bot dry-run logs when available.)_

---

## Architecture / domain

> High-signal domain ownership — strong for senior-leaning bullets.

- **System shape:** pnpm monorepo — shared lib, Medplum bots package, Vite/React provider app; FHIR as system of record on Medplum
- **Domain focus:** Bariatric care operations — scheduling, patient/partner timelines, procedure-stage tracking, lab orders, inbound telephony
- **Cross-cutting concerns you owned:** FHIR access policies, role-based permissions, restricted clinical content (labs), timezone correctness, timeline context sharing
- **Your focus areas:** Provider-facing scheduling and timelines; Medplum bot integrations (Healthie, Twilio, Health Gorilla); access-control policies

**CV-ready bullets:**

- Owned shared timeline architecture reused across patient and partner contexts, consolidating filters, documents, telephony, and notes into common components and services.
- Implemented FHIR access policies and permission providers for labs, partner timelines, and related contacts — enforcing least-privilege in a clinical workflow app.

---

## Hard problems solved

> Depth markers: correctness under edge cases, scale, consistency, security, migrations, ambiguous product rules, etc.

| Problem | What was hard | Approach | Outcome |
| ------- | ------------- | -------- | ------- |
| Timezone-aware scheduling | Appointment display and day boundaries shift when users change timezone or cross DST | Centralized timezone utilities with dayjs, US-prioritized picker, timezone-aware date range calculations with unit tests | Accurate calendar and popover times across US timezones |
| Rescheduled appointment status | FHIR reasonCode vs cancelationReason inconsistency broke status transitions | Migrated to cancelationReason, centralized status options, patch-based updates with deduplication | Reliable in-timeline status changes without duplicate entries |
| Lab order external failure | Partial FHIR writes before Health Gorilla submission left orphaned resources | Rollback created resources on failure; JSON-aware error formatting for operators | Cleaner failure mode and test-covered rollback paths |
| Partner timeline query limits | Partner document/filter queries exceeded Medplum search limits | Optimized filters, fixed request limits, separated patient vs org phone routing | Partner timeline usable at scale without timeout errors |
| Call routing pool discovery | PractitionerRole searches capped below Care Manager pool size | Raised search limits to 10,000 and added Care Manager fallback routing | Inbound patient calls reach available staff more reliably |

**CV-ready bullets:**

- Solved timezone boundary bugs in multi-provider scheduling with tested dayjs utilities and timezone-aware filter/date-picker integration.
- Designed lab order failure rollback and restricted-content access policies so clinical data stays consistent and permission-scoped under edge-case failures.

---

## Quality & delivery

- Automated tests (unit / integration / e2e) for the paths you owned
- Feature flags or gradual rollout for risky changes
- Staging (or equivalent) before production
- Clear PR / stacked delivery when work was phased

**CV-ready bullets:**

- Added Vitest coverage for timezone utilities, appointment overlap validation, availability window merging, lab order rollback, and Health Gorilla error formatting.
- Delivered large timeline refactor (BAR-1542) in phased PRs — shared components first, then partner-specific features — to limit merge risk.
- Used staging build targets (`build-staging`) and Medplum bot local server + ngrok for Twilio webhook development before production deploys.

---

## Ownership & process

> Spec → plan → implement → review → ship (only claim what you did).

- Authored or drove: feature tickets (BAR-*) from scheduling through partner timeline and integrations
- Reviewed: others’ PRs in scheduling, timeline, and bots areas (inferred from merge-commit co-author patterns)
- Partnered with: product/clinical ops on procedure stages, lab workflows, and scheduling rules
- Led rollout: Medplum bot deploys via selective deployment tooling; access-policy changes shipped with policy subscription tests

**CV-ready bullets:**

- Drove multi-PR feature stacks for scheduling (BAR-1420/BAR-1935) and partner timeline (BAR-1483/BAR-1542) from implementation through merge.
- Partnered on clinical workflow rules — appointment overlap, rescheduled status, procedure-stage labels — encoding product requirements into FHIR-aware UI and bots.

---

## Cross-cutting work

> Often under-counted on resumes; call out if you owned them.

| Area | Examples you touched | CV-worthy? |
| ---- | -------------------- | ---------- |
| Auth / sessions / permissions | LabsWritePolicy, PartnerTimelinePolicy, appointment edit/delete by role, RelatedPerson policies | Y |
| Observability (logging, metrics, error tracking) | Sentry in provider-app; deploy logging improvements | Y |
| Notifications (email / SMS / push) | Twilio SMS for partners/patients, appointment notifications (touch) | Y |
| Payments / billing | Patient payment state from Healthie (procedure billing context) | Y |
| Background jobs / bots / workers | update-procedure-status, telephony routing bots, selective deploy | Y |
| Design system / shared UI | Mantine-based scheduling filters, unified timeline components | Y |
| Developer experience / CI / tooling | Bot selective deploy, wrangler/vitest updates, favicon/theming | Y |
| Security / compliance / privacy | Restricted lab content, break-glass, confidentiality levels on DiagnosticReport | Y |

**CV-ready bullets:**

- Implemented role-based access policies and restricted-content handling for lab orders and partner timelines in a HIPAA-sensitive clinical app.
- Improved developer workflow for Medplum bot deployments with selective deploy and enhanced logging.

---

## Polished resume bullets (draft)

> Internal draft — refine here, then copy into **ATS export** and **LinkedIn export** below.  
> Aim for action + scope + outcome (+ metric when real).

1. Built timezone-aware scheduling with multi-filter calendar views, per-provider availability, and role-based practitioner selection for a bariatric care provider platform on Medplum/FHIR.
2. Shipped appointment status lifecycle and booking validation — overlap checks, rescheduled-status handling, and role-scoped edit permissions — directly from the patient timeline.
3. Authored Healthie-integrated procedure-status automation (Medplum bot) and surfaced bariatric procedure/payment states across patient chart and faxing workflows.
4. Implemented labs write access control, restricted-content break-glass, and Health Gorilla lab order rollback with user-facing error handling and Vitest coverage.
5. Created the partner timeline platform — telephony/SMS for organizations, documents, filters, and a shared timeline architecture reused by patient and partner views.
6. Improved inbound Twilio call routing with expanded PractitionerRole discovery and Care Manager fallback for reliable patient phone coverage.

---

## ATS export (single role — copy to master CV)

> **Do not submit this file to an ATS.** Copy the block below into your consolidated CV under **Experience**.  
> Plain text only: no tables, PR numbers, repo paths, code, or markdown formatting in the filled export.

```
Everself | Software Engineer | Nov 2025 – Present | Remote

Built FHIR-backed provider tooling for bariatric care — scheduling, patient/partner timelines, lab orders, and telephony on a React/Medplum monorepo.

• Built timezone-aware scheduling with multi-filter calendar views, per-provider availability, and role-based practitioner selection on Medplum/FHIR.
• Shipped appointment status lifecycle and booking validation — overlap checks, rescheduled handling, and role-scoped permissions — from the patient timeline.
• Authored Healthie-integrated procedure-status automation and surfaced bariatric procedure/payment states across patient chart and operational workflows.
• Implemented labs access control, restricted-content handling, and Health Gorilla lab order rollback with tested error paths for clinical data safety.
• Delivered partner timeline platform with organization telephony, documents, and shared timeline components reused across patient and partner contexts.
• Improved inbound call routing with expanded practitioner discovery and Care Manager fallback for reliable phone coverage.

Skills: TypeScript, React, Vite, Mantine, Medplum, FHIR, Vitest, Twilio, Healthie, Health Gorilla, pnpm
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
**Company:** Everself  
**Dates:** Nov 2025 – Present  
**Location:** Remote

**Description:**

Full-stack engineer on Everself's Medplum-based provider platform for bariatric care — scheduling, clinical timelines, lab workflows, and telephony integrations.

• Built timezone-aware scheduling with multi-filter calendar views, per-provider availability, and role-based practitioner selection on Medplum/FHIR.  
• Shipped appointment status lifecycle and booking validation — overlap checks, rescheduled handling, and role-scoped permissions — directly from the patient timeline.  
• Authored Healthie-integrated procedure-status automation and surfaced bariatric procedure/payment states across patient chart and faxing workflows.  
• Implemented labs access control, restricted-content break-glass, and Health Gorilla lab order rollback with user-facing error handling.  
• Delivered partner timeline platform — organization telephony/SMS, documents, filters — with shared timeline architecture across patient and partner views.  
• Improved inbound Twilio call routing with expanded practitioner discovery and Care Manager fallback.

**Skills to tag (optional):** TypeScript, React, Medplum, FHIR, Health Tech, Twilio, Vitest, Mantine, GraphQL

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
git log --author='Eduardo Neves' --oneline --no-merges

# Merged PRs by you (needs gh auth; run inside the repo)
gh pr list --author=@me --state merged --limit 100

# Optional: commits touching migrations / operational scripts
git log --author='Eduardo Neves' --oneline -- scripts/ migrations/ db/ packages/bots/
```

### Key PRs / commit ranges

| PR | Title (branch) | Merged | Feature cluster |
| -- | -------------- | ------ | --------------- |
| #944 | BAR-2017 — treatment appointment filter | 2026-03 | Scheduling |
| #911 | BAR-1935 — calendar role filter & day view | 2026-03 | Scheduling |
| #894 | BAR-1420 — scheduling filters & timezone | 2026-02 | Scheduling |
| #886 | deploy-single-bot | 2026-02 | Bot deploy |
| #836 | BAR-1896 — appointment status on timeline | 2026-01 | Appointment lifecycle |
| #771 | BAR-1520 — booking overlap validation | 2026-01 | Appointment lifecycle |
| #813 | BAR-1497 — update patient states page | 2026-01 | Procedure stages |
| #589 | BAR-1497 — procedure-status bot | 2025-12 | Procedure stages |
| #712 | BAR-1525 — labs write access control | 2025-12 | Labs access |
| #719 | BAR-1530 — Health Gorilla requirements | 2025-12 | Lab orders |
| #638 | BAR-1542 — unified timeline context | 2025-11 | Partner timeline |
| #579 | BAR-1480 — partner timeline core | 2025-11 | Partner timeline |
| #689 | bar-1584 — partner timeline access | 2025-12 | Partner access |
| #888 | BAR-1952 — Care Manager call fallback | 2026-02 | Telephony |
| #874 | BAR-1943 — encounter draft auto-save | 2026-02 | Encounters |

### Other repositories

> Copy this template (or add a section below) per repo/company.

- [x] `everself/medplum-provider` — done (this file)
- [ ] _org/repo_ — _status: not started / in progress / done_

---

## Changelog of this doc

| Date | Change |
| ---- | ------ |
| 2026-08-03 | Origin-specific draft created |
| 2026-08-04 | Generalized into repo-agnostic template |
| 2026-08-04 | Added ATS export and LinkedIn export sections (per-repo copy blocks) |
| 2026-08-04 | Filled from commits/PRs for `everself/medplum-provider` |
