---
name: generate-resume
description: >-
  Generate a dated resume/CV experience markdown for a git repository by copying
  RESUME_TEMPLATE.md and filling it from the user's merged PRs and commits.
  Use when the user says "/generate-resume", "generate resume", "generate CV",
  "fill resume from this repo", or "resume experience from git". Optional args:
  "/generate-resume {git-repo}" — target repo path; "/generate-resume --in-repo"
  — also write a copy inside the target repo root (default saves to the
  agent-skills repo resume/ archive).
disable-model-invocation: true
---

# Generate resume

Create a **new dated markdown file** from the bundled template. Never edit the template itself.

## Archive location

Generated files live in this repo under `resume/` (gitignored). Resolve paths from the skill directory:

1. `SKILL_DIR` — directory containing this `SKILL.md` (follow symlinks).
2. `AGENT_SKILLS_ROOT` — parent of `skills/` (repo root).
3. `RESUME_ROOT` — `{AGENT_SKILLS_ROOT}/resume/`
4. `RESUME_DIR` — `{RESUME_ROOT}/by-repo/{slug}/`
5. Template — `{SKILL_DIR}/RESUME_TEMPLATE.md`

## Invocation

| User says | Target repo | Output location |
| --------- | ----------- | --------------- |
| `/generate-resume` or "generate resume" | Current workspace (must be a git repo) | `{RESUME_DIR}/` |
| `/generate-resume {path}` | That path (absolute or relative) | `{RESUME_DIR}/` |
| `/generate-resume --in-repo` or `/generate-resume {path} --in-repo` | Same as above | Archive **and** `{REPO_ROOT}/` |

- `--in-repo` may appear before or after `{path}`.
- If no path is given and the workspace is not a git repo → stop and ask for `{git-repo}`.

## Personal archive layout

```
{AGENT_SKILLS_ROOT}/resume/
  INDEX.md                              # updated after every run (gitignored)
  by-repo/
    {slug}/
      RESUME_EXPERIENCE_{date}_{slug}.md
```

- `{slug}` = short repo name (e.g. `origin-apps` from `Origin-Therapy/origin-apps`, or directory basename).
- Create `{RESUME_DIR}/` if missing.

## Steps

### 1. Resolve target repo

1. Parse flags: set `WRITE_IN_REPO=true` when `--in-repo` is present; strip the flag before resolving path.
2. Resolve `SKILL_DIR`, `AGENT_SKILLS_ROOT`, `RESUME_ROOT`, and `RESUME_DIR` (see **Archive location** above).
3. If `{git-repo}` was passed, `cd` there (resolve relative paths against the workspace).
4. Verify it is a git repo: `git rev-parse --show-toplevel`.
5. Record:
   - `REPO_ROOT`
   - `slug` from remote (`owner/repo` → repo segment) or directory basename
   - `REPO_FULL` = `owner/repo` when remote available, else `slug`
   - remote URL if available

### 2. Resolve author identity (cascade — do not ask unless all fail)

Run in order; stop when you have a GitHub login and at least one author filter for commits:

1. **GitHub CLI:** `gh api user` → `login`, optional `name`, `email` if present. Also try `gh api user/emails` when authenticated.
2. **GitLens / GitKraken MCP** (if available): `pull_request_assigned_to_me` with `provider: github` and fields including author identity (`authoredByMe`). Infer login from results authored by you.
3. **Local git config** (in `REPO_ROOT`): `git config user.email`, `git config user.name`.
4. **Ask the user** only if still missing identity.

Use GitHub `login` for PR filters (`gh pr list --author=LOGIN` or `--author=@me` when `gh` is that user). Use email(s) / name for `git log --author=…`.

### 3. Gather evidence (in `REPO_ROOT`)

Prefer merged PRs; use commits to fill gaps.

```bash
gh pr list --author=LOGIN --state merged --limit 100
# or: gh pr list --author=@me --state merged --limit 100

git log --author='EMAIL_OR_NAME' --oneline --no-merges -n 200

# Migrations / scripts (adjust paths to what exists)
git log --author='EMAIL_OR_NAME' --oneline -- scripts/ migrations/ db/ 2>/dev/null
```

Also skim repo context for stack/domain (do not invent):

- `README*`, `AGENTS.md`, `package.json` / lockfiles, docs the project already has
- Paths and PR titles the user actually touched

If GitLens MCP can list PRs for this org/repo assigned/authored by you, use it as a supplement — `gh` remains primary when available.

### 4. Write output (new file only)

1. Read the template at `{SKILL_DIR}/RESUME_TEMPLATE.md` (**do not modify it**).
2. Create a **filled copy** at:

   `{RESUME_DIR}/RESUME_EXPERIENCE_{YYYY-MM-DD}_{slug}.md`

   - `{YYYY-MM-DD}` = today's date
   - If that path already exists, append a suffix (`_2`, `_3`, …) — never overwrite.
   - Ensure `{RESUME_DIR}/` exists before writing.

3. **Optional repo copy:** when `WRITE_IN_REPO=true`, also write the same filled content to:

   `{REPO_ROOT}/RESUME_EXPERIENCE_{YYYY-MM-DD}_{slug}.md`

   (same suffix rule if file exists; never overwrite.)

4. Fill every section of the template structure you can support with evidence:
   - Infer **company/product**, **tech**, and **domain terms** from the repo (e.g. FHIR/Medplum when present elsewhere differently).
   - Cluster merged PRs into **features** (outcome language, not file lists).
   - Migrations/scripts: purpose + path/PR; impact counts only if evidenced — else `unknown — needs estimate`.
   - Draft **Polished resume bullets**, then fill **ATS export** and **LinkedIn export** from those bullets (and Role/context / Tech used).
   - **ATS export:** plain-text experience block only — no tables, PR numbers, repo paths, or markdown inside the filled block; 3–6 bullets; comma-separated Skills line.
   - **LinkedIn export:** same facts as ATS; optional 1–2 sentence overview; bullets may be slightly longer; no internal jargon.
   - Record key PRs in **Raw sources** (reference section — never copy into export blocks).
   - Update the doc **Changelog** with today's fill date and repo.

5. **Never invent metrics**, confidential detail, or ownership you cannot back with PRs/commits.

### 5. Update index

After writing, update `{RESUME_ROOT}/INDEX.md`:

1. Create the file from the stub below if it does not exist.
2. **Prepend** a new row to the table (newest first) with:
   - **Date** — `{YYYY-MM-DD}`
   - **Repository** — `{REPO_FULL}`
   - **Slug** — `{slug}`
   - **File** — relative path from `{RESUME_ROOT}/` (e.g. `by-repo/origin-apps/RESUME_EXPERIENCE_2026-08-04_origin-apps.md`)
   - **Feature clusters** — count of `### Feature:` sections written
3. Do not remove existing rows.

**INDEX.md stub** (use when creating the file for the first time):

```markdown
# Resume experience index

Personal archive of per-repo CV source docs generated by `/generate-resume`.

| Date | Repository | Slug | File | Feature clusters |
| ---- | ---------- | ---- | ---- | ---------------- |
```

### 6. Report to the user

- Primary output file path (`{RESUME_DIR}/…`)
- Repo copy path if `--in-repo` was used
- Index updated at `{RESUME_ROOT}/INDEX.md`
- Identity used (GitHub login + author filters)
- Brief count: merged PRs considered, feature clusters written
- Remind user: copy **ATS export** / **LinkedIn export** into their master CV or profile (this file is not ATS-submittable)
- Anything left as `unknown — needs estimate`

## Rules

- Never mutate `RESUME_TEMPLATE.md`.
- Always a **new dated file** in `{RESUME_DIR}/`.
- Attribute only work matching the resolved author.
- Prefer product language over internal chore commits.
- One repo per output file; run again with another `{git-repo}` for other repos.
- Per-repo output is a **source doc** for one experience entry — not a full multi-page CV. Export blocks are for manual merge into a consolidated resume later.
- Default archive is this repo's `resume/` folder (gitignored). Use `--in-repo` only when the user wants a copy in the target repo.
