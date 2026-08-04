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
