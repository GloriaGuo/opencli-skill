# OpenClaw Usage

This document shows how to install and use the OpenCLI skill in OpenClaw.

## Install Locations

OpenClaw supports multiple skill locations:

- shared skills: `~/.openclaw/skills`
- workspace-specific skills: `<workspace>/skills`

Shared install:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.openclaw/skills/opencli
```

Workspace install:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ./skills/opencli
```

## Reload Behavior

If OpenClaw has skill watching enabled, skill updates are reloaded automatically. If not, restart the relevant agent session or gateway after installing or updating the skill.

## Example Prompts

### Browser-backed sites

```text
Use $opencli to inspect the installed OpenCLI surface and fetch the top 10 Bilibili hot videos as JSON.
```

```text
Use $opencli to search Zhihu for "Android 性能优化" and summarize the top results.
```

### Desktop adapters

```text
Use $opencli to inspect the local OpenCLI setup and tell me whether the Codex and Cursor adapters are available.
```

```text
Use $opencli to read the current Cursor conversation and summarize the latest work.
```

### Troubleshooting

```text
Use $opencli to diagnose why my browser-backed OpenCLI command is returning empty data.
```

```text
Use $opencli to compare the installed OpenCLI command surface with the README and tell me what is actually available.
```

## Recommended Pattern

Inside OpenClaw, the safest default remains:

1. mention `$opencli`
2. ask for discovery first
3. prefer read-first behavior
4. request `json` if another tool or agent will consume the output
