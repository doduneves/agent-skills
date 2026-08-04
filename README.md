# agent-skills

Personal [Agent Skills](https://agentskills.io) for coding agents (Cursor, Claude Code, Codex, etc.).

## Skills

| Skill | Description |
| ----- | ----------- |
| [generate-resume](skills/generate-resume/) | Build dated CV experience markdown from a git repo's merged PRs and commits |
| [merge-resume-experience](skills/merge-resume-experience/) | Merge multiple per-repo docs into one employer entry with ATS + LinkedIn exports |

## Install (Cursor)

```bash
git clone git@github.com:YOUR_USER/agent-skills.git ~/Apps/dodu/agent-skills
ln -sf ~/Apps/dodu/agent-skills/skills/generate-resume ~/.cursor/skills/generate-resume
ln -sf ~/Apps/dodu/agent-skills/skills/merge-resume-experience ~/.cursor/skills/merge-resume-experience
```

Restart Cursor (or reload) after symlinking.

## Install (Claude Code)

```bash
ln -sf ~/Apps/dodu/agent-skills/skills/generate-resume ~/.claude/skills/generate-resume
ln -sf ~/Apps/dodu/agent-skills/skills/merge-resume-experience ~/.claude/skills/merge-resume-experience
```

## Resume archive

`/generate-resume` writes per-repo output to `resume/by-repo/`. `/merge-resume-experience` combines multiple repos into `resume/by-experience/` using manifests in `resume/experiences/`. See [resume/README.md](resume/README.md).
