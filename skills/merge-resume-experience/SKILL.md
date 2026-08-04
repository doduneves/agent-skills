---
name: merge-resume-experience
description: >-
  Merge multiple per-repo resume source docs into one employer/role experience
  with ATS and LinkedIn export blocks. Use when the user says
  "/merge-resume-experience", "merge resume experience", "combine repo resumes",
  or "aggregate CV from repos". Args: "/merge-resume-experience {slug}" — manifest
  at resume/experiences/{slug}.yml; optional "--refresh" re-runs /generate-resume
  on each repo in the manifest first. Supports optional projects for sectioned
  LinkedIn exports (one employer, multiple client/project sections).
disable-model-invocation: true
---

# Merge resume experience

Combine **Layer 1** per-repo docs (`resume/by-repo/{slug}/`) into **Layer 2** one employer entry (`resume/by-experience/{slug}/`). Never edit bundled templates or manifests unless the user asks.

Requires prior `/generate-resume` runs for each repo (or use `--refresh`).

## Archive location

Resolve paths from the skill directory:

1. `SKILL_DIR` — directory containing this `SKILL.md` (follow symlinks).
2. `AGENT_SKILLS_ROOT` — parent of `skills/` (repo root).
3. `RESUME_ROOT` — `{AGENT_SKILLS_ROOT}/resume/`
4. `EXPERIENCES_DIR` — `{RESUME_ROOT}/experiences/`
5. `EXPERIENCE_DIR` — `{RESUME_ROOT}/by-experience/{slug}/`
6. Template — `{SKILL_DIR}/EXPERIENCE_TEMPLATE.md`
7. Example manifest — `{EXPERIENCES_DIR}/example.yml`
8. Personal profile — `{RESUME_ROOT}/profile.yml` (optional; see **Personal profile**)
9. Profile template — `{RESUME_ROOT}/profile.example.yml`

## Invocation

| User says | Manifest | Output |
| --------- | -------- | ------ |
| `/merge-resume-experience {slug}` | `{EXPERIENCES_DIR}/{slug}.yml` | `{EXPERIENCE_DIR}/RESUME_EXPERIENCE_{date}_{slug}.md` |
| `/merge-resume-experience {slug} --refresh` | same | same (after refreshing each repo) |

- `--refresh` may appear before or after `{slug}`.
- If manifest missing → tell user to copy `{EXPERIENCES_DIR}/example.yml` to `{EXPERIENCES_DIR}/{slug}.yml` and edit paths.
- If `{slug}` omitted → list available manifests in `{EXPERIENCES_DIR}/` or ask which experience to merge.

## Manifest schema (`resume/experiences/{slug}.yml`)

### Top-level fields

| Field | Required | Notes |
| ----- | -------- | ----- |
| `slug` | yes | Must match filename stem and output folder |
| `company` | yes | Employer name for exports (e.g. Vinta Software) |
| `role` | yes | Job title |
| `dates` | yes | Overall span, e.g. `Nov 2025 – Aug 2026` |
| `location` | no | e.g. `Remote`, `São Paulo, Brazil` |
| `context` | no | One-line scope for ATS header; also seeds LinkedIn intro when sectioned |
| `employment_type` | no | e.g. `Contract` — shown in LinkedIn export header |
| `repos` | yes* | Flat repo list; used when `projects` absent, or as fallback for `--refresh` |
| `projects` | no | Client/project groupings for sectioned LinkedIn (see below) |
| `sources` | no | Pinned paths relative to `resume/`; overrides auto-latest lookup |
| `linkedin_format` | no | `flat` (default) or `sectioned`. Default `sectioned` when `projects` is set |
| `project_order` | no | `reverse_chron` (default) or `manifest` — order of LinkedIn project sections |
| `max_bullets_ats` | no | Default `8` |
| `max_bullets_linkedin` | no | Default `8`; total cap for flat LinkedIn only |
| `use_profile` | no | Default `true` — load `profile.yml` for skills enrichment and role fallback |

\*At least one of `repos`, `projects[].repos`, or `sources` must be usable.

### Projects block (optional — sectioned LinkedIn)

Use when one employer entry covers multiple client or internal projects (e.g. contract at Vinta across Everself, Origin, Building Blocks).

```yaml
projects:
  - name: Origin Therapy
    dates: Apr 2026 – Jul 2026
    max_bullets: 4
    repos:
      - path: ~/Apps/origin/origin-apps
        slug: origin-apps
  - name: Everself
    dates: Nov 2025 – Mar 2026
    max_bullets: 4
    repos:
      - path: ~/Apps/bariendo/medplum-provider
        slug: medplum-provider
      - path: ~/Apps/bariendo/medplum-provider-scripts
        slug: medplum-provider-scripts
```

| Project field | Required | Notes |
| ------------- | -------- | ----- |
| `name` | yes | Section header in LinkedIn export (client or product name) |
| `dates` | no | Shown in section header; omit if same as manifest |
| `max_bullets` | no | Default `4` — cap per project section |
| `repos` | yes | Same shape as top-level `repos` (`path`, optional `slug`) |

When `projects` is set:

- **`--refresh`** iterates all repos from every project (dedupe by slug).
- **Source resolution** maps each repo slug to its per-repo doc.
- **LinkedIn** uses sectioned export (intro + project blocks).
- **ATS** uses flat export with client names inline in bullets.

When `projects` is absent → flat merge for both ATS and LinkedIn (existing behavior).

## Steps

### 1. Load manifest

1. Parse flags: `REFRESH=true` when `--refresh` present.
2. Resolve paths (see **Archive location**).
3. Read `{EXPERIENCES_DIR}/{slug}.yml`. Validate required fields.
4. Detect mode: `SECTIONED=true` when `projects` is non-empty, or when `linkedin_format: sectioned`.
5. Build repo list: union of `repos` and all `projects[].repos` (dedupe by slug).
6. Record export limits: `max_bullets_ats`, `max_bullets_linkedin`, per-project `max_bullets` (default 4).
7. Load profile when `use_profile` is not `false` and `{RESUME_ROOT}/profile.yml` exists (see **Personal profile**).

### 2. Refresh per-repo sources (when `--refresh`)

For each unique repo in the manifest:

1. Resolve `path` (expand `~`, relative to workspace).
2. Follow the **generate-resume** skill for that path (invoke `/generate-resume {path}` workflow — new dated file in `by-repo/{repo-slug}/`, update `INDEX.md`).
3. Do not skip repos silently; report failures per repo.

### 3. Resolve source files

For each repo slug in the manifest:

1. If `sources` is set in the manifest → use those paths (must exist under `{RESUME_ROOT}/`).
2. Else → pick the **newest** `RESUME_EXPERIENCE_*.md` in `{RESUME_ROOT}/by-repo/{slug}/` (by date in filename, then mtime).
3. If no source file found for a repo → stop and tell user to run `/generate-resume {path}` or use `--refresh`.

### 4. Extract content from each source

From each per-repo doc, pull (do not copy raw tables into exports):

- **Role / context** — company, role, dates, one-line context (for cross-check only; manifest wins in output)
- **Polished resume bullets** — numbered list or **Outcome bullets (CV-ready)** under features
- **ATS export** — bullets and Skills line if section exists
- **LinkedIn export** — description overview and bullets if section exists
- **Tech used** / **Short CV stack line** — for skills union
- **Feature clusters** — count `### Feature:` sections for the source table

If a source predates ATS/LinkedIn export sections, use polished bullets and tech sections only.

### 5. Merge logic

#### 5a. Shared

1. **Metadata** — use manifest `company`, `role`, `dates`, `location`, `context` (refine context from sources if manifest empty). Role fallback: `identity.default_role` from profile when manifest `role` empty.
2. **Skills** — union keywords from all sources' Skills lines and **Tech used**; dedupe; apply **profile enrichment** (see **Personal profile**); comma-separated.
3. **Senior framing** — when profile loaded and `senior_framing.enabled`, upgrade bullet verbs where evidence supports senior scope (architectural, cross-cutting, owned systems).
4. **Merge notes** — record what was deduped, dropped, profile skills added, or needs human review.

#### 5b. ATS export (always flat)

1. **Collect** all bullets from all repos across projects (or flat `repos`).
2. **Cluster & dedupe globally** — merge bullets that describe the same outcome across repos/projects.
3. **Rank** — metrics > cross-cutting impact > domain depth > narrow UI tweaks.
4. **Prefix client names** when `projects` is set — lead bullets with project name where it aids clarity, e.g. *"For Origin Therapy, built ops-portal FHIR template lifecycle tooling…"*
5. **Cap** — top `max_bullets_ats`.
6. Plain text only: no PR numbers, repo paths, or internal jargon.

#### 5c. LinkedIn export — flat mode (`projects` absent)

Same as before: collect all bullets, dedupe globally, rank, cap `max_bullets_linkedin`. Optional 1–2 sentence overview from `context`.

#### 5d. LinkedIn export — sectioned mode (`projects` set)

1. **Intro** — 2–3 sentences from manifest `context` (contract/employer-wide scope; no per-project repetition).
2. **Project order** — `reverse_chron` by `projects[].dates` (default), or manifest order when `project_order: manifest`.
3. **Per project:**
   - Section header: `NAME (dates)` in ALL CAPS or Title Case on its own line (blank line before).
   - Collect bullets only from that project's repos.
   - Dedupe/rank **within the project** (merge bullets from multiple repos in same project, e.g. medplum-provider + scripts).
   - Do **not** dedupe across projects — keep sections distinct.
   - Cap at `projects[].max_bullets` (default 4).
4. **Header fields** — manifest `company`, `role`, overall `dates`, `location`, optional `employment_type`.
5. **Skills to tag** — union from all sources (same as ATS).

Fill **LinkedIn export (sectioned)** in the output doc. Leave **LinkedIn export (flat)** empty or omit when sectioned.

### 6. Write output (new file only)

1. Read `{SKILL_DIR}/EXPERIENCE_TEMPLATE.md` (**do not modify it**).
2. Create filled copy at:

   `{EXPERIENCE_DIR}/RESUME_EXPERIENCE_{YYYY-MM-DD}_{slug}.md`

   - Suffix `_2`, `_3`, … if file exists — never overwrite.
   - Ensure `{EXPERIENCE_DIR}/` exists.

3. Fill template sections from merge results.
   - When sectioned: fill **LinkedIn export (sectioned)**; skip or mark N/A the flat LinkedIn block.
   - When flat: fill **LinkedIn export (flat)** only.
4. Update **Changelog** with today's date, source count, and mode (flat vs sectioned).

### 7. Update experience index

Update `{RESUME_ROOT}/EXPERIENCE_INDEX.md`:

1. Create from stub below if missing.
2. **Prepend** row (newest first): Date, Experience slug, Company, Source repos (count), File, ATS bullets (count).
3. Do not remove existing rows.

**EXPERIENCE_INDEX.md stub:**

```markdown
# Merged experience index

Personal archive of employer/role docs merged by `/merge-resume-experience`.

| Date | Slug | Company | Sources | File | ATS bullets |
| ---- | ---- | ------- | ------- | ---- | ----------- |
```

### 8. Report to the user

- Output file path
- Manifest used
- Merge mode: flat or sectioned (with project names if sectioned)
- Source files merged (with dates)
- Whether `--refresh` ran and which repos were updated
- Index updated at `{RESUME_ROOT}/EXPERIENCE_INDEX.md`
- Bullet counts before/after dedupe (ATS global; LinkedIn per project if sectioned)
- Copy **ATS export** to master CV
- Copy **LinkedIn export (sectioned)** or **LinkedIn export (flat)** to LinkedIn profile
- Items flagged in **Merge notes** for review
- Whether `profile.yml` was loaded and which profile skills were added

## Personal profile (`resume/profile.yml`)

Optional gitignored file. Copy from `profile.example.yml`. Both `/generate-resume` and this skill use it with **evidence-first** rules.

### When to load

- Manifest `use_profile: false` → skip profile entirely.
- Else if `{RESUME_ROOT}/profile.yml` exists → load and enrich.
- Else → repo-only skills (current behavior).

### Enrichment algorithm (merged Skills line)

1. **Union** skills from all source docs (ATS Skills lines, Tech used, Short CV stack).
2. **Scan** combined source text for keywords in `export_rules.category_triggers`.
3. For each match → append skills from that profile category, excluding `export_rules.export_exclude`.
4. Include `core_always_include` when any source uses TypeScript/React stack.
5. **Dedupe**, order repo-evidenced skills first, then profile additions.
6. **Cap** at manifest override or `export_rules.max_skills_ats` / `max_skills_linkedin`.

### Role, intro, and bullets

- **Role:** manifest `role` wins; fallback `identity.default_role`.
- **LinkedIn intro:** may incorporate `cv_summary` themes when aligned with manifest `context` — do not replace manifest context.
- **Senior framing:** prefer `senior_framing.verbs` on ranked bullets when scope warrants it; never on dropped/low-signal items.

### What profile must NOT do

- Add Python, NestJS, Azure, Redis, etc. to exports unless a source doc or repo trigger keyword evidences them.
- Add AI/MCP skills when listed in `export_exclude`.
- Invent metrics, client names, or features not in source docs.

## Rules

- Never mutate `EXPERIENCE_TEMPLATE.md` or `{EXPERIENCES_DIR}/example.yml` during a merge run.
- Never overwrite existing merged output — always new dated file.
- Manifest metadata wins over per-repo Role/context for the merged entry.
- Do not invent metrics or ownership not present in source docs.
- One experience slug = one employer/role entry. Multiple client projects under one employer use `projects` in a single manifest (e.g. `vinta.yml`).
- Per-repo docs remain the evidence layer; merged doc is the copy-paste layer for CV/LinkedIn.

## Related

- **Layer 1:** `/generate-resume` — one repo → `resume/by-repo/{slug}/`
- **Example manifest:** `{EXPERIENCES_DIR}/example.yml` → copy to `{EXPERIENCES_DIR}/{slug}.yml`
- **Multi-project example:** `{EXPERIENCES_DIR}/vinta.yml` — Vinta contract with Everself, Origin, Building Blocks
- **Personal profile:** `{RESUME_ROOT}/profile.yml` — senior skill inventory and export enrichment
