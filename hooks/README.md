# Hooks（钩子）

> ⚠️ **支持的客户端**: 仅 Claude Code
>
> Codex CLI、Gemini CLI、OpenCode 不支持 Hooks 功能。

## 什么是 Hook

Hook（钩子）是在特定事件发生时自动执行的操作。通过 Hooks，你可以在 AI 执行某些操作前后自动运行脚本或命令。

常见用途：
- 代码保存后自动格式化
- 文件修改后自动运行 lint
- 会话开始时加载特定配置
- 工具执行失败时发送通知

## 支持的事件

| 事件 | 触发时机 |
|------|----------|
| `PreToolUse` | 工具执行前 |
| `PostToolUse` | 工具成功执行后 |
| `PostToolUseFailure` | 工具执行失败后 |
| `UserPromptSubmit` | 用户提交提示时 |
| `Notification` | 发送通知时 |
| `Stop` | Claude 尝试停止时 |
| `SubagentStart` | 子代理启动时 |
| `SubagentStop` | 子代理停止时 |
| `SessionStart` | 会话开始时 |
| `SessionEnd` | 会话结束时 |
| `PreCompact` | 对话历史压缩前 |
| `PermissionRequest` | 显示权限对话框时 |

## 配置方式

### 方式一：在 settings.json 中配置

**配置文件**：
- 全局：`~/.claude/settings.json`
- 项目：`.claude/settings.json`

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

### 方式二：在插件中配置

**配置文件**：`hooks/hooks.json`

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/format.sh"
          }
        ]
      }
    ]
  }
}
```

## Hook 类型

### command - 执行命令

```json
{
  "type": "command",
  "command": "npm run lint"
}
```

### prompt - LLM 评估

```json
{
  "type": "prompt",
  "prompt": "检查这个更改是否符合代码规范：$ARGUMENTS"
}
```

### agent - 代理验证

```json
{
  "type": "agent",
  "agent": "security-checker"
}
```

## 配置示例

### 自动格式化代码

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

### 自动运行测试

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npm test -- --findRelatedTests $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### 会话开始时加载配置

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "cat ~/.project-context.md"
          }
        ]
      }
    ]
  }
}
```

### 安全检查

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "检查这个命令是否安全：$ARGUMENTS"
          }
        ]
      }
    ]
  }
}
```

### 多个 Hook 组合

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
          },
          {
            "type": "command",
            "command": "eslint --fix $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '会话开始于 $(date)' >> ~/.claude/session.log"
          }
        ]
      }
    ]
  }
}
```

## 环境变量

Hook 执行时可用的环境变量：

| 变量 | 说明 |
|------|------|
| `$CLAUDE_FILE_PATH` | 当前操作的文件路径 |
| `$ARGUMENTS` | 传递给 Hook 的参数 |
| `$CLAUDE_PLUGIN_ROOT` | 插件根目录（仅插件内） |

## 调试 Hooks

如果 Hook 不工作，检查以下几点：

1. **脚本权限** - 确保脚本有执行权限
   ```bash
   chmod +x ./scripts/my-hook.sh
   ```

2. **路径正确** - 使用绝对路径或环境变量

3. **matcher 匹配** - 确保 matcher 正则表达式正确

4. **查看日志** - 使用 `claude --debug` 查看详细日志

## 最佳实践

1. **保持简单** - Hook 应该快速执行，避免长时间阻塞
2. **错误处理** - 脚本应该优雅处理错误
3. **幂等性** - Hook 应该可以安全地多次执行
4. **日志记录** - 记录 Hook 执行情况便于调试
5. **测试** - 在生产使用前充分测试 Hook

## 目录组织建议

```
hooks/
├── scripts/
│   ├── format.sh
│   ├── lint.sh
│   └── test.sh
├── hooks.json
└── README.md
```
