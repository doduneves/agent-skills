# Resume archive

Generated CV source docs from `/generate-resume` and merged experience docs from `/merge-resume-experience`.

## Layout

```
resume/
  profile.example.yml      # template — copy to profile.yml
  profile.yml              # your skills inventory (gitignored)
  INDEX.md                 # per-repo run history (gitignored)
  EXPERIENCE_INDEX.md      # merged experience run history (gitignored)
  experiences/             # YAML manifests — one per employer/role (gitignored)
  by-repo/{slug}/          # Layer 1: one git repo → one source doc
  by-experience/{slug}/    # Layer 2: N repos → one ATS + LinkedIn export
```

## Workflow

1. **Profile (once):** `cp profile.example.yml profile.yml` — add your full senior skill inventory
2. **Per repo:** `/generate-resume {path}` → `by-repo/{slug}/RESUME_EXPERIENCE_{date}_{slug}.md`
3. **Group repos:** copy `experiences/example.yml` → `experiences/{slug}.yml` and edit
4. **Merge:** `/merge-resume-experience {slug}` → `by-experience/{slug}/RESUME_EXPERIENCE_{date}_{slug}.md`
5. **Copy** ATS export and LinkedIn export into your CV or LinkedIn profile

See [experiences/README.md](experiences/README.md) for manifest setup.

## Personal profile (`profile.yml`)

Both skills load `{RESUME_ROOT}/profile.yml` when present:

- **Skills lines** — repo evidence first; profile enriches via category triggers (never invents stack)
- **Default role** — `Senior Software Engineer` etc. when manifest/repo omits title
- **Senior framing** — optional verb upgrades on bullets when evidence supports scope
- **Full skill list** — stays in profile for LinkedIn profile Skills / CV summary; `export_exclude` keeps AI/MCP off job exports

Committed: this README, `profile.example.yml`, `experiences/example.yml`, `experiences/README.md`. Everything else under `resume/` is gitignored personal archive.
