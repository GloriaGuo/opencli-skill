# OpenCLI Skill

English | [中文](#中文说明)

An agent skill for working with [OpenCLI](https://github.com/jackwener/opencli): command discovery, browser-login reuse, desktop-app adapters, downloads, troubleshooting, and adapter-generation workflows.

Repository: [GloriaGuo/opencli-skill](https://github.com/GloriaGuo/opencli-skill)

## Why This Exists

OpenCLI is powerful, but it spans several different modes:

- public data commands
- browser-backed site adapters
- Electron desktop adapters
- external CLI passthrough
- adapter generation workflows

This skill gives AI coding agents a read-first, agent-safe operating path so they can:

- discover the installed OpenCLI surface before guessing
- choose the right adapter for a site or app
- prefer structured output such as JSON
- troubleshoot Browser Bridge, login-state, and installed-version mismatches

## What This Skill Covers

- choosing OpenCLI vs browser agents vs Playwright
- discovering commands safely with `opencli list -f yaml`
- running browser-backed site commands with Chrome login reuse
- running desktop adapters such as Codex and Cursor
- using OpenCLI as a passthrough hub for local CLIs such as `gh` and `docker`
- downloading media and exported articles
- troubleshooting daemon, extension, auth, and command-surface issues
- generating new OpenCLI adapters

## Repository Structure

- [SKILL.md](./SKILL.md): main skill entrypoint
- [agents/openai.yaml](./agents/openai.yaml): UI metadata
- [references/agent-playbook.md](./references/agent-playbook.md): shortest agent-first path
- [references/commands.md](./references/commands.md): common commands and output modes
- [references/china-workflows.md](./references/china-workflows.md): China-focused site and desktop examples
- [references/practical-scenarios.md](./references/practical-scenarios.md): detailed scenario manual
- [references/troubleshooting.md](./references/troubleshooting.md): common failures and fixes
- [examples/README.md](./examples/README.md): example prompts and command recipes
- [examples/claude-code.md](./examples/claude-code.md): Claude Code installation and prompt examples
- [examples/openclaw.md](./examples/openclaw.md): OpenClaw installation and usage examples
- [scripts/validate_skill.py](./scripts/validate_skill.py): portable validation script for CI and local checks

## Install

### Codex

Clone or copy the repo into your Codex skills directory:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

Or copy the folder contents manually into:

```bash
${CODEX_HOME:-$HOME/.codex}/skills/opencli
```

## Use In Codex

Once installed, ask Codex naturally or invoke the skill explicitly:

```text
Use $opencli to inspect the installed OpenCLI surface and fetch the Bilibili hot list.
```

```text
Use $opencli to troubleshoot why my Zhihu command returns empty data.
```

Codex-oriented installation path:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

## Use In Claude Code

Claude Code skills typically live under `~/.claude/skills`. Copy or clone this repository there:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.claude/skills/opencli
```

Then ask Claude Code in natural language or reference the skill explicitly:

```text
Use $opencli to run a safe read-first OpenCLI workflow for this task.
```

```text
Use $opencli to inspect the local OpenCLI setup and tell me whether the Codex and Cursor adapters are available.
```

Claude Code usage examples are collected in [examples/claude-code.md](./examples/claude-code.md).

## Use In OpenClaw

OpenClaw can load shared skills from `~/.openclaw/skills` or workspace-specific skills from `<workspace>/skills`.

Shared install for all agents:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.openclaw/skills/opencli
```

Workspace-specific install:

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ./skills/opencli
```

If skill watching is enabled in OpenClaw, updates are auto-reloaded. Otherwise restart the agent session or gateway after installing.

Example prompts:

```text
Use $opencli to inspect the installed OpenCLI surface and run a read-first workflow for Bilibili.
```

```text
Use $opencli to diagnose why my browser-backed OpenCLI command is returning empty data.
```

OpenClaw-specific notes and examples are collected in [examples/openclaw.md](./examples/openclaw.md).

## Recommended OpenCLI Setup

```bash
npm install -g @jackwener/opencli
opencli doctor
opencli list -f yaml
```

For browser-backed commands:

- keep Chrome open
- log into the target site in Chrome
- install and enable the OpenCLI Browser Bridge extension

## Quick Start

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
opencli cursor ask "Summarize the latest workspace conversation"
```

Downloads:

```bash
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli bilibili download BV1xxx --output ./bilibili
```

## Design Principles

- prefer deterministic command output over ad-hoc browsing
- start with read-only commands
- trust the installed CLI surface over stale README assumptions
- use `json` for agents and `yaml` for humans
- treat browser login state as a first-class dependency

## Validation

```bash
python3 scripts/validate_skill.py
```

## 中文说明

这是一个面向多种 AI agent 环境的 `OpenCLI` skill，目标不是重复官方 README，而是让 agent 更稳定地完成真实任务：

- 先发现本机已安装的命令面
- 再选择站点 / 桌面应用 / CLI passthrough / adapter generation 路径
- 默认走只读优先、结构化输出优先
- 遇到浏览器命令时优先检查 Chrome 登录态和 Browser Bridge

## 这个 skill 解决什么问题

- 帮 agent 判断什么时候该用 OpenCLI，而不是 browser agent 或 Playwright
- 帮 agent 在动手前先跑 `opencli list -f yaml`
- 帮 agent 对 B 站、知乎、小红书、微信公众号、Boss、Cursor、Codex 等场景选对命令
- 帮 agent 在安装版本和 README 不一致时，以本机实际命令面为准
- 帮 agent 在写操作前先做读操作，降低误操作风险

## 仓库内容

- [SKILL.md](./SKILL.md)：主 skill
- [agents/openai.yaml](./agents/openai.yaml)：UI 元数据
- [references/agent-playbook.md](./references/agent-playbook.md)：最短执行路径，偏 agent-first
- [references/commands.md](./references/commands.md)：常用命令和输出格式
- [references/china-workflows.md](./references/china-workflows.md)：中文平台和桌面应用示例
- [references/practical-scenarios.md](./references/practical-scenarios.md)：详细实用场景手册
- [references/troubleshooting.md](./references/troubleshooting.md)：常见故障排查
- [examples/README.md](./examples/README.md)：可直接照抄的 prompt 和命令示例
- [examples/claude-code.md](./examples/claude-code.md)：Claude Code 安装与调用示例
- [examples/openclaw.md](./examples/openclaw.md)：OpenClaw 安装与调用示例
- [scripts/validate_skill.py](./scripts/validate_skill.py)：本地与 CI 通用的校验脚本

## 安装方式

### Codex

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

或者手动复制到：

```bash
${CODEX_HOME:-$HOME/.codex}/skills/opencli
```

## 在 Codex 里怎么用

安装后，可以自然描述任务，也可以显式提到 `$opencli`：

```text
Use $opencli to inspect the installed OpenCLI surface and fetch the Bilibili hot list.
```

```text
Use $opencli to troubleshoot why my Zhihu command returns empty data.
```

Codex 默认安装路径：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

## 在 Claude Code 里怎么用

Claude Code 的 skill 通常放在 `~/.claude/skills` 下，可以这样安装：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.claude/skills/opencli
```

然后直接用自然语言，或者显式提到 `$opencli`：

```text
Use $opencli to run a safe read-first OpenCLI workflow for this task.
```

```text
Use $opencli to inspect the local OpenCLI setup and tell me whether the Codex and Cursor adapters are available.
```

更完整的 Claude Code 示例见 [examples/claude-code.md](./examples/claude-code.md)。

## 在 OpenClaw 里怎么用

OpenClaw 可以从 `~/.openclaw/skills` 读取共享 skill，也可以从当前 workspace 的 `./skills` 读取工作区级 skill。

给所有 agent 共用的安装方式：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.openclaw/skills/opencli
```

只给当前 workspace 使用：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ./skills/opencli
```

如果 OpenClaw 开启了 skills watch，修改后会自动刷新；否则安装后重启对应 agent 会话或 gateway。

OpenClaw 里可以这样触发：

```text
Use $opencli to inspect the installed OpenCLI surface and run a read-first workflow for Bilibili.
```

```text
Use $opencli to diagnose why my browser-backed OpenCLI command is returning empty data.
```

更完整的 OpenClaw 示例见 [examples/openclaw.md](./examples/openclaw.md)。

## 推荐的 OpenCLI 环境准备

```bash
npm install -g @jackwener/opencli
opencli doctor
opencli list -f yaml
```

如果要跑浏览器类命令：

- 保持 Chrome 打开
- 先在 Chrome 中登录目标网站
- 安装并启用 OpenCLI Browser Bridge 扩展

## 快速示例

公共数据：

```bash
opencli hackernews top --limit 5 -f json
```

浏览器站点：

```bash
opencli bilibili hot --limit 5 -f json
opencli zhihu search "Android 性能优化" -f json
```

桌面应用：

```bash
opencli codex --help
opencli codex read -f json
opencli cursor read -f json
opencli cursor ask "总结当前工作区最近一次对话"
```

下载：

```bash
opencli zhihu download "https://zhuanlan.zhihu.com/p/xxx" --output ./zhihu
opencli bilibili download BV1xxx --output ./bilibili
```

## 设计原则

- 优先用确定性的命令输出，而不是临场网页推理
- 先读后写
- 以本机安装版本的命令面为准，不盲信旧文档
- agent 消费优先 `json`，人工查看优先 `yaml`
- 浏览器登录态是第一优先级依赖

## 校验

```bash
python3 scripts/validate_skill.py
```
