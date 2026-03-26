# OpenCLI Practical Scenarios Manual

This manual is for real tasks, not feature tours. Use it to decide when OpenCLI is the right tool, how to start safely, and which command patterns are worth memorizing.

## Choose OpenCLI Or Something Else

Use OpenCLI when:

- The site or desktop app is already supported
- You want deterministic structured output
- You want to reuse your existing Chrome login state
- You need a terminal entrypoint that another agent or script can call
- You need to automate Electron apps such as Codex, Cursor, ChatGPT, or Notion

Do not reach for OpenCLI first when:

- The site is unknown and has no adapter yet
- The task is one-off exploration with many branching UI decisions
- You need full browser test coverage, tracing, assertions, and CI-grade E2E workflows

Rule of thumb:

- OpenCLI: known target, repeatable workflow, stable output
- Browser agent: unknown target, exploratory workflow, dynamic decisions
- Playwright: custom programmable automation and testing

## Preflight Checklist

Before any non-public command, check these in order:

```bash
opencli doctor
opencli list -f yaml
```

Then verify the correct runtime:

- Public command: no browser login needed
- Browser command: Chrome must be open and logged in to the target site
- Desktop adapter: the app must be open and on a readable screen
- Download command: adapter support and any helper tools such as `yt-dlp` must be present

Safe defaults:

- Use `-f json` when another agent or script will consume the result
- Use `--limit 5` or `--limit 10` for the first read
- Start with read-only commands before any outbound action

## Scenario 1: Daily Trend Monitoring

Goal: pull hot topics from multiple sources and feed them into another summarization step.

Start with low-cost public commands:

```bash
opencli hackernews top --limit 10 -f json
opencli google news --limit 10 -f json
opencli wikipedia trending -f json
```

Add browser-backed Chinese platforms after `opencli doctor` passes:

```bash
opencli bilibili hot --limit 10 -f json
opencli zhihu hot -f json
opencli weibo hot -f json
```

Why OpenCLI works well here:

- Every source already has a fixed command surface
- Output is easy to merge because it is structured
- You avoid re-navigating web UIs with an LLM every run

## Scenario 2: Chinese Content Research

Goal: research a topic across search-heavy Chinese platforms.

Example: Android performance optimization research.

```bash
opencli zhihu search "Android 性能优化" -f json
opencli bilibili search "Android 性能优化" --limit 10 -f json
opencli xiaohongshu search "Android 性能优化" --limit 10 -f yaml
opencli weread search "性能优化" -f yaml
```

Recommended pattern:

1. Use search commands with a small limit
2. Skim titles and authors first
3. Expand only the source that looks promising
4. Export or download only when you know the source is valuable

This is where OpenCLI beats generic browser agents: the task is repetitive and search-oriented, so fixed commands are faster than page-by-page reasoning.

## Scenario 3: Export Articles And Download Media

Goal: archive knowledge or media locally for later processing.

Examples:

```bash
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --download-images
opencli weixin download --url "https://mp.weixin.qq.com/s/xxx" --output ./weixin
opencli xiaohongshu download abc123 --output ./xhs
opencli bilibili download BV1xxx --output ./bilibili
```

Operational advice:

- Treat browser-backed download failures as login-state problems first
- For Bilibili and similar video downloads, install `yt-dlp` before debugging anything else
- Always set an explicit output folder so downstream tooling has a stable path

## Scenario 4: Creator Research And Competitive Scanning

Goal: inspect what creators are publishing and how they are performing.

Examples:

```bash
opencli xiaohongshu creator-profile -f json
opencli xiaohongshu creator-stats -f json
opencli xiaohongshu creator-notes --limit 10 -f json
opencli bilibili user-videos <uid> --limit 10 -f json
opencli bilibili ranking --limit 10 -f json
```

Recommended pattern:

1. Read profile or overview metrics first
2. Pull a small set of recent notes or videos
3. Only then decide whether to download or summarize

OpenCLI is strong here because creators and social platforms expose repetitive shapes: feed, profile, notes, metrics, downloads.

## Scenario 5: Recruiting And Outreach

Goal: search jobs, read detail pages, and only then take action.

Examples:

```bash
opencli boss search "Android" --limit 10 -f json
opencli boss recommend --limit 10 -f json
opencli boss detail <job-id> -f yaml
opencli boss chatlist -f yaml
```

If you plan to send greetings or messages:

- Inspect the job detail first
- Confirm the role is relevant
- Keep outbound actions explicit and human-approved

OpenCLI helps because job search is a mix of search, recommendation, detail reads, and occasional outbound actions, all against a site that usually requires login.

## Scenario 6: Desktop Agent Orchestration

Goal: treat desktop AI apps as callable tools from the terminal.

Examples for Codex:

```bash
opencli codex --help
opencli codex read -f json
opencli codex history -f json
opencli codex model -f json
opencli codex ask "概括这个线程最近的实现进度"
opencli codex export
```

Examples for Cursor:

```bash
opencli cursor --help
opencli cursor read -f json
opencli cursor history -f json
opencli cursor model -f json
opencli cursor ask "总结当前工作区最近一次对话"
opencli cursor export
```

Important: desktop adapters are not perfectly uniform. Always check the adapter help or `opencli list -f yaml` before assuming a subcommand exists.

Recommended order:

1. `--help` or registry lookup
2. `read`, `history`, or `model`
3. `ask` or `send`
4. `export` for durable artifacts

Use OpenCLI here when you want terminal control over apps that would otherwise require brittle UI scripting.

## Scenario 7: Unified CLI Hub For Other Tools

Goal: give an agent one command surface for common local tools.

Examples:

```bash
opencli gh pr list --limit 5
opencli docker ps
opencli obsidian search query="AI"
opencli gws docs list
opencli register mycli
```

This is useful when the agent should first discover tools via `opencli list`, then call everything through one namespace instead of remembering many binaries.

## Scenario 8: Building A New Adapter

Goal: add support for a site that OpenCLI does not cover yet.

Use the exploration workflow:

```bash
opencli explore https://example.com --site mysite
opencli synthesize mysite
opencli generate https://example.com --goal "hot"
opencli cascade https://api.example.com/data
```

Recommended workflow:

1. Use `explore` to inspect network behavior and page structure
2. Use `cascade` if auth strategy is unclear
3. Use `synthesize` to build the adapter from exploration artifacts
4. Use `generate` when you want the one-shot path

This is where OpenCLI overlaps with browser tooling, but in a very different way: it is trying to create a reusable command surface, not just finish one browsing session.

## Common Mistakes

### Treating OpenCLI like a universal browser agent

Wrong expectation: it should figure out any random UI on the fly.

Better expectation: it works best when an adapter already exists or you are explicitly creating one.

### Forgetting login state

Wrong move: debugging command flags before checking Chrome.

Better move: if the command is browser-backed, verify Chrome is open, the site is logged in, and `opencli doctor` is green.

### Starting with write actions

Wrong move: greeting a job post, posting content, or sending a message before reading context.

Better move: start with search, detail, read, history, or export.

### Assuming desktop adapters share one schema

Wrong move: expecting `status` or `screenshot` everywhere.

Better move: check adapter help for the exact command surface of the installed OpenCLI version.

## Minimal Memorization Set

If you only memorize a few commands, memorize these:

```bash
opencli doctor
opencli list -f yaml
opencli <site> <command> -f json
opencli <desktop-app> --help
opencli <desktop-app> read -f json
opencli <desktop-app> ask "<prompt>"
```

Everything else can be rediscovered from there.
