# OpenCLI Skill

A Codex skill for working with [OpenCLI](https://github.com/jackwener/opencli): installation checks, command discovery, browser-login reuse, desktop-app adapters, downloads, troubleshooting, and adapter-generation workflows.

Repository: [GloriaGuo/opencli-skill](https://github.com/GloriaGuo/opencli-skill)

## What This Skill Covers

- Choosing when OpenCLI is a better fit than browser agents or Playwright
- Discovering commands safely with `opencli list -f yaml`
- Running browser-backed site commands with Chrome login reuse
- Running desktop adapters such as Codex and Cursor
- Using OpenCLI as a passthrough hub for local CLIs such as `gh` and `docker`
- Downloading media and exported articles
- Troubleshooting the Browser Bridge, daemon, auth, and installed-command mismatches
- Generating new OpenCLI adapters

## Files

- [SKILL.md](./SKILL.md): main skill entrypoint
- [agents/openai.yaml](./agents/openai.yaml): UI metadata for the skill
- [references/agent-playbook.md](./references/agent-playbook.md): fastest agent-first operating path
- [references/commands.md](./references/commands.md): common commands and output patterns
- [references/china-workflows.md](./references/china-workflows.md): China-focused site and desktop examples
- [references/practical-scenarios.md](./references/practical-scenarios.md): detailed use-case manual
- [references/troubleshooting.md](./references/troubleshooting.md): common failure signatures and fixes

## Install

Clone or copy the repo into your Codex skills directory:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

Or copy the folder contents manually into:

```bash
${CODEX_HOME:-$HOME/.codex}/skills/opencli
```

## Recommended OpenCLI Setup

```bash
npm install -g @jackwener/opencli
opencli doctor
opencli list -f yaml
```

For browser-backed commands:

- Keep Chrome open
- Log into the target site in Chrome
- Install and enable the OpenCLI Browser Bridge extension

## Quick Examples

Public data:

```bash
opencli hackernews top --limit 5 -f json
```

Browser-backed data:

```bash
opencli bilibili hot --limit 5 -f json
opencli zhihu search "Android 性能优化" -f json
```

Desktop adapters:

```bash
opencli codex --help
opencli codex read -f json
opencli cursor read -f json
opencli cursor ask "总结当前工作区最近一次对话"
```

Downloads:

```bash
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli bilibili download BV1xxx --output ./bilibili
```

## Design Principles

- Prefer deterministic command output over ad-hoc browsing
- Start with read-only commands
- Trust the installed CLI surface over stale README assumptions
- Use `json` for agents and `yaml` for humans
- Treat browser login state as a first-class dependency

## Validation

This skill can be validated with:

```bash
python3 /Users/ruiguo/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```
