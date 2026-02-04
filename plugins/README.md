# Plugins（插件）

> ⚠️ **支持的客户端**: Claude Code、OpenCode
>
> Codex CLI、Gemini CLI 不支持 Plugins 功能。

## 什么是 Plugin

Plugin（插件）是扩展 AI 客户端功能的完整包。一个插件可以包含多种组件：Skills、Agents、Commands、Hooks、MCP 服务器等。

插件与单独的 Skill 或 Command 的区别：
- **Skill/Command** - 单一功能
- **Plugin** - 多个功能的集合，可能包含多个 Skills、Agents、Hooks 等

## 插件结构

```
my-plugin/
├── .claude-plugin/          # 元数据目录
│   └── plugin.json         # 插件清单（必需）
├── commands/               # 命令
│   └── my-command.md
├── agents/                 # 代理
│   └── my-agent.md
├── skills/                 # 技能
│   └── my-skill/
│       └── SKILL.md
├── hooks/                  # 钩子
│   └── hooks.json
├── .mcp.json              # MCP 服务器配置
├── scripts/               # 脚本
│   └── helper.sh
└── README.md
```

## plugin.json 格式

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "插件描述",
  "author": {
    "name": "作者名",
    "email": "email@example.com"
  },
  "homepage": "https://github.com/user/my-plugin",
  "repository": "https://github.com/user/my-plugin",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"]
}
```

## 各客户端使用方法

### Claude Code

**安装方式**：

方式一：从 marketplace 安装
```bash
claude plugin install <plugin-name>
claude plugin install <plugin-name>@<marketplace>
```

方式二：从本地安装
```bash
claude plugin install ./path/to/plugin
```

方式三：从 Git 仓库安装
```bash
claude plugin install https://github.com/user/plugin-repo
```

**安装范围**：
```bash
# 用户级（默认）- 所有项目可用
claude plugin install <plugin> --scope user

# 项目级 - 通过版本控制共享
claude plugin install <plugin> --scope project

# 本地级 - 仅当前项目，不提交到 git
claude plugin install <plugin> --scope local
```

**管理命令**：
```bash
# 列出已安装插件
claude plugin list

# 启用/禁用插件
claude plugin enable <plugin>
claude plugin disable <plugin>

# 更新插件
claude plugin update <plugin>

# 卸载插件
claude plugin uninstall <plugin>
```

**交互式命令**（在会话中）：
```
/plugin list
/plugin install <plugin>
/plugin enable <plugin>
/plugin disable <plugin>
```

---

### OpenCode

**安装路径**：
- 全局：`~/.config/opencode/plugins/`
- 项目：`.opencode/plugins/`

**安装方式**：
```bash
# 复制插件目录
cp -r my-plugin ~/.config/opencode/plugins/

# 或在配置文件中指定
```

**配置文件**（`opencode.json`）：
```json
{
  "plugins": {
    "my-plugin": {
      "enabled": true,
      "path": "./plugins/my-plugin"
    }
  }
}
```

---

### Codex CLI / Gemini CLI

❌ **不支持** - 这些客户端目前不支持 Plugins 功能。

## 示例插件

### 代码质量插件

目录结构：
```
code-quality/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── lint.md
│   └── format.md
├── agents/
│   └── code-reviewer.md
├── skills/
│   └── refactor-helper/
│       └── SKILL.md
├── hooks/
│   └── hooks.json
└── README.md
```

plugin.json:
```json
{
  "name": "code-quality",
  "version": "1.0.0",
  "description": "代码质量工具集：lint、format、review",
  "author": {
    "name": "Developer"
  },
  "keywords": ["lint", "format", "review", "quality"]
}
```

hooks.json（自动格式化）:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

## Plugin vs Skill 的区别

| 特性 | Skill | Plugin |
|------|-------|--------|
| 复杂度 | 单一功能 | 多功能集合 |
| 组件 | SKILL.md + 资源 | Skills + Agents + Commands + Hooks + MCP |
| 安装方式 | 复制目录 | 通过命令安装 |
| 版本管理 | 无 | 有（plugin.json） |
| 依赖管理 | 无 | 可以声明依赖 |
| 分发方式 | 手动复制 | Marketplace 或 Git |

## 创建插件的最佳实践

1. **单一职责** - 每个插件专注于一个领域
2. **清晰文档** - 提供 README 说明用途和使用方法
3. **版本管理** - 使用语义化版本号
4. **测试** - 在发布前充分测试
5. **安全** - 不要在插件中硬编码敏感信息

## 目录组织建议

```
plugins/
├── code-quality/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── ...
├── git-workflow/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── ...
└── README.md
```
