# OpenCLI Agent Playbook

This file is optimized for agent behavior. Read it when the user wants results through OpenCLI and you need a safe default path quickly.

## Trigger Phrases

Strong signals:

- "use opencli"
- "check what opencli can do"
- "run this through my browser login"
- "query Bilibili / Zhihu / Xiaohongshu / Weixin / Boss"
- "control Codex / Cursor / ChatGPT / Notion from terminal"
- "download this article/video with opencli"
- "generate an opencli adapter"

Weak but useful signals:

- The task needs deterministic CLI output instead of free-form browsing
- The user already has the site logged in in Chrome
- The task targets a known site repeatedly

## Default Action Order

Follow this order unless the user already gave an exact command:

1. `opencli doctor` if the task is browser-backed and health is unknown
2. `opencli list -f yaml` if the command surface is unknown
3. Use read-only commands first
4. Use `-f json` if another tool or model will consume the result
5. Expand scope only after a small first pass succeeds

## First Commands By Situation

### Browser-backed site

```bash
opencli doctor
opencli list -f yaml
opencli <site> <read-command> --limit 5 -f json
```

### Desktop adapter

```bash
opencli <app> --help
opencli <app> read -f json
opencli <app> history -f json
opencli <app> model -f json
```

### External CLI passthrough

```bash
opencli <tool> ...
```

### Adapter development

```bash
opencli explore <url> --site <name>
opencli synthesize <name>
opencli generate <url> --goal "<goal>"
```

## Safe Defaults

- Start with `--limit 5` or `--limit 10`
- Prefer `json` for machine consumption
- Prefer `yaml` when the human will scan raw output
- Do not begin with write actions such as `post`, `reply`, `publish`, `greet`, `send`, or `follow`
- For browser-backed commands, suspect login state before suspecting the adapter

## Write-Action Rule

Before any write-capable command:

1. Read current state first
2. Confirm target identity or content source
3. Keep the outbound command explicit in the transcript

Examples:

- Before `boss greet`, run `boss detail`
- Before `jike post`, inspect timeline or notifications if relevant
- Before desktop `send` or `ask`, confirm the app and active context with `read`, `history`, or `model`

## Installed-Version Rule

Do not assume the README equals the installed surface area.

When in doubt:

```bash
opencli <adapter> --help
opencli list -f yaml
```

Trust the installed CLI over stale docs.
