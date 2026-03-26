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

## Config File

OpenClaw skill loading is controlled through:

```text
~/.openclaw/openclaw.json
```

### Minimal watch setup

```json5
{
  skills: {
    load: {
      watch: true,
      watchDebounceMs: 250,
    },
    entries: {
      opencli: {
        enabled: true,
      },
    },
  },
}
```

### Load from extra directories

```json5
{
  skills: {
    load: {
      extraDirs: [
        "/Users/you/skill-packs",
      ],
      watch: true,
      watchDebounceMs: 250,
    },
  },
}
```

### What the config means

- `watch: true`: reload skill file changes automatically
- `watchDebounceMs: 250`: avoid reloading too often during rapid edits
- `entries.opencli.enabled: true`: explicitly enable this skill
- `entries.opencli.enabled: false`: disable it without deleting the folder
- `extraDirs`: mount one or more additional skill directories

## Reload Behavior

If OpenClaw has skill watching enabled, skill updates are reloaded automatically. If not, restart the relevant agent session or gateway after installing or updating the skill.

## Recommended Install Patterns

### Shared skill for all agents

Use `~/.openclaw/skills/opencli` when multiple agents should see the same skill.

### Workspace-local skill

Use `./skills/opencli` when the skill should apply only to one project or one agent workspace.

### Central skill pack

Use `skills.load.extraDirs` when you maintain several reusable skills outside the default OpenClaw directories.

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
