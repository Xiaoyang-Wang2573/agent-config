# Agent Config

AI Agent 客户端配置文件集合，支持多种主流 AI 编程助手。

## 支持的客户端

| 客户端 | Agents | Commands | Skills | MCP | Plugins | Hooks |
|--------|--------|----------|--------|-----|---------|-------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| [Codex CLI](https://github.com/openai/codex) | ✓ | ✗ | ✗ | ✓ | ✗ | ✗ |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | ✗ | ✗ | ✓ | ✗ | ✗ | ✗ |
| [OpenCode](https://opencode.ai/) | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |

## 目录结构

```
agent-config/
├── agents/      # AI 代理定义
├── commands/    # 斜杠命令（仅 Claude Code）
├── skills/      # 技能包
├── mcp/         # MCP 服务器配置
├── plugins/     # 插件配置
└── hooks/       # 钩子配置（仅 Claude Code）
```

## 快速开始

### Claude Code

```bash
# Linux/macOS
cp -r agents/* ~/.claude/agents/
cp -r commands/* ~/.claude/commands/
cp -r skills/* ~/.claude/skills/

# Windows (PowerShell)
Copy-Item -Recurse agents\* $env:USERPROFILE\.claude\agents\
Copy-Item -Recurse commands\* $env:USERPROFILE\.claude\commands\
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

### Codex CLI

```bash
# 将 agents 内容合并到 AGENTS.md
cat agents/**/*.md >> ~/.codex/AGENTS.md

# 或复制到项目根目录
cp agents/**/*.md ./AGENTS.md
```

### Gemini CLI

```bash
# Linux/macOS
cp -r skills/* ~/.gemini/skills/

# 或使用命令安装
gemini skills install <skill-path>
```

### OpenCode

```bash
# Linux/macOS
cp -r agents/* ~/.config/opencode/agents/
cp -r skills/* ~/.config/opencode/skills/

# Windows (PowerShell)
Copy-Item -Recurse agents\* $env:APPDATA\opencode\agents\
Copy-Item -Recurse skills\* $env:APPDATA\opencode\skills\
```

## 各目录说明

| 目录 | 说明 | 详细文档 |
|------|------|----------|
| [agents/](./agents/) | AI 代理，定义专门的角色和能力 | [agents/README.md](./agents/README.md) |
| [commands/](./commands/) | 斜杠命令，快速执行特定任务 | [commands/README.md](./commands/README.md) |
| [skills/](./skills/) | 技能包，包含指令和资源的完整功能模块 | [skills/README.md](./skills/README.md) |
| [mcp/](./mcp/) | MCP 服务器配置，连接外部工具和服务 | [mcp/README.md](./mcp/README.md) |
| [plugins/](./plugins/) | 插件，扩展客户端功能 | [plugins/README.md](./plugins/README.md) |
| [hooks/](./hooks/) | 钩子，在特定事件时自动执行操作 | [hooks/README.md](./hooks/README.md) |

## 常见问题

### Q: 配置文件放在哪里？

每个客户端有不同的配置目录：

| 客户端 | 全局配置目录 | 项目配置目录 |
|--------|-------------|-------------|
| Claude Code | `~/.claude/` | `.claude/` |
| Codex CLI | `~/.codex/` | 项目根目录 |
| Gemini CLI | `~/.gemini/` | `.gemini/` |
| OpenCode | `~/.config/opencode/` | `.opencode/` |

### Q: 如何知道某个功能是否被我的客户端支持？

每个目录的 README.md 开头都会标注支持的客户端列表。

### Q: 可以同时使用多个客户端吗？

可以。不同客户端的配置目录是独立的，互不影响。

## 参考链接

- [Claude Code 文档](https://docs.anthropic.com/en/docs/claude-code)
- [Codex CLI 文档](https://github.com/openai/codex)
- [Gemini CLI 文档](https://github.com/google-gemini/gemini-cli)
- [OpenCode 文档](https://opencode.ai/docs/)
- [MCP 协议](https://modelcontextprotocol.io/)

## 许可证

MIT License
