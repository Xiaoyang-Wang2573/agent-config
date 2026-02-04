# Commands（命令）

> ⚠️ **支持的客户端**: 仅 Claude Code
>
> Codex CLI、Gemini CLI、OpenCode 不支持 Commands 功能。

## 什么是 Command

Command（命令）是可以通过斜杠 `/` 快速调用的预定义任务。它们是简单的、单一用途的操作，适合频繁执行的任务。

例如：
- `/commit` - 生成 commit 信息
- `/review` - 审查代码变更
- `/test` - 生成测试用例

## 文件格式

Command 文件使用 Markdown 格式：

```markdown
---
name: command-name
description: 命令的简短描述
---

命令执行时的指令内容。

可以使用参数占位符：
- $1 - 第一个参数
- $2 - 第二个参数
```

## Claude Code 使用方法

**安装路径**：
- 全局：`~/.claude/commands/`
- 项目：`.claude/commands/`

**安装方式**：
```bash
# Linux/macOS
cp -r commands/* ~/.claude/commands/

# Windows (PowerShell)
Copy-Item -Recurse commands\* $env:USERPROFILE\.claude\commands\
```

**使用方式**：
```
/command-name [参数1] [参数2]
```

例如：
```
/commit
/test src/utils.ts
/review --focus security
```

## 参数占位符

Commands 支持参数占位符，在执行时会被实际参数替换：

| 占位符 | 说明 |
|--------|------|
| `$1` | 第一个参数 |
| `$2` | 第二个参数 |
| `$3` | 第三个参数 |
| `$@` | 所有参数 |

## 示例 Command

### 生成 Commit 信息

文件：`commands/commit.md`

```markdown
---
name: commit
description: 根据暂存的更改生成 commit 信息
---

分析当前 git 暂存区的更改，生成符合 Conventional Commits 规范的 commit 信息。

要求：
1. 使用英文
2. 类型包括：feat, fix, docs, style, refactor, test, chore
3. 简洁明了，不超过 72 个字符
4. 如有必要，添加详细说明

格式：
<type>(<scope>): <subject>

<body>
```

### 生成测试用例

文件：`commands/test.md`

```markdown
---
name: test
description: 为指定文件生成测试用例
---

为 $1 生成单元测试。

要求：
1. 使用项目现有的测试框架
2. 覆盖主要功能和边界条件
3. 遵循 AAA 模式（Arrange-Act-Assert）
4. 测试文件放在对应的 __tests__ 目录或 .test.ts 后缀
```

### 代码审查

文件：`commands/review.md`

```markdown
---
name: review
description: 审查当前更改或指定文件
---

审查 $1 的代码变更。

审查重点：
1. 代码正确性和逻辑
2. 潜在的 bug 和边界条件
3. 安全漏洞（注入、XSS 等）
4. 性能问题
5. 代码风格和可读性

输出格式：
- 🔴 严重问题（必须修复）
- 🟡 建议改进（推荐修复）
- 🟢 小问题（可选修复）
```

## Command vs Skill 的区别

| 特性 | Command | Skill |
|------|---------|-------|
| 复杂度 | 简单，单一文件 | 复杂，可包含多个文件 |
| 用途 | 快速执行特定任务 | 完整的功能模块 |
| 资源 | 仅包含指令 | 可包含脚本、参考文档等 |
| 调用方式 | `/command-name` | 自动触发或 `/skill-name` |

## 目录组织建议

```
commands/
├── git/
│   ├── commit.md
│   ├── pr.md
│   └── changelog.md
├── code/
│   ├── review.md
│   ├── refactor.md
│   └── document.md
├── test/
│   ├── unit.md
│   └── integration.md
└── README.md
```
