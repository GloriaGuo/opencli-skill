# OpenCLI China-Focused Workflows

Use these templates when the task is centered on Chinese sites, Chinese-language research, creator workflows, recruiting workflows, or local desktop-agent tooling.

## Hot Lists And Trend Scanning

```bash
opencli bilibili hot --limit 10 -f json
opencli zhihu hot -f json
opencli weibo hot -f yaml
opencli jike feed -f yaml
opencli linux-do feed -f yaml
```

Use `json` when another agent or script will rank, summarize, or deduplicate the result.

## Search And Research

```bash
opencli zhihu search "Android 性能优化" -f json
opencli bilibili search "Jetpack Compose" --limit 10 -f json
opencli xiaohongshu search "AI Agent" --limit 10 -f yaml
opencli douban search "三体" -f yaml
opencli weread search "产品思维" -f yaml
```

Start with a small limit and expand only if the task needs broader recall.

## Content Export And Download

```bash
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --download-images
opencli weixin download --url "https://mp.weixin.qq.com/s/xxx" --output ./weixin
opencli xiaohongshu download abc123 --output ./xhs
opencli bilibili download BV1xxx --output ./bilibili
```

Treat failed downloads from browser-backed sites as login-state problems first. For Bilibili video downloads, install `yt-dlp`.

## Creator And Social Workflows

```bash
opencli xiaohongshu creator-profile -f json
opencli xiaohongshu creator-stats -f json
opencli xiaohongshu creator-notes --limit 10 -f json
opencli jike notifications -f yaml
opencli jike post "今天完成了 OpenCLI skill 的整理"
```

Prefer read-only inspection before publishing or sending anything.

## Recruiting And Job Search

```bash
opencli boss search "Android" --limit 10 -f json
opencli boss recommend --limit 10 -f json
opencli boss detail <job-id> -f yaml
opencli boss greet <job-id>
opencli boss chatlist -f yaml
```

Read job details before greeting. Keep any outbound action explicit and deliberate.

## Finance And Market Checks

```bash
opencli xueqiu hot-stock -f json
opencli xueqiu stock TSLA -f yaml
opencli sinafinance news -f yaml
```

Use this as a fast market snapshot, not as a substitute for formal investment due diligence.

## Desktop Agent Workflows

### Cursor

```bash
opencli cursor read
opencli cursor history
opencli cursor model
opencli cursor ask "总结当前工作区最近一次对话"
opencli cursor export
```

### Codex

```bash
opencli codex read
opencli codex history
opencli codex model
opencli codex ask "概括这个线程最近的实现进度"
opencli codex export
```

### ChatGPT / Doubao / Notion

```bash
opencli chatgpt read
opencli doubao-app read
opencli notion search "weekly review"
opencli notion read <page-id>
```

Start with `read`, `history`, or `model` on desktop adapters so you can confirm the app is reachable before using `send` or `ask`.

## Suggested Patterns

- For Chinese content monitoring: combine `bilibili`, `zhihu`, `weibo`, and `jike`.
- For creator research: combine `xiaohongshu`, `bilibili`, and `zhihu`.
- For job workflows: combine `boss`, `zhihu search`, and `weixin download` when collecting public context.
- For local agent orchestration: combine `codex`, `cursor`, and `notion`.
