# OpenCLI Command Reference

## Setup And Discovery

```bash
npm install -g @jackwener/opencli
opencli doctor
opencli list
opencli list -f yaml
```

Use `opencli list -f yaml` before guessing adapter names or subcommands.

## Output Modes

```bash
opencli bilibili hot -f json
opencli bilibili hot -f yaml
opencli bilibili hot -f table
opencli bilibili hot -v
```

- `json`: best for agents and pipes
- `yaml`: best for quick human review
- `table`: best for interactive terminal reading
- `md` and `csv`: use when the downstream consumer needs those formats
- `-v`: show pipeline details for debugging

## Public Commands

These usually do not need Chrome login.

```bash
opencli hackernews top --limit 5
opencli wikipedia summary "OpenAI"
opencli arxiv search "reasoning models"
opencli google news --limit 10
```

## Browser Commands

These usually require Chrome to be running and already logged in.

```bash
opencli bilibili hot --limit 5 -f json
opencli zhihu hot -f yaml
opencli xiaohongshu search "agent" --limit 10
opencli twitter search "opencli" --limit 10
opencli reddit hot --limit 10
opencli boss search "Android" --limit 10
```

Run `opencli doctor` first if the extension or browser bridge may be unhealthy.

## Desktop Adapters

Inspect the adapter help or registry first, then start with read-only commands.

```bash
opencli cursor read
opencli cursor history
opencli cursor model
opencli codex read
opencli codex history
opencli codex model
opencli notion search "weekly plan"
opencli discord-app channels
opencli chatgpt read
```

If the adapter supports `send`, `ask`, or `new`, prefer those over brittle UI automation.

## External CLI Passthrough

OpenCLI can proxy local CLIs through one discovery surface.

```bash
opencli gh pr list --limit 5
opencli docker ps
opencli obsidian search query="AI"
opencli gws docs list
opencli register mycli
```

## Downloads

```bash
opencli xiaohongshu download abc123 --output ./xhs
opencli bilibili download BV1xxx --output ./bilibili
opencli twitter download elonmusk --limit 20 --output ./twitter
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli weixin download --url "https://mp.weixin.qq.com/s/xxx" --output ./weixin
```

Install `yt-dlp` for streaming video downloads such as Bilibili.

## Plugins And Custom Tools

```bash
opencli plugin install github:user/opencli-plugin-my-tool
opencli plugin list
opencli plugin update --all
opencli plugin uninstall my-tool
opencli register mycli
```

## Adapter Development

```bash
opencli explore https://example.com --site mysite
opencli synthesize mysite
opencli generate https://example.com --goal "hot"
opencli cascade https://api.example.com/data
```

Exploration artifacts are written under `.opencli/explore/<site>/`.
