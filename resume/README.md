# Resume archive

Generated CV source docs from `/generate-resume` and merged experience docs from `/merge-resume-experience`.

## Layout

```
resume/
  INDEX.md                 # per-repo run history (gitignored)
  EXPERIENCE_INDEX.md      # merged experience run history (gitignored)
  experiences/             # YAML manifests — one per employer/role (gitignored)
  by-repo/{slug}/          # Layer 1: one git repo → one source doc
  by-experience/{slug}/    # Layer 2: N repos → one ATS + LinkedIn export
```

## Workflow

1. **Per repo:** `/generate-resume {path}` → `by-repo/{slug}/RESUME_EXPERIENCE_{date}_{slug}.md`
2. **Group repos:** copy `experiences/example.yml` → `experiences/{slug}.yml` and edit
3. **Merge:** `/merge-resume-experience {slug}` → `by-experience/{slug}/RESUME_EXPERIENCE_{date}_{slug}.md`
4. **Copy** ATS export and LinkedIn export into your CV or LinkedIn profile

See [experiences/README.md](experiences/README.md) for manifest setup.

These paths are **gitignored** except this README, `experiences/README.md`, and `experiences/example.yml`.
