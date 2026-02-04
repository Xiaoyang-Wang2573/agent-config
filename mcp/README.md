# MCP（Model Context Protocol）

> ⚠️ **支持的客户端**: Claude Code、Codex CLI、OpenCode
>
> Gemini CLI 对 MCP 的支持情况不确定。

## 什么是 MCP

MCP（Model Context Protocol）是一个开放协议，允许 AI 助手连接外部工具和服务。通过 MCP，你可以让 AI 访问数据库、API、文件系统等外部资源。

常见的 MCP 服务器：
- **filesystem** - 文件系统访问
- **github** - GitHub API 集成
- **postgres** - PostgreSQL 数据库
- **slack** - Slack 消息集成

## 配置格式

MCP 配置通常使用 JSON 格式：

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-name"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

## 各客户端配置方法

### Claude Code

**配置文件**：
- 全局：`~/.claude/mcp.json`
- 项目：`.mcp.json`（项目根目录）

**配置示例**：

`~/.claude/mcp.json`:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/dir"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>"
      }
    }
  }
}
```

**使用方式**：
- 配置后重启 Claude Code
- 使用 `/mcp` 查看已连接的服务器
- MCP 工具会自动出现在可用工具列表中

---

### Codex CLI

**配置文件**：`~/.codex/config.toml`

**配置示例**：

```toml
[mcp]
# MCP 服务器配置
[mcp.servers.filesystem]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"]

[mcp.servers.github]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-github"]
env = { GITHUB_PERSONAL_ACCESS_TOKEN = "<your-token>" }
```

---

### OpenCode

**配置文件**：`opencode.json`（项目根目录或全局配置）

**配置示例**：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"]
    },
    "custom-server": {
      "command": "node",
      "args": ["./my-mcp-server.js"],
      "cwd": "${workspaceFolder}"
    }
  }
}
```

---

### Gemini CLI

❓ **不确定** - Gemini CLI 对 MCP 的支持情况尚未确认。

## 常用 MCP 服务器

| 服务器 | 用途 | 安装命令 |
|--------|------|----------|
| filesystem | 文件系统访问 | `npx @modelcontextprotocol/server-filesystem` |
| github | GitHub API | `npx @modelcontextprotocol/server-github` |
| postgres | PostgreSQL | `npx @modelcontextprotocol/server-postgres` |
| sqlite | SQLite | `npx @modelcontextprotocol/server-sqlite` |
| slack | Slack 集成 | `npx @modelcontextprotocol/server-slack` |
| puppeteer | 浏览器自动化 | `npx @modelcontextprotocol/server-puppeteer` |

## 配置示例

### 文件系统访问

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/projects",
        "/Users/username/documents"
      ]
    }
  }
}
```

### GitHub 集成

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

### 数据库连接

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/dbname"
      }
    }
  }
}
```

### 自定义 MCP 服务器

```json
{
  "mcpServers": {
    "my-custom-server": {
      "command": "node",
      "args": ["./servers/my-server.js"],
      "cwd": "/path/to/project",
      "env": {
        "API_KEY": "your-api-key",
        "DEBUG": "true"
      }
    }
  }
}
```

## 环境变量

配置中可以使用环境变量：

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

## 调试 MCP

如果 MCP 服务器无法连接，检查以下几点：

1. **命令是否存在** - 确保 `npx` 或指定的命令在 PATH 中
2. **网络连接** - 某些服务器需要网络访问
3. **环境变量** - 确保必需的环境变量已设置
4. **权限** - 确保有权限访问指定的资源
5. **日志** - 查看客户端的调试日志

## 参考链接

- [MCP 官方文档](https://modelcontextprotocol.io/)
- [MCP 服务器列表](https://github.com/modelcontextprotocol/servers)
- [MCP 规范](https://spec.modelcontextprotocol.io/)
