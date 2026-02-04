# Agents（代理）

> ⚠️ **支持的客户端**: Claude Code、Codex CLI、OpenCode
>
> Gemini CLI 不支持 Agents 功能。

## 什么是 Agent

Agent（代理）是具有特定角色和专业能力的 AI 助手。通过定义 Agent，你可以让 AI 以特定的身份和专业知识来处理任务。

例如：
- **后端架构师** - 专注于 API 设计和系统架构
- **前端开发者** - 专注于 UI 实现和用户体验
- **代码审查员** - 专注于代码质量和最佳实践

## 文件格式

Agent 文件使用 Markdown 格式，包含 YAML frontmatter：

```markdown
---
name: agent-name
description: 代理的简短描述，说明何时使用
capabilities:
  - 能力1
  - 能力2
---

# Agent 名称

详细的系统提示词，定义代理的角色、专业知识和行为方式。

## 职责
- 职责1
- 职责2

## 专业领域
- 领域1
- 领域2
```

## 各客户端使用方法

### Claude Code

**安装路径**：
- 全局：`~/.claude/agents/`
- 项目：`.claude/agents/`

**安装方式**：
```bash
# Linux/macOS
cp -r agents/* ~/.claude/agents/

# Windows (PowerShell)
Copy-Item -Recurse agents\* $env:USERPROFILE\.claude\agents\
```

**使用方式**：
- 在对话中使用 `/agents` 查看可用代理
- Claude 会根据任务自动选择合适的代理
- 也可以手动指定使用某个代理

**特点**：
- 支持子目录分类（如 `agents/engineering/`, `agents/design/`）
- 支持 YAML frontmatter 定义元数据

---

### Codex CLI

**安装路径**：
- 全局：`~/.codex/AGENTS.md`
- 项目：项目根目录的 `AGENTS.md`

**安装方式**：

Codex CLI 使用单一的 `AGENTS.md` 文件，需要将多个 agent 内容合并：

```bash
# 创建或追加到 AGENTS.md
cat agents/**/*.md >> ~/.codex/AGENTS.md

# 或手动编辑 AGENTS.md，将需要的 agent 内容复制进去
```

**文件格式**：
```markdown
# AGENTS.md

## 工作约定
- 约定1
- 约定2

## 代码风格
- 风格1
- 风格2
```

**特点**：
- 必须使用 `AGENTS.md` 文件名（大写）
- 支持 `AGENTS.override.md` 用于临时覆盖
- 文件大小限制：默认 32 KiB

---

### OpenCode

**安装路径**：
- 全局：`~/.config/opencode/agents/`
- 项目：`.opencode/agents/`

**安装方式**：
```bash
# Linux/macOS
cp -r agents/* ~/.config/opencode/agents/

# Windows (PowerShell)
Copy-Item -Recurse agents\* $env:APPDATA\opencode\agents\
```

**使用方式**：
- 使用 Tab 键切换主代理
- 使用 `@agent-name` 调用特定代理
- 运行 `opencode agent create` 交互式创建代理

**特点**：
- 文件名即为代理标识（如 `review.md` 创建 `review` 代理）
- 支持 YAML frontmatter 配置

---

### Gemini CLI

❌ **不支持** - Gemini CLI 目前不支持 Agents 功能。

## 示例 Agent

```markdown
---
name: code-reviewer
description: 代码审查专家，专注于代码质量、安全性和最佳实践
capabilities:
  - 代码审查
  - 安全漏洞检测
  - 性能优化建议
---

# 代码审查专家

你是一位经验丰富的代码审查专家，专注于提高代码质量和团队协作效率。

## 职责

- 审查代码变更，确保符合团队规范
- 识别潜在的 bug 和安全漏洞
- 提供建设性的改进建议
- 确保代码可读性和可维护性

## 审查重点

1. **代码正确性** - 逻辑是否正确，边界条件是否处理
2. **安全性** - 是否存在注入、XSS 等安全风险
3. **性能** - 是否有明显的性能问题
4. **可读性** - 命名是否清晰，结构是否合理
5. **测试覆盖** - 是否有足够的测试

## 反馈风格

- 具体指出问题所在的代码行
- 解释为什么这是一个问题
- 提供具体的改进建议
- 区分"必须修改"和"建议修改"
```

## 目录组织建议

```
agents/
├── engineering/          # 工程相关
│   ├── backend.md
│   ├── frontend.md
│   └── devops.md
├── design/              # 设计相关
│   ├── ui-designer.md
│   └── ux-researcher.md
├── review/              # 审查相关
│   ├── code-reviewer.md
│   └── security-auditor.md
└── README.md
```
