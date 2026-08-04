---
name: merge-resume-experience
description: >-
  Merge multiple per-repo resume source docs into one employer/role experience
  with ATS and LinkedIn export blocks. Use when the user says
  "/merge-resume-experience", "merge resume experience", "combine repo resumes",
  or "aggregate CV from repos". Args: "/merge-resume-experience {slug}" — manifest
  at resume/experiences/{slug}.yml; optional "--refresh" re-runs /generate-resume
  on each repo in the manifest first.
disable-model-invocation: true
---

# Merge resume experience

Combine **Layer 1** per-repo docs (`resume/by-repo/{slug}/`) into **Layer 2** one employer entry (`resume/by-experience/{slug}/`). Never edit bundled templates or manifests unless the user asks.

Requires prior `/generate-resume` runs for each repo (or use `--refresh`).

## Archive location

Resolve paths from the skill directory:

1. `SKILL_DIR` — directory containing this `SKILL.md`.
2. `AGENT_SKILLS_ROOT` — parent of `skills/` (repo root).
3. `RESUME_ROOT` — `{AGENT_SKILLS_ROOT}/resume/`
4. `EXPERIENCES_DIR` — `{RESUME_ROOT}/experiences/`
5. `EXPERIENCE_DIR` — `{RESUME_ROOT}/by-experience/{slug}/`
6. Template — `{SKILL_DIR}/EXPERIENCE_TEMPLATE.md`
7. Example manifest — `{EXPERIENCES_DIR}/example.yml`

## Invocation

| User says | Manifest | Output |
| --------- | -------- | ------ |
| `/merge-resume-experience {slug}` | `{EXPERIENCES_DIR}/{slug}.yml` | `{EXPERIENCE_DIR}/RESUME_EXPERIENCE_{date}_{slug}.md` |
| `/merge-resume-experience {slug} --refresh` | same | same (after refreshing each repo) |

- `--refresh` may appear before or after `{slug}`.
- If manifest missing → tell user to copy `{EXPERIENCES_DIR}/example.yml` to `{EXPERIENCES_DIR}/{slug}.yml` and edit paths.
- If `{slug}` omitted → list available manifests in `{EXPERIENCES_DIR}/` or ask which experience to merge.

## Manifest schema (`resume/experiences/{slug}.yml`)

| Field | Required | Notes |
| ----- | -------- | ----- |
| `slug` | yes | Must match filename stem and output folder |
| `company` | yes | Employer / product name for exports |
| `role` | yes | Job title |
| `dates` | yes | e.g. `Apr 2026 – Present` |
| `location` | no | e.g. `Remote`, `São Paulo, Brazil` |
| `context` | no | One-line scope; agent may refine from sources |
| `repos` | yes* | Local paths for `--refresh`; each may include optional `slug` |
| `sources` | no | Pinned paths relative to `resume/`; overrides auto-latest lookup |
| `max_bullets_ats` | no | Default `8` |
| `max_bullets_linkedin` | no | Default `8` |

\*At least one of `repos` (for refresh) or `sources` (for merge) must be usable. For merge without refresh, resolve sources from `repos[].slug` or infer slug from each repo path.

## Steps

### 1. Load manifest

1. Parse flags: `REFRESH=true` when `--refresh` present.
2. Resolve paths (see **Archive location**).
3. Read `{EXPERIENCES_DIR}/{slug}.yml`. Validate required fields.
4. Record export limits: `max_bullets_ats`, `max_bullets_linkedin` (defaults 8).

### 2. Refresh per-repo sources (when `--refresh`)

For each entry in `repos`:

1. Resolve `path` (expand `~`, relative to workspace).
2. Follow the **generate-resume** skill for that path (invoke `/generate-resume {path}` workflow — new dated file in `by-repo/{repo-slug}/`, update `INDEX.md`).
3. Do not skip repos silently; report failures per repo.

### 3. Resolve source files

For each repo in the manifest:

1. If `sources` is set in the manifest → use those paths (must exist under `{RESUME_ROOT}/`).
2. Else for each `repos[].slug` (or inferred slug) → pick the **newest** `RESUME_EXPERIENCE_*.md` in `{RESUME_ROOT}/by-repo/{slug}/` (by date in filename, then mtime).
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

1. **Metadata** — use manifest `company`, `role`, `dates`, `location`, `context` (refine context from sources if manifest empty).
2. **Collect** all bullets from polished lists, feature outcome bullets, and existing export blocks across sources.
3. **Cluster & dedupe** — merge bullets that describe the same outcome, feature, or theme (e.g. billing in API + web → one bullet). Prefer the stronger wording (metrics, scope, outcome).
4. **Rank** — metrics > cross-cutting impact > domain depth > narrow UI tweaks. Drop chore-only or redundant lines.
5. **Cap** — top `max_bullets_ats` for ATS export; top `max_bullets_linkedin` for LinkedIn (may match ATS or add slightly more narrative).
6. **Skills** — union keywords from all sources' Skills lines and **Tech used**; dedupe; comma-separated.
7. **Exports** — plain text in ATS block: no PR numbers, repo paths, or internal jargon.
8. **Merge notes** — record what was deduped, dropped, or needs human review.

### 6. Write output (new file only)

1. Read `{SKILL_DIR}/EXPERIENCE_TEMPLATE.md` (**do not modify it**).
2. Create filled copy at:

   `{EXPERIENCE_DIR}/RESUME_EXPERIENCE_{YYYY-MM-DD}_{slug}.md`

   - Suffix `_2`, `_3`, … if file exists — never overwrite.
   - Ensure `{EXPERIENCE_DIR}/` exists.

3. Fill template sections from merge results.
4. Update **Changelog** with today's date and source count.

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
- Source files merged (with dates)
- Whether `--refresh` ran and which repos were updated
- Index updated at `{RESUME_ROOT}/EXPERIENCE_INDEX.md`
- Bullet counts before/after dedupe
- Copy **ATS export** / **LinkedIn export** into CV or LinkedIn
- Items flagged in **Merge notes** for review

## Rules

- Never mutate `EXPERIENCE_TEMPLATE.md` or `{EXPERIENCES_DIR}/example.yml`.
- Never overwrite existing merged output — always new dated file.
- Manifest metadata wins over per-repo Role/context for the merged entry.
- Do not invent metrics or ownership not present in source docs.
- One experience slug = one employer/role entry; different companies = different manifests.
- Per-repo docs remain the evidence layer; merged doc is the copy-paste layer for CV/LinkedIn.

## Related

- **Layer 1:** `/generate-resume` — one repo → `resume/by-repo/{slug}/`
- **Example manifest:** `{EXPERIENCES_DIR}/example.yml` → copy to `{EXPERIENCES_DIR}/{slug}.yml`
