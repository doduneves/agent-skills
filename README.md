# agent-skills

Personal [Agent Skills](https://agentskills.io) for coding agents (Cursor, Claude Code, Codex, etc.).

## Skills

| Skill | Description |
| ----- | ----------- |
| [generate-resume](skills/generate-resume/) | Build dated CV experience markdown from a git repo's merged PRs and commits |

## Install (Cursor)

```bash
git clone git@github.com:YOUR_USER/agent-skills.git ~/Apps/dodu/agent-skills
ln -sf ~/Apps/dodu/agent-skills/skills/generate-resume ~/.cursor/skills/generate-resume
```

Restart Cursor (or reload) after symlinking.

## Install (Claude Code)

```bash
ln -sf ~/Apps/dodu/agent-skills/skills/generate-resume ~/.claude/skills/generate-resume
```

## Resume archive

`/generate-resume` writes output to `resume/` in this repo (gitignored). See [resume/README.md](resume/README.md).

Run the skill from the **target project** workspace — e.g. open `building-blocks` in Cursor, then invoke `/generate-resume`. Evidence comes from that repo; the filled markdown is saved here.
