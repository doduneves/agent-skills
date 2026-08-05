# Engineering highlights (CV source)

> **Purpose:** Personal draft material for resume / LinkedIn bullets.  
> **Scope:** One repository — Origin Apps monorepo (`Origin-Therapy/origin-apps`).  
> **Primary sources:** 291 commits by `eduardo.neves@vinta.com.br` (2026-04-29 – 2026-07-28); PR attribution via merge-commit diff analysis (`gh pr list` / search empty — org visibility).  
> **Status:** Filled 2026-08-04 (regeneration `_2` with ATS / LinkedIn export blocks).

---

## Role / context

| Field | Value |
| ----- | ----- |
| Company / product | Origin Therapy — behavioral-health platform (patient onboarding, provider portal, ops portal) |
| Repository | `Origin-Therapy/origin-apps` |
| Your role | Software Engineer (contract / Vinta) |
| Date range | 2026-04 – Present |
| Areas / surfaces you owned | Provider portal, ops portal, shared packages (`packages/provider`, `packages/fhir-helpers`), FHIR deploy scripts, operational backfills |
| GitHub username | @doduneves |
| Git author email(s) | `eduardo.neves@vinta.com.br` |

**One-line context (for CV header under the job):**  
Built and shipped FHIR-backed features across a Next.js monorepo serving patient onboarding, provider charting/scheduling, and internal ops tooling on Medplum.

---

## Features built

> One feature ≈ one merged PR (or a small PR stack). Prefer outcome language over file lists.

### Feature: Ops system intake template management

- **Summary:** Delivered end-to-end ops tooling for system intake Questionnaires — API routes, lifecycle actions (draft/publish/retire/revert-to-root), preview, audit logging, and tag-driven discovery — so ops staff can manage discipline-specific intake forms without deploy scripts.
- **Outcome bullets (CV-ready):**
  - Built ops-portal intake template APIs and UI with draft/publish/retire lifecycle, preview, and audit trail for FHIR Questionnaires.
  - Unified intake template editing across disciplines and removed Form.io dependencies in favor of a native drag-and-drop canvas builder with follow-up question support.
  - Implemented revert-to-root and root-protection rules so production intake defaults stay safe during template iteration.
- **Scope:** API routes, ops UI, FHIR Questionnaire lifecycle, feature flags, unit tests
- **Surfaces:** ops portal, provider intake template editor
- **PRs:** #448, #449, #421, #454
- **Notes:** Largest commit clusters; #448 (11) + #449 (22) + #421 (21) author commits in feature PRs (staging bundle merges excluded).

### Feature: Ops system chart template lifecycle

- **Summary:** Shipped create/edit/publish flows for system chart note templates in the ops portal, including service-type scoping, questionnaire serialization, publish confirmation, and freeze-at-lock behavior for in-use templates.
- **Outcome bullets (CV-ready):**
  - Implemented full lifecycle management for system chart templates (create, edit, publish, retire) with service-type and discipline metadata.
  - Added questionnaire response freezing at chart lock so published template versions stay consistent with signed notes.
  - Enhanced drag-and-drop canvas builder for chart template authoring shared with provider-side editors.
- **Scope:** ops UI, FHIR Questionnaire/QuestionnaireResponse, NoteForm integration, deploy matching
- **Surfaces:** ops portal, provider chart notes
- **PRs:** #467, #498, #470, #499, #466
- **Notes:** #466 improved FHIR deploy to preserve retired root status and prefer URL-based root matching for patient-intake resources.

### Feature: Chart template editor — new field types & UX

- **Summary:** Extended the provider chart template editor with a redesigned palette/layout, multiselect and Likert rating fields, default values for text fields, and faithful questionnaire rendering parity between editor preview and runtime forms.
- **Outcome bullets (CV-ready):**
  - Redesigned chart template editor with drag-and-drop palette, inspector drawer, billing-group toggles, and discipline chips.
  - Shipped multiselect, integer, and 5-point Likert field types with HL7 itemControl extensions and expanded unit test coverage for repeating groups.
  - Added default-value seeding for string/text fields so chart notes pre-populate from template definitions.
- **Scope:** provider UI, shared questionnaire rendering, Vitest
- **Surfaces:** provider portal (templates, chart notes, intake)
- **PRs:** #330, #635, #636, #644, #317
- **Notes:** #636 also restored billing/insurance intake steps and added conditional rendering for medical forms.

### Feature: Provider licensed states & waitlist hard-lock

- **Summary:** Implemented licensed-state management on provider profiles (stored on PractitionerRole Location references), wired state-based waitlist filtering, and surfaced UX when providers had unset licenses — enabling compliance-aware matching.
- **Outcome bullets (CV-ready):**
  - Built licensed-states read/write on provider profile API and UI, persisting state locations on FHIR PractitionerRole while preserving ZIP service-area refs.
  - Added waitlist hard-lock: filter referral queue by provider licensed states with profile nudge when unset.
  - Covered normalization, concurrent location loading, and e2e for ZIP vs JDN location matching.
- **Scope:** FHIR PractitionerRole/Location, provider API, waitlist API + UI, seed script, e2e
- **Surfaces:** provider portal (profile, waitlist), matching queue
- **PRs:** #596
- **Notes:** 10 author commits in PR; foundational helpers in `packages/provider`.

### Feature: Provider patient caseload table

- **Summary:** Rebuilt the provider patients/caseload experience with dedicated API routes, search, pagination, episode/task integration, and organization-reference resolution for impersonation scenarios.
- **Outcome bullets (CV-ready):**
  - Delivered provider patient caseload API and refactored table UI with search, sorting, pagination, and appointment fetching improvements.
  - Resolved organization references for provider roles under impersonation so multi-tenant caseload data stays scoped correctly.
  - Rolled out behind a feature flag with unit tests for patient search.
- **Scope:** API routes, provider UI, FHIR EpisodeOfCare/Task/Appointment reads
- **Surfaces:** provider portal
- **PRs:** #403
- **Notes:** 20 author commits; replaced legacy caseload components.

### Feature: Provider deactivation & schedule lifecycle

- **Summary:** Added ops and API support to deactivate/reactivate providers, filter active/inactive roles across UI and APIs, reject bookings on inactive schedules, and optionally update Schedule resources during lifecycle transitions.
- **Outcome bullets (CV-ready):**
  - Implemented provider deactivate/reactivate API routes with validation, schedule side-effects, and booking rejection for inactive providers.
  - Added active/inactive filtering and status display across ops provider management and downstream APIs.
  - Expanded test coverage for schedule management during lifecycle transitions.
- **Scope:** API routes, ops UI, FHIR Schedule/PractitionerRole, Vitest
- **Surfaces:** ops portal, provider scheduling/matching
- **PRs:** #375
- **Notes:** 10 author commits.

### Feature: Multi-discipline system intake forms (OT / PT)

- **Summary:** Added Pediatric OT and PT intake Questionnaires, tag-driven discovery, and resolution logic so onboarding can serve discipline-specific system forms instead of a single generic intake.
- **Outcome bullets (CV-ready):**
  - Authored and integrated Pediatric Occupational Therapy and Physical Therapy intake questionnaires into the system forms pipeline.
  - Improved questionnaire resolution to support multiple concurrent system intake forms with phased rollout documentation.
  - Deployed FHIR questionnaire resources and wired patient onboarding to select the correct form by discipline/service type.
- **Scope:** FHIR Questionnaires, intake resolution, deploy scripts, ops discovery
- **Surfaces:** patient onboarding, ops portal
- **PRs:** #358, #363
- **Notes:** Core FHIR resources under `core-fhir-resources/`.

### Feature: Waitlist & matching queue UX

- **Summary:** Enriched provider waitlist with insurance, telehealth preference, and added-date columns/filters; surfaced patient concerns badges on patient summary and waitlist views.
- **Outcome bullets (CV-ready):**
  - Added insurance and telehealth filters/columns to provider waitlist and matching queue for faster triage.
  - Built patient concerns extraction from onboarding and grouped badge display on waitlist and patient summary.
  - Fixed multi-specialty waitlist filtering to prioritize therapy track correctly.
- **Scope:** provider UI, matching queue API, questionnaire answer parsing
- **Surfaces:** provider portal (waitlist, patient detail)
- **PRs:** #347, #343
- **Notes:** 5 + 7 author commits respectively.

### Feature: Sentry observability with PHI scrubbing

- **Summary:** Integrated Sentry across Next.js server/edge/client with centralized instrumentation, circular-reference-safe scrubbing, and email redaction suitable for a healthcare app.
- **Outcome bullets (CV-ready):**
  - Integrated Sentry for error tracking and performance monitoring across Next.js runtime surfaces.
  - Implemented PHI-aware value scrubbing (email redaction, circular reference handling) in beforeSend hooks and shared logging utilities.
  - Centralized instrumentation handlers to keep error capture consistent app-wide.
- **Scope:** Sentry config, logging utilities, instrumentation
- **Surfaces:** all web app surfaces
- **PRs:** #339
- **Notes:** 7 author commits.

### Feature: Operational backfill scripts (tenancy & matching hygiene)

- **Summary:** Authored and hardened multiple dry-run-by-default backfill scripts for practitioner org accounts, withdrawn-patient unenrollment, and schedule compartment stamps — with production confirmation guards and stats tracking.
- **Outcome bullets (CV-ready):**
  - Built idempotent backfill scripts for Practitioner `meta.accounts`, Schedule org compartments, and withdrawn-patient org unenrollment with dry-run defaults and batching.
  - Added targeted-mode unenrollment with stats tracking, pagination, and production confirmation prompts for safe staging→prod rollout.
  - Extended seed/deploy scripts to preserve organization account linkage when mutating provider locations.
- **Scope:** `scripts/migrations/`, shared script helpers, FHIR batch writes
- **Surfaces:** internal ops tooling (CLI)
- **PRs:** #292, #324, #329, #394, #291, #303, #301
- **Notes:** Record counts unknown — needs estimate from dry-run logs.

### Feature: Structured scheduling offers on patient matching acceptance

- **Summary:** When providers accept a matching case, structured scheduling time-slot offers are attached to the workflow so ops/scheduling has concrete availability windows to act on.
- **Outcome bullets (CV-ready):**
  - Added structured scheduling offers to the provider acceptance flow in patient matching.
  - Updated matching implementation plan and task output formatting for scheduling rollout.
- **Scope:** matching workflow, Task/FHIR writes
- **Surfaces:** provider matching, ops matching
- **PRs:** #392
- **Notes:** 2 author commits; smaller scoped feature.

### Feature: Appointment wall-clock timezone primitives (in progress)

- **Summary:** Started phased implementation of wall-clock scheduling timezone support — IANA timezone guards, scheduling primitives, provider time facade, and test updates — behind `NEXT_PUBLIC_ENABLE_APPOINTMENT_WALL_CLOCK`.
- **Outcome bullets (CV-ready):**
  - Designed and implemented initial wall-clock scheduling primitives with IANA timezone validation and shared time facade exports.
  - Authored phased implementation plan for appointment timezone correctness across scheduler surfaces.
- **Scope:** shared timezone utilities, provider time facade, feature flag, unit tests
- **Surfaces:** provider scheduling (planned rollout)
- **PRs:** _Phase 0 — pending merge (commits on feature branch)_
- **Notes:** Commits in Jul 2026; not yet attributed to a merged feature PR in this worktree. Prefer omitting from ATS until merged.

<!-- Duplicate the block above per feature. -->

---

## Tech used

> Group for the CV “tech stack” line; keep only what you actually used in this repo.

### Languages & runtime

- TypeScript
- Node.js (Next.js App Router, tsx operational scripts)

### Frontend

- Next.js (App Router), React
- Tailwind CSS, shadcn/ui, Mantine (modals)
- Drag-and-drop canvas builders (custom + shared template editor shell)

### Backend / services

- Next.js route handlers & server actions
- Medplum (`@medplum/core` / `@medplum/react`)
- Medplum bots present in monorepo — not primary focus of authored commits

### Data / storage

- Medplum FHIR (Questionnaire, QuestionnaireResponse, Patient, PractitionerRole, Schedule, Appointment, EpisodeOfCare, Task, Location, Organization)
- No traditional app database — reads/writes via MedplumClient

### Integrations / third parties

- Sentry (`@sentry/nextjs`)
- Stripe, Candid Health, RingCentral (present in monorepo; primary work was portal/FHIR)

### Tooling & quality

- pnpm workspaces + Turborepo
- Vitest + Testing Library
- Playwright e2e
- ESLint, Prettier

### Infra / delivery

- Vercel (web deploy)
- FHIR resource deploy via `tsx` scripts (`DOTENV_CONFIG_PATH` per environment)
- Feature flags (`NEXT_PUBLIC_ENABLE_*`)

**Short CV stack line (draft):**  
`TypeScript · Next.js · React · Medplum/FHIR · Tailwind/shadcn · Vitest · Playwright · Sentry · Turborepo · Vercel`

---

## Migrations / one-off scripts

> Prefer scripts you authored or owned. Note safety patterns (dry-run by default, batching, backups, idempotency) when relevant.

| Script / migration | Purpose | Approx. records / users affected | Env (staging → prod) | PR / path | Notes |
| ------------------ | ------- | -------------------------------- | -------------------- | --------- | ----- |
| `backfill-practitioner-organization-accounts` | Normalize `Practitioner.meta.accounts` from ProjectMembership org links | unknown — needs estimate | staging signed off, then prod | #292, `scripts/migrations/` | dry-run default, `--commit`, optional `--max`, force flag |
| `backfill-withdraw-unenroll` | Unenroll withdrawn patients from org compartments; complete matching cleanup | unknown — needs estimate | staging → prod | #324, #329, `scripts/migrations/` | targeted mode, stats tracking, production confirmation |
| `backfill-schedule-org-accounts` | Correct `Schedule.meta.accounts` for provider schedules | unknown — needs estimate | staging → prod | #394 | batch org resolution, performance optimizations |
| `inventory-matching-hybrid-withdrawn` (dry-run) | Identify hybrid withdrawn matching cases | unknown — needs estimate | staging analysis | commit `2d899c99` | dry-run identification script |
| `seed-providers` (licensed states) | Preserve JDN location refs when writing ZIP service areas | unknown — needs estimate | dev/staging seeding | #596, `scripts/` | idempotent seed updates |
| FHIR deploy root matching | Prefer URL-based root selection; preserve retired status on deploy | all intake/chart questionnaire deploys | staging → prod | #466, deploy scripts | deploy-time, not row backfill |

**CV-ready bullets (draft):**

- Authored dry-run-by-default FHIR backfill scripts for practitioner tenancy, schedule compartments, and withdrawn-patient unenrollment with production confirmation guards and batch processing.
- Hardened operational scripts with pagination, stats tracking, and targeted modes for safe staging-first rollout in a multi-tenant healthcare data model.

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
| Users / customers / accounts affected | unknown — needs estimate | backfill dry-run logs |
| Entities touched (orders, records, jobs, …) | unknown — needs estimate | FHIR backfill scripts |
| Teams / tenants / environments in scope | Multi-tenant via Organization compartments; staging + prod deploy paths | repo architecture |
| Error rate / support tickets reduced | _not measured in commits_ | Sentry integration enables tracking |
| Latency / cost / perf improvement | _not measured in commits_ | schedule backfill perf refactor |

**CV-ready impact lines:**

- Enabled ops self-service for intake and chart template changes that previously required engineer-led FHIR deploys (qualitative — no deploy-frequency metric in git).
- Reduced risk of cross-tenant data exposure by backfilling `meta.accounts` on Practitioner and Schedule resources (qualitative — counts need dry-run logs).

---

## Architecture / domain

> High-signal domain ownership — strong for senior-leaning bullets.

- **System shape:** Turborepo monorepo — Next.js web app + shared packages + Medplum bots + FHIR definitional resources; no app DB.
- **Domain focus:** Provider portal (charting, templates, waitlist, caseload), ops portal (system templates), patient onboarding intake, matching/scheduling, multi-tenant FHIR modeling.
- **Cross-cutting concerns you owned:** Medplum compartment tenancy (`meta.accounts`), FHIR Questionnaire lifecycle, template editor shared shell, operational backfills, PHI-safe observability.
- **Your focus areas:** Template authoring (intake + chart), ops template management, provider profile/waitlist compliance (licensed states), caseload API, provider lifecycle, data backfills.

**CV-ready bullets:**

- Shipped features on a FHIR-first architecture where Questionnaires drive patient intake and provider chart notes, with ops tooling for lifecycle management instead of ad-hoc extensions.
- Owned multi-tenant correctness patterns — organization compartments on FHIR writes, impersonation-safe org resolution, and backfills to repair historical `meta.accounts` gaps.

---

## Hard problems solved

> Depth markers: correctness under edge cases, scale, consistency, security, migrations, ambiguous product rules, etc.

| Problem | What was hard | Approach | Outcome |
| ------- | ------------- | -------- | ------- |
| Intake template lifecycle safety | Retiring or editing production intake defaults could break in-flight patient onboarding | Root-protection rules, revert-to-root, audit log, preview-before-publish, deploy URL-prefer-root matching | Ops can iterate templates without losing canonical roots (#449, #466) |
| Chart template version drift | Signed notes must reflect the template version used at lock time | Questionnaire response freezing at chart lock + serialization on publish | Consistent read-only note rendering (#499) |
| Licensed states vs ZIP service areas | PractitionerRole Locations serve dual purpose (ZIP areas + state licenses) | JDN location refs for states, preserve non-JDN refs on write, concurrent reads | Waitlist hard-lock without clobbering service-area data (#596) |
| Withdrawn patient org cleanup | Withdrawn matching cases left patients enrolled in org compartments | Idempotent backfill with targeted mode, stats, re-enrollment checks | Safer tenancy hygiene script (#324, #329) |
| PHI in error telemetry | Healthcare app must not leak patient data to Sentry | Centralized scrubbers, email redaction, circular-ref safe serialization | Sentry integrated with scrubbing hooks (#339) |
| Form.io removal | Legacy intake editor depended on Form.io | Native canvas builder with follow-up questions, drag-and-drop, shared editor shell | Form.io removed; unified builder (#421) |

**CV-ready bullets:**

- Solved template lifecycle correctness for production FHIR Questionnaires — root protection, revert-to-root, and deploy matching so intake defaults stay stable under ops iteration.
- Untangled dual-use PractitionerRole Location data (ZIP service areas vs licensed states) to enable compliance-aware waitlist filtering without data loss.

---

## Quality & delivery

- Automated tests (unit / integration / e2e) for the paths you owned
- Feature flags or gradual rollout for risky changes
- Staging (or equivalent) before production
- Clear PR / stacked delivery when work was phased

**CV-ready bullets:**

- Added Vitest coverage across template editors, licensed-states helpers, waitlist filtering, patient search, provider lifecycle, and questionnaire rendering paths.
- Gated patient caseload split and ops templates behind feature flags; used dry-run-by-default backfills with staging-first rollout for tenancy migrations.
- Delivered large features as stacked PRs (ops intake API → UI → editor unification; chart template create → lifecycle → freeze-at-lock).

---

## Ownership & process

> Spec → plan → implement → review → ship (only claim what you did).

- Authored or drove: implementation plans in-repo (appointment wall-clock timezone, ops system chart templates, provider deactivation, system intake behavior); phased plan docs committed with features.
- Reviewed: _not evidenced in git log — omit from CV unless you have separate data_
- Partnered with: product/ops on template lifecycle rules, matching waitlist requirements (inferred from domain)
- Led rollout: feature flags for caseload/templates; backfill scripts with `--commit` after staging dry-run

**CV-ready bullets:**

- Drove phased delivery for ops template management and appointment timezone work — plan docs, feature flags, and incremental PR stacks.
- Established operational safety patterns for FHIR backfills (dry-run default, batch limits, production confirmation) reused across practitioner, schedule, and withdraw scripts.

---

## Cross-cutting work

> Often under-counted on resumes; call out if you owned them.

| Area | Examples you touched | CV-worthy? |
| ---- | -------------------- | ---------- |
| Auth / sessions / permissions | FHIR AccessPolicy deploy resource (#309), compartment `meta.accounts` | Y |
| Observability (logging, metrics, error tracking) | Sentry integration + PHI scrubbing (#339) | Y |
| Notifications (email / SMS / push) | _not primary in your commits_ | N |
| Payments / billing | Billing group toggles in chart templates; restored billing/insurance intake steps (#636) | Y (peripheral) |
| Background jobs / bots / workers | Medplum bots present; your work focused on scripts not bot handlers | N |
| Design system / shared UI | Shared template editor shell, InspectorDrawer, canvas builder, shadcn/ui | Y |
| Developer experience / CI / tooling | PR template (#275); feature-flag env wiring | Y (minor) |
| Security / compliance / privacy | PHI scrubbing, licensed-state compliance, tenancy backfills | Y |

**CV-ready bullets:**

- Integrated Sentry with healthcare-appropriate PHI scrubbing across Next.js server, edge, and client runtimes.
- Contributed to multi-tenant security hygiene via `meta.accounts` backfills and AccessPolicy deploy artifacts.

---

## Polished resume bullets (draft)

> Internal draft — refine here, then copy into **ATS export** and **LinkedIn export** below.  
> Aim for action + scope + outcome (+ metric when real).

1. Built ops-portal tooling for FHIR intake and chart template lifecycle (create, publish, retire, preview, audit) so operations teams can manage clinical forms without engineer-led deploys.
2. Redesigned provider chart and intake template editors with drag-and-drop authoring, multiselect/Likert field types, follow-up questions, and default-value seeding — replacing legacy Form.io dependencies.
3. Implemented provider licensed-state management and waitlist hard-lock filtering on Medplum PractitionerRole resources, enabling compliance-aware patient matching across state lines.
4. Delivered provider patient caseload API and UI with search, pagination, and impersonation-safe organization scoping in a multi-tenant FHIR backend.
5. Authored dry-run-by-default operational backfills for practitioner tenancy, schedule compartments, and withdrawn-patient unenrollment with production confirmation guards and batch processing.
6. Integrated Sentry observability with PHI-aware scrubbing (email redaction, circular-reference handling) across a Next.js healthcare monorepo.

---

## ATS export (single role — copy to master CV)

> **Do not submit this file to an ATS.** Copy the block below into your consolidated CV under **Experience**.  
> Plain text only: no tables, PR numbers, repo paths, code, or markdown formatting in the filled export.

```
Origin Therapy | Software Engineer | Apr 2026 – Present

Built and shipped FHIR-backed features across a Next.js monorepo for patient onboarding, provider charting/scheduling, and ops tooling on Medplum.

• Built ops-portal tooling for FHIR intake and chart template lifecycle (create, publish, retire, preview, audit) so operations can manage clinical forms without engineer-led deploys.
• Redesigned provider chart and intake template editors with drag-and-drop authoring, multiselect/Likert fields, follow-up questions, and default-value seeding, replacing legacy Form.io.
• Implemented provider licensed-state management and waitlist filtering on Medplum PractitionerRole resources for compliance-aware patient matching.
• Delivered provider patient caseload APIs and UI with search, pagination, and impersonation-safe organization scoping in a multi-tenant FHIR backend.
• Authored dry-run-by-default FHIR backfills for practitioner tenancy, schedule compartments, and withdrawn-patient unenrollment with production confirmation guards.
• Integrated Sentry with PHI-aware scrubbing across Next.js server, edge, and client runtimes.

Skills: TypeScript, Next.js, React, Medplum, FHIR, Tailwind CSS, Vitest, Playwright, Sentry, Turborepo, Vercel
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
**Company:** Origin Therapy  
**Dates:** Apr 2026 – Present  
**Location:** Remote

**Description:**

Built and shipped FHIR-backed product features across Origin’s Next.js monorepo — patient onboarding, provider portal, and ops tooling on Medplum — with a focus on clinical form authoring, matching/waitlist workflows, and multi-tenant data hygiene.

• Built ops self-service for FHIR intake and chart template lifecycle (create, publish, retire, preview, audit) so clinical forms no longer require engineer-led deploys.  
• Redesigned provider chart/intake template editors with drag-and-drop authoring, new field types (multiselect, Likert), follow-up questions, and Form.io removal.  
• Implemented licensed-state management and waitlist hard-lock filtering for compliance-aware provider–patient matching.  
• Delivered provider caseload APIs/UI with search, pagination, and impersonation-safe organization scoping.  
• Authored safe FHIR backfill tooling (dry-run defaults, batching, production confirmation) for tenancy and withdrawn-patient cleanup.  
• Integrated Sentry observability with PHI-aware scrubbing suitable for a healthcare application.

**Skills to tag (optional):** TypeScript, Next.js, React, Medplum, FHIR, Tailwind CSS, Vitest, Playwright, Sentry, Turborepo

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

# Merged PRs by you (needs gh auth; org repo may require membership)
gh pr list --author=doduneves --state merged --limit 100

# PR attribution when gh search blocked — commits introduced per merge:
git log --grep='Merge pull request' --format='%H %s' --all | while read hash rest; do
  count=$(git log "${hash}^2" --not "${hash}^1" --author='eduardo.neves@vinta.com.br' --oneline --no-merges 2>/dev/null | wc -l)
  [ "$count" -gt 0 ] && echo "$count|$rest"
done | sort -t'|' -k1 -rn

# Optional: commits touching migrations / operational scripts
git log --author='eduardo.neves@vinta.com.br' --oneline -- scripts/
```

### Key PRs / commit ranges

| PR | Title (from merge message) | Author commits in PR | Feature cluster |
| -- | ----- | ------ | --------------- |
| #448 | feat/ops-intake-api | 11 | Ops intake API + lifecycle |
| #449 | feat/ops-intake-ui | 22 | Ops intake UI + preview/revert |
| #421 | feat/unify-intake-edit-template | 21 | Intake editor / Form.io removal |
| #454 | feat/provider-intake-medplum-parity | 16 | Intake medplum parity |
| #498 | feat/ops-chart-create | 6 | Ops chart template create |
| #470 | feat/ops-charts-ui | 4 | Ops chart template UI |
| #499 | feat/chart-freeze-at-lock | 8 | Chart freeze at lock |
| #467 | feat/ops-chart-lifecycle | 3 | Chart lifecycle plan + structure |
| #466 | feat/update-deploy-questionnaire-for-only-root-intakes | 4 | FHIR deploy root matching |
| #596 | feat/provider-licensed-states | 10 | Licensed states + waitlist |
| #403 | feat/split-patients-table | 20 | Provider caseload |
| #375 | feat/ops-can-deactivate-providers | 10 | Provider lifecycle |
| #358 | feat/add-new-intake-forms | 9 | OT/PT intake forms |
| #363 | feat/add-new-intake-forms-pt | 3 | PT intake forms |
| #347 | feat/adding-new-waitlist-columns | 5 | Waitlist columns/filters |
| #343 | feat/show-concerns-on-patient-list | 7 | Patient concerns badges |
| #339 | chore/sentry-setup | 7 | Sentry + PHI scrubbing |
| #330 | feat/new-edit-template-layout | 15 | Chart editor redesign |
| #635 | feat/chart-template-number-multiselect | 18 | Multiselect + rendering |
| #636 | feat/chart-template-rating | 5 | Likert + intake fixes |
| #644 | feat/template-text-default-values | 7 | Default values |
| #324 | chore/backfill-withdraw-unenroll | 11 | Withdraw unenroll backfill |
| #329 | feat/withdraw-unenroll-script-with-targeted-patient | 3 | Targeted unenroll |
| #292 | chore/backfill-practitioner-organization-accounts | 11 | Practitioner accounts backfill |
| #394 | chore/backfill-schedule-org-accounts | 4 | Schedule accounts backfill |
| #392 | feat/structured-time-slots-on-patient-matching-2 | 2 | Matching scheduling offers |

**Note:** `gh pr list --author=doduneves` and `gh search prs` returned empty / permission errors for this org. PR attribution derived from local merge-commit analysis (feature PRs above; staging/`dev_w*` bundle merges excluded from feature clusters).

### Other repositories

- [ ] _other org/repo_ — not started

---

## Changelog of this doc

| Date | Change |
| ---- | ------ |
| 2026-08-03 | Origin-specific draft created |
| 2026-08-04 | Generalized into repo-agnostic template |
| 2026-08-04 | Added ATS export and LinkedIn export sections (per-repo copy blocks) |
| 2026-08-04 | Filled from commits/PRs for `Origin-Therapy/origin-apps` — identity @doduneves / eduardo.neves@vinta.com.br; 291 commits; 12 feature clusters (regeneration `_2`) |
