# Experience manifests

YAML files that group multiple repos into one employer/role entry for `/merge-resume-experience`.

## Setup

```bash
cp example.yml acme.yml
# edit paths, company, role, dates
```

Then run:

```text
/merge-resume-experience acme
/merge-resume-experience acme --refresh   # re-run /generate-resume on each repo first
```

Output lands in `by-experience/{slug}/` (gitignored). Copy **ATS export** and **LinkedIn export** from there into your CV or LinkedIn profile.

Manifest files in this folder are personal — gitignored except this README.

## Flat vs sectioned LinkedIn

| Manifest shape | ATS export | LinkedIn export |
| -------------- | ---------- | --------------- |
| Flat `repos` only | One flat bullet list | One flat bullet list |
| `projects` block | One flat list; client names inline in bullets | One entry with intro + project sections |

Use **sectioned** when one employer (e.g. Vinta Software) covers multiple client or internal projects. Example: `vinta.yml`.

```text
/merge-resume-experience vinta
```

- **CV / ATS:** copy the **ATS export** block (flat, deduped across all projects).
- **LinkedIn:** copy the **LinkedIn export (sectioned)** block into a single Experience entry at the employer company.

Single-project manifests (`everself.yml`, `origin.yml`, etc.) still work for standalone merges with flat LinkedIn exports.
