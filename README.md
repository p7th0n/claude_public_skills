# Claude Public Skills

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![claude-code](https://img.shields.io/badge/claude--code-blueviolet)](https://github.com/topics/claude-code)
[![skills](https://img.shields.io/badge/skills-blueviolet)](https://github.com/topics/skills)
[![ai-agents](https://img.shields.io/badge/ai--agents-blueviolet)](https://github.com/topics/ai-agents)

Shared [Claude Code](https://claude.com/claude-code) skills — packaged instructions Claude follows for a particular kind of task.

## Sharing strategy

- Clone this repo, e.g. to `~/Dev/claude_public_skills`.
- Symlink individual skill folders into `~/.claude/skills/` (see each skill's install note below).
- Pull changes when updates are available — the symlink picks them up automatically.

## What's a Claude Code skill?

A skill is a packaged set of instructions Claude Code follows for a specific kind of task — a `SKILL.md` file with a `description` that tells Claude when to load it, plus optional supporting files (templates, reference docs). Claude Code picks up any skill folder symlinked into `~/.claude/skills/`.

## Install

```
git clone <this-repo-url> ~/Dev/claude_public_skills
ln -s ~/Dev/claude_public_skills/<skill-name> ~/.claude/skills/<skill-name>
```

Repeat the `ln -s` for each skill you want. `git pull` picks up updates automatically since the symlink points straight at the repo.

## Skills

| Skill | Description |
| --- | --- |
| [`storymap`](storymap/SKILL.md) | Run a Jeff Patton-style user story mapping session to build shared understanding with stakeholders before writing a roadmap entry or spec proposal. |
| [`prose-style`](prose-style/SKILL.md) | Apply when writing or editing prose meant for humans to read — READMEs, docs, comments, commit messages, PR descriptions, user-facing text. |
