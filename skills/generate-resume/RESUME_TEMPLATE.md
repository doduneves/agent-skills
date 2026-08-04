# Engineering highlights (CV source)

> **Purpose:** Personal draft material for resume / LinkedIn bullets.  
> **Scope:** One repository (or product) per filled copy of this template. Duplicate the file per company/repo as needed.  
> **Primary sources:** Your merged PRs and commits (not chat logs).  
> **Status:** Generic template — fill from `git log --author=…` / `gh pr list --author=@me`.

---

## Role / context

| Field | Value |
| ----- | ----- |
| Company / product | _ |
| Repository | _e.g. `org/repo` or local path_ |
| Your role | _e.g. Software Engineer_ |
| Date range | _e.g. YYYY-MM – YYYY-MM (or Present)_ |
| Areas / surfaces you owned | _e.g. web app / API / admin / mobile / shared libs_ |
| GitHub username | _@…_ |
| Git author email(s) | _used to filter commits_ |

**One-line context (for CV header under the job):**  
_e.g. Built and shipped features across a … stack serving …_

---

## Features built

> One feature ≈ one merged PR (or a small PR stack). Prefer outcome language over file lists.

### Feature: _Name_

- **Summary:** _What shipped and why it mattered_
- **Outcome bullets (CV-ready):**
  - _
  - _
- **Scope:** _UI / API / data model / jobs / flags / infra_
- **Surfaces:** _web / mobile / admin / internal tools / …_
- **PRs:** _#…_
- **Notes:** _

### Feature: _Name_

- **Summary:**
- **Outcome bullets (CV-ready):**
  - _
- **Scope:**
- **Surfaces:**
- **PRs:**
- **Notes:**

<!-- Duplicate the block above per feature. -->

---

## Tech used

> Group for the CV “tech stack” line; keep only what you actually used in this repo.

### Languages & runtime

- _

### Frontend

- _

### Backend / services

- _

### Data / storage

- _

### Integrations / third parties

- _

### Tooling & quality

- _

### Infra / delivery

- _

**Short CV stack line (draft):**  
`Lang · Framework · Datastore · Tests · Cloud · …`

---

## Migrations / one-off scripts

> Prefer scripts you authored or owned. Note safety patterns (dry-run by default, batching, backups, idempotency) when relevant.

| Script / migration | Purpose | Approx. records / users affected | Env (staging → prod) | PR / path | Notes |
| ------------------ | ------- | -------------------------------- | -------------------- | --------- | ----- |
| _name_ | _what it fixed/backfilled_ | _e.g. N rows / unknown — needs estimate_ | _staging signed off, then prod_ | `_path/…_` | _dry-run default, `--commit`_ |

**CV-ready bullets (draft):**

- _
- _

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
| Users / customers / accounts affected | _ | _script log / PR / analytics_ |
| Entities touched (orders, records, jobs, …) | _ | _ |
| Teams / tenants / environments in scope | _ | _ |
| Error rate / support tickets reduced | _ | _ |
| Latency / cost / perf improvement | _ | _ |

**CV-ready impact lines:**

- _
- _

---

## Architecture / domain

> High-signal domain ownership — strong for senior-leaning bullets.

- **System shape:** _monolith / monorepo / services / event-driven / …_
- **Domain focus:** _billing / auth / search / scheduling / …_
- **Cross-cutting concerns you owned:** _tenancy, permissions, data consistency, …_
- **Your focus areas:** _fill in_

**CV-ready bullets:**

- _
- _

---

## Hard problems solved

> Depth markers: correctness under edge cases, scale, consistency, security, migrations, ambiguous product rules, etc.

| Problem | What was hard | Approach | Outcome |
| ------- | ------------- | -------- | ------- |
| _short name_ | _ | _ | _ |

**CV-ready bullets:**

- _
- _

---

## Quality & delivery

- Automated tests (unit / integration / e2e) for the paths you owned
- Feature flags or gradual rollout for risky changes
- Staging (or equivalent) before production
- Clear PR / stacked delivery when work was phased

**CV-ready bullets:**

- _
- _

---

## Ownership & process

> Spec → plan → implement → review → ship (only claim what you did).

- Authored or drove: _specs / RFCs / implementation plans_
- Reviewed: _others’ PRs in …_
- Partnered with: _product, design, ops, data, …_
- Led rollout: _flag on → soak → prod / docs / comms_

**CV-ready bullets:**

- _
- _

---

## Cross-cutting work

> Often under-counted on resumes; call out if you owned them.

| Area | Examples you touched | CV-worthy? |
| ---- | -------------------- | ---------- |
| Auth / sessions / permissions | _ | Y/N |
| Observability (logging, metrics, error tracking) | _ | Y/N |
| Notifications (email / SMS / push) | _ | Y/N |
| Payments / billing | _ | Y/N |
| Background jobs / bots / workers | _ | Y/N |
| Design system / shared UI | _ | Y/N |
| Developer experience / CI / tooling | _ | Y/N |
| Security / compliance / privacy | _ | Y/N |

**CV-ready bullets:**

- _
- _

---

## Polished resume bullets (draft)

> Internal draft — refine here, then copy into **ATS export** and **LinkedIn export** below.  
> Aim for action + scope + outcome (+ metric when real).

1. _
2. _
3. _
4. _
5. _

---

## ATS export (single role — copy to master CV)

> **Do not submit this file to an ATS.** Copy the block below into your consolidated CV under **Experience**.  
> Plain text only: no tables, PR numbers, repo paths, code, or markdown formatting in the filled export.

```
Company / Product | Role Title | MMM YYYY – MMM YYYY (or Present) | Location (optional)

One-line context (optional, max 1 sentence).

• Bullet 1
• Bullet 2
• Bullet 3

Skills: Lang, Framework, Datastore, Tests, Cloud, …
```

**Rules when filling:**

- 3–6 bullets max — prioritize highest-signal outcomes from this repo.
- Use a consistent date format across all repo exports (e.g. `Jan 2024 – Present`).
- Metrics only when evidenced in **Impact / metrics** above.
- **Skills** line: comma-separated keywords from **Tech used** (what you actually used here).

---

## LinkedIn export (Experience entry — copy to profile)

> Copy fields below into LinkedIn → Experience. Slightly more narrative than ATS is fine; still no PR numbers or repo paths.

**Title:** _Role Title_  
**Company:** _Company / Product name_  
**Dates:** _MMM YYYY – MMM YYYY (or Present)_  
**Location:** _City, Country or Remote_

**Description:**

_Optional 1–2 sentence overview of your scope and impact on this product._

• _Bullet 1_  
• _Bullet 2_  
• _Bullet 3_

**Skills to tag (optional):** _comma-separated list for LinkedIn skill suggestions_

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
git log --author='YOUR_NAME_OR_EMAIL' --oneline --no-merges

# Merged PRs by you (needs gh auth; run inside the repo)
gh pr list --author=@me --state merged --limit 100

# Optional: commits touching migrations / operational scripts
git log --author='YOUR_NAME_OR_EMAIL' --oneline -- scripts/ migrations/ db/
```

### Key PRs / commit ranges

| PR | Title | Merged | Feature cluster |
| -- | ----- | ------ | --------------- |
| #_ | _ | _ | _ |

### Other repositories

> Copy this template (or add a section below) per repo/company.

- [ ] _org/repo_ — _status: not started / in progress / done_
- [ ] _org/repo_ — _

---

## Changelog of this doc

| Date | Change |
| ---- | ------ |
| 2026-08-03 | Origin-specific draft created |
| 2026-08-04 | Generalized into repo-agnostic template |
| 2026-08-04 | Added ATS export and LinkedIn export sections (per-repo copy blocks) |
| _ | Filled from commits/PRs for _repo_ |
