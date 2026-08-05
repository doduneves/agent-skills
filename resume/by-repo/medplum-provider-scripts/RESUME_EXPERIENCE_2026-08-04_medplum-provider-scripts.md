# Engineering highlights (CV source)

> **Purpose:** Personal draft material for resume / LinkedIn bullets.  
> **Scope:** One repository — Everself operational scripts (`everself/medplum-provider-scripts`).  
> **Primary sources:** 24 commits by `Eduardo Neves` / `eduardo.neves@vinta.com.br` (2026-02-10 – 2026-03-30); PR attribution via merge-commit diff analysis (`gh pr list --author=@me` empty — org visibility).  
> **Status:** Filled 2026-08-04.

---

## Role / context

| Field | Value |
| ----- | ----- |
| Company / product | Everself — Medplum-based provider platform; operational data-fix and backfill scripts |
| Repository | `everself/medplum-provider-scripts` |
| Your role | Software Engineer (contract / Vinta) |
| Date range | 2026-02 – 2026-03 |
| Areas / surfaces you owned | Timestamped one-off scripts against Medplum staging/prod — patient identifiers, practitioner NPI, procedure stages, appointment status reconciliation |
| GitHub username | @doduneves |
| Git author email(s) | `eduardo.neves@vinta.com.br` |

**One-line context (for CV header under the job):**  
Authored production-safe Medplum/FHIR backfill and reconciliation scripts for bariatric care data hygiene — dry-run defaults, cursor pagination, rate limiting, and audit logging.

---

## Features built

> One feature ≈ one merged PR (or a small PR stack). Prefer outcome language over file lists.

### Feature: Patient Medical Record ID & telecom backfill

- **Summary:** Built a cursor-paginated backfill to stamp missing Medical Record ID identifiers on Patient resources, normalize identifier type coding, and fix telecom `use` values — with dry-run default, resume checkpoints, and Medplum API rate protection for staging→prod rollout.
- **Outcome bullets (CV-ready):**
  - Authored patient Medical Record ID backfill with HL7 MR type coding, telecom.use validation, and single-patient targeting for safe spot fixes.
  - Implemented cursor pagination, batch processing, progress persistence, and RPS throttling with retry/backoff to run safely against Medplum prod APIs.
  - Added execution-time logging and edge-case batch logging so ops can audit pages processed, skips, and updates without logging PHI.
- **Scope:** TypeScript CLI, Medplum FHIR Patient patches, progress files
- **Surfaces:** internal ops tooling (CLI scripts)
- **PRs:** #9
- **Notes:** BAR-1530; `scripts/20260209-backfill-patient-medical-record-id.ts`; pairs with Health Gorilla lab-order requirements in main provider app.

### Feature: Practitioner NPI bulk update

- **Summary:** Delivered a practitioner NPI update script sourced from a curated JSON list (migrated from CSV), with dry-run/force modes, page delays, and the same rate-limit/retry/progress patterns as patient backfills.
- **Outcome bullets (CV-ready):**
  - Built practitioner NPI update script with dry-run default, force-update option, and JSON-based practitioner lookup replacing deprecated CSV data source.
  - Reused shared production-safe patterns — progress tracking, page size/delay config, RPS limits, and execution-time logging — for repeatable staging validation before prod.
- **Scope:** TypeScript CLI, Medplum FHIR Practitioner patches
- **Surfaces:** internal ops tooling (CLI scripts)
- **PRs:** #9
- **Notes:** BAR-1530; `scripts/20260209-update-practitioner-npi.ts`; shipped in same PR stack as patient backfill.

### Feature: Bulk procedure stage update via Medplum bot

- **Summary:** Created a script to invoke the update-procedure-status Medplum bot across the patient cohort — with bot concurrency caps, extension cleanup before execution, cursor pagination, and dry-run default — aligning bulk ops with the Everself API payment-state workflow.
- **Outcome bullets (CV-ready):**
  - Authored bulk procedure-stage update script that invokes update-procedure-status bot per patient with 20-concurrent-bot cap and cursor-paginated cohort processing.
  - Enhanced bot pre-processing by stripping bariatric-procedure-stage extension before execution so bot logic recalculates stage cleanly from Healthie data.
  - Standardized script integration on Everself API env configuration, replacing legacy env variable references, and streamlined patient processing utilities.
- **Scope:** TypeScript CLI, Medplum bot execution, `@everself/lib` extensions
- **Surfaces:** internal ops tooling (CLI scripts)
- **PRs:** #38, #42
- **Notes:** BAR-1497; `scripts/20260305-update-procedure-states.ts`; record counts unknown — needs estimate from dry-run logs.

### Feature: Healthie appointment status reconciliation

- **Summary:** Built a reconciliation script that finds Appointments whose Medplum status diverges from original Healthie `pm_status` metadata, updates statuses (including rescheduled cancelationReason), and emits CSV audit rows in live mode.
- **Outcome bullets (CV-ready):**
  - Implemented Healthie-to-Medplum appointment status reconciliation comparing stored `data-from-healthie` pm_status against current FHIR Appointment.status.
  - Handled rescheduled mappings with SNOMED cancelationReason coding and CSV audit output for staging review before prod execution.
  - Applied Medplum API throttling, retry logic, and dry-run default (explicitly enforced) for HIPAA-conscious operational runs.
- **Scope:** TypeScript CLI, Medplum FHIR Appointment patches, CSV audit trail
- **Surfaces:** internal ops tooling (CLI scripts)
- **PRs:** _(branch BAR-2017 — merge pending or direct to main; author commits on branch)_
- **Notes:** BAR-2017; `scripts/20260330-update-healthie-status-mismatches.ts`; appointment count unknown — needs estimate from dry-run CSV.

---

## Tech used

> Group for the CV “tech stack” line; keep only what you actually used in this repo.

### Languages & runtime

- TypeScript, Node.js, ts-node / tsx

### Frontend

- _(none — CLI/ops scripts repo)_

### Backend / services

- Medplum Client SDK, `@everself/lib` (constants/extensions)

### Data / storage

- FHIR R4 via Medplum — Patient, Practitioner, Appointment; HL7 identifier type coding; Healthie migration extensions

### Integrations / third parties

- Medplum API, Healthie metadata (`data-from-healthie` extension), Everself API (procedure/payment state bot secrets)

### Tooling & quality

- dotenv, progress/checkpoint JSON files, CSV audit output, peer-review + Linear log workflow per repo conventions

### Infra / delivery

- Local CLI execution against staging then prod Medplum environments; timestamped script naming convention (`YYYYMMDD-description.ts`)

**Short CV stack line (draft):**  
`TypeScript · Medplum/FHIR · Node.js · Healthie · CLI backfills · Rate limiting · Dry-run ops`

---

## Migrations / one-off scripts

> Prefer scripts you authored or owned. Note safety patterns (dry-run by default, batching, backups, idempotency) when relevant.

| Script / migration | Purpose | Approx. records / users affected | Env (staging → prod) | PR / path | Notes |
| ------------------ | ------- | -------------------------------- | -------------------- | --------- | ----- |
| `20260209-backfill-patient-medical-record-id.ts` | Fill missing MR identifiers, normalize type coding, fix telecom.use | unknown — needs estimate | staging signed off, then prod | #9 / `scripts/` | dry-run default `--dry=false`; cursor resume; `--patientId` for single fix |
| `20260209-update-practitioner-npi.ts` | Update Practitioner NPI from curated JSON list | unknown — needs estimate | staging → prod | #9 / `scripts/` | dry-run default; `--force=true` for overwrite |
| `20260305-update-procedure-states.ts` | Bulk-invoke update-procedure-status bot per patient | unknown — needs estimate | staging → prod | #38, #42 / `scripts/` | BOT_CONCURRENCY=20; strips extension before bot; dry-run default |
| `20260330-update-healthie-status-mismatches.ts` | Reconcile Appointment.status vs Healthie pm_status | unknown — needs estimate | staging → prod | BAR-2017 / `scripts/` | CSV audit in live mode; target-date filter; dry-run default |

**CV-ready bullets (draft):**

- Built dry-run-by-default Medplum backfill scripts with cursor pagination, RPS throttling, retry/backoff, and resume checkpoints for safe staging→prod data fixes.
- Orchestrated bulk Medplum bot execution for bariatric procedure-stage updates with concurrency limits and pre-bot extension cleanup.

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
| Users / customers / accounts affected | unknown — needs estimate | patient/practitioner cohort sizes from dry-run logs |
| Entities touched (orders, records, jobs, …) | unknown — needs estimate | script progress JSON / CSV audit files |
| Teams / tenants / environments in scope | Everself Medplum staging + prod | script headers |
| Error rate / support tickets reduced | _ | _ |
| Latency / cost / perf improvement | API RPS throttling (5 req/s default) reduces prod API overload risk | script constants |

**CV-ready impact lines:**

- Enabled ops to resume interrupted backfills from cursor checkpoints without reprocessing entire patient cohorts.
- _(Add patient/appointment counts from dry-run progress files when available.)_

---

## Architecture / domain

> High-signal domain ownership — strong for senior-leaning bullets.

- **System shape:** Lightweight scripts repo separate from main provider app — timestamped one-offs for auditability; promotes reusable logic back to main codebase when warranted
- **Domain focus:** FHIR data hygiene for bariatric provider platform — patient identifiers, practitioner NPI, procedure stages, Healthie migration fidelity
- **Cross-cutting concerns you owned:** Production safety (dry-run, staging-first), HIPAA-conscious logging (IDs/counts not PHI), idempotency/resume, Medplum API rate limits
- **Your focus areas:** Authoring and hardening operational backfill/reconciliation scripts end-to-end

**CV-ready bullets:**

- Established repeatable production-safe script patterns (dry-run default, cursor pagination, progress files, RPS limits) reused across patient, practitioner, and appointment reconciliation tooling.
- Owned FHIR identifier and telecom normalization backfills supporting Health Gorilla lab-order integration requirements in the main provider platform.

---

## Hard problems solved

> Depth markers: correctness under edge cases, scale, consistency, security, migrations, ambiguous product rules, etc.

| Problem | What was hard | Approach | Outcome |
| ------- | ------------- | -------- | ------- |
| Large patient cohort backfills | Medplum API rate limits and mid-run failures lose progress | Cursor pagination, progress JSON checkpoints, RPS throttle + retry, page delays | Resumable backfills without re-scanning from page 1 |
| Bot bulk execution | Unbounded bot calls can overwhelm Medplum | 20-concurrent cap, strip stale extension before invoke, dry-run mode | Controlled procedure-stage refresh across cohort |
| Healthie status drift | Migrated appointments retain Healthie pm_status in extension but Medplum status may diverge | Compare extension vs status, map rescheduled with cancelationReason, CSV audit | Targeted reconciliation with reviewable change log |
| Telecom/identifier correctness | Missing MR type coding or telecom.use breaks downstream integrations | Validate and patch in same backfill pass with single-patient override | One script covers identifier + telecom hygiene |

**CV-ready bullets:**

- Designed resumable Medplum backfills with cursor checkpoints and API throttling so prod data fixes survive timeouts and rate limits.
- Solved Healthie→Medplum appointment status drift with extension-aware reconciliation and rescheduled cancelationReason handling.

---

## Quality & delivery

- Automated tests (unit / integration / e2e) for the paths you owned
- Feature flags or gradual rollout for risky changes
- Staging (or equivalent) before production
- Clear PR / stacked delivery when work was phased

**CV-ready bullets:**

- Followed repo safety guidelines: dry-run default, staging-first, peer-reviewed PRs, PHI-free logging, and Linear-attached execution logs.
- Iterated BAR-1530 backfill through 15+ commits — batching, rate limits, progress tracking, single-patient mode — before prod rollout.
- Phased BAR-1497 delivery: initial procedure-stage script (#38), then extension-cleanup enhancement (#42) before bulk bot runs.

---

## Ownership & process

> Spec → plan → implement → review → ship (only claim what you did).

- Authored or drove: BAR-1530, BAR-1497, BAR-2017 operational scripts from requirements through PR review
- Reviewed: peer-review required per repo README before any prod execution
- Partnered with: ops/clinical data team on NPI lists, procedure-stage rules, and Healthie migration fidelity
- Led rollout: staging dry-runs → log review → prod execution with checkpoint resume

**CV-ready bullets:**

- Drove BAR-1530 data-normalization scripts from first commit through production-hardening (pagination, throttling, progress files) with ops sign-off workflow.
- Partnered on Healthie migration cleanup — procedure stages and appointment status reconciliation — encoding clinical mapping rules into auditable CLI tooling.

---

## Cross-cutting work

> Often under-counted on resumes; call out if you owned them.

| Area | Examples you touched | CV-worthy? |
| ---- | -------------------- | ---------- |
| Auth / sessions / permissions | Medplum client credentials via env | N |
| Observability (logging, metrics, error tracking) | Execution-time logging, progress stats, CSV audit trails | Y |
| Notifications (email / SMS / push) | _ | N |
| Payments / billing | Procedure/payment state via update-procedure-status bot orchestration | Y |
| Background jobs / bots / workers | Bulk bot invocation with concurrency cap | Y |
| Design system / shared UI | _ | N |
| Developer experience / CI / tooling | Timestamped script conventions, progress file patterns | Y |
| Security / compliance / privacy | PHI-free logging guidelines, dry-run defaults, staging-first | Y |

**CV-ready bullets:**

- Implemented HIPAA-conscious operational logging — internal IDs, counts, and summaries only — across all authored backfill scripts.
- Built reusable Medplum API protection layer (RPS, retries, page delays) copied across patient, practitioner, and appointment scripts.

---

## Polished resume bullets (draft)

> Internal draft — refine here, then copy into **ATS export** and **LinkedIn export** below.  
> Aim for action + scope + outcome (+ metric when real).

1. Authored production-safe Medplum/FHIR backfill scripts for patient Medical Record IDs and practitioner NPIs with dry-run defaults, cursor pagination, and API rate limiting.
2. Built bulk procedure-stage update orchestration invoking Medplum bots with concurrency caps and pre-execution extension cleanup for bariatric patient cohorts.
3. Delivered Healthie appointment status reconciliation tooling comparing migrated pm_status metadata against FHIR status with CSV audit trails for ops review.
4. Established repeatable ops script patterns — progress checkpoints, retry/backoff, PHI-free logging — for staging-to-prod data hygiene on a clinical platform.

---

## ATS export (single role — copy to master CV)

> **Do not submit this file to an ATS.** Copy the block below into your consolidated CV under **Experience**.  
> Plain text only: no tables, PR numbers, repo paths, code, or markdown formatting in the filled export.

```
Everself | Software Engineer | Feb 2026 – Mar 2026 | Remote

Authored production-safe Medplum/FHIR operational scripts for bariatric care data hygiene — backfills, bot orchestration, and Healthie reconciliation.

• Built patient Medical Record ID and practitioner NPI backfill scripts with dry-run defaults, cursor pagination, API throttling, and resume checkpoints for staging-to-prod rollout.
• Orchestrated bulk procedure-stage updates via Medplum bots with concurrency limits and pre-execution extension cleanup across patient cohorts.
• Delivered Healthie appointment status reconciliation comparing migrated metadata against FHIR status with CSV audit output for ops review.
• Established reusable production-safe patterns — PHI-free logging, retry/backoff, progress files — across operational data-fix tooling.

Skills: TypeScript, Medplum, FHIR, Node.js, Healthie, CLI automation, Data migration
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
**Dates:** Feb 2026 – Mar 2026  
**Location:** Remote

**Description:**

Operational engineering on Everself's Medplum provider platform — authoring audited backfill and reconciliation scripts for patient identifiers, procedure stages, and Healthie migration data quality.

• Built patient Medical Record ID and practitioner NPI backfill scripts with dry-run defaults, cursor pagination, API throttling, and resume checkpoints.  
• Orchestrated bulk procedure-stage updates via Medplum bots with concurrency limits and pre-execution extension cleanup.  
• Delivered Healthie appointment status reconciliation with CSV audit trails for ops review before production execution.  
• Established reusable production-safe script patterns — PHI-free logging, retry/backoff, progress checkpoints — for clinical data hygiene.

**Skills to tag (optional):** TypeScript, Medplum, FHIR, Data Migration, Node.js, Health Tech

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
git log --author='Eduardo Neves' --oneline -- scripts/
```

### Key PRs / commit ranges

| PR | Title (branch) | Merged | Feature cluster |
| -- | -------------- | ------ | --------------- |
| #9 | BAR-1530 — Health Gorilla resource normalization | 2026-02 | Patient MR ID + Practitioner NPI |
| #38 | BAR-1497 — procedure states script | 2026-03 | Procedure stage bulk update |
| #42 | BAR-1497 — remove extension before bot | 2026-03 | Procedure stage bulk update |
| — | BAR-2017 — Healthie status mismatches | branch | Appointment reconciliation |

### Other repositories

> Copy this template (or add a section below) per repo/company.

- [x] `everself/medplum-provider` — done (companion app repo)
- [x] `everself/medplum-provider-scripts` — done (this file)

---

## Changelog of this doc

| Date | Change |
| ---- | ------ |
| 2026-08-03 | Origin-specific draft created |
| 2026-08-04 | Generalized into repo-agnostic template |
| 2026-08-04 | Added ATS export and LinkedIn export sections (per-repo copy blocks) |
| 2026-08-04 | Filled from commits/PRs for `everself/medplum-provider-scripts` |
