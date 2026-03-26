# OpenCLI Skill

[English](./README.md) | 中文

一个面向多种 AI agent 环境的 OpenCLI skill，帮助 agent 用更稳定、可发现、只读优先的方式调用 OpenCLI。

仓库地址：[GloriaGuo/opencli-skill](https://github.com/GloriaGuo/opencli-skill)

## 这个 skill 是做什么的

OpenCLI 本身覆盖了很多能力：

- 公共数据命令
- 浏览器登录态复用的网站适配器
- Electron 桌面应用适配器
- 本地 CLI passthrough
- 站点适配器生成

这个 skill 的目标不是重复官方 README，而是让 agent 在真实任务里更稳定地做对这些事：

- 先发现本机实际安装的命令面
- 再选择正确的站点、桌面应用或 CLI 路径
- 优先使用结构化输出
- 在浏览器场景里优先检查 Browser Bridge 和登录态
- 在可能写入的场景里默认先读后写

## 覆盖内容

- OpenCLI 与 browser agent / Playwright 的选型
- `opencli list -f yaml` 的发现式使用方式
- Chrome 登录态复用的网站命令
- Codex、Cursor 等桌面应用适配器
- `gh`、`docker` 一类的 passthrough CLI
- 下载文章、图片、视频
- OpenCLI adapter 生成与排障
- OpenClaw / Claude Code / Codex 的安装与调用方式

## 仓库结构

- [SKILL.md](./SKILL.md)：主 skill
- [agents/openai.yaml](./agents/openai.yaml)：UI 元数据
- [references/agent-playbook.md](./references/agent-playbook.md)：最短 agent-first 路径
- [references/commands.md](./references/commands.md)：常用命令与输出格式
- [references/china-workflows.md](./references/china-workflows.md)：中文平台和桌面应用示例
- [references/practical-scenarios.md](./references/practical-scenarios.md)：详细实用场景手册
- [references/troubleshooting.md](./references/troubleshooting.md)：排障说明
- [examples/README.md](./examples/README.md)：示例入口
- [examples/claude-code.md](./examples/claude-code.md)：Claude Code 用法
- [examples/openclaw.md](./examples/openclaw.md)：OpenClaw 用法
- [examples/openclaw-prompts.md](./examples/openclaw-prompts.md)：OpenClaw prompt 模板
- [examples/openclaw.json5.example](./examples/openclaw.json5.example)：OpenClaw 配置示例

## 安装

### Codex

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/opencli"
```

### Claude Code

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.claude/skills/opencli
```

### OpenClaw

共享安装：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ~/.openclaw/skills/opencli
```

workspace 安装：

```bash
git clone git@github.com:GloriaGuo/opencli-skill.git ./skills/opencli
```

## 推荐的 OpenCLI 环境准备

```bash
npm install -g @jackwener/opencli
opencli doctor
opencli list -f yaml
```

如果要跑浏览器类命令：

- 保持 Chrome 打开
- 在 Chrome 中登录目标网站
- 安装并启用 OpenCLI Browser Bridge 扩展

## 在 OpenClaw 里推荐的配置

OpenClaw 配置文件：

```text
~/.openclaw/openclaw.json
```

一个最小可用配置：

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

如果你有一个集中维护的 skill 目录：

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
opencli codex read -f json
opencli cursor read -f json
opencli cursor ask "总结当前工作区最近一次对话"
```

## 推荐从这些文件开始看

- 想快速上手：看 [README.md](./README.md)
- 想看真实场景：看 [references/practical-scenarios.md](./references/practical-scenarios.md)
- 想看中文平台示例：看 [references/china-workflows.md](./references/china-workflows.md)
- 想在 OpenClaw 里用：看 [examples/openclaw.md](./examples/openclaw.md)
- 想直接抄 prompt：看 [examples/openclaw-prompts.md](./examples/openclaw-prompts.md)
