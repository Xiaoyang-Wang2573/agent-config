# Skills（技能）

> ⚠️ **支持的客户端**: Claude Code、Gemini CLI、OpenCode
>
> Codex CLI 不支持 Skills 功能。

## 什么是 Skill

Skill（技能）是包含指令和资源的完整功能模块。与简单的 Command 不同，Skill 可以包含多个文件，如脚本、参考文档、配置模板等。

例如：
- **skill-creator** - 创建新技能的工具包
- **pdf-processor** - PDF 文档处理技能
- **code-reviewer** - 代码审查技能

## 目录结构

每个 Skill 是一个目录，必须包含 `SKILL.md` 作为入口文件：

```
skill-name/
├── SKILL.md           # 必需：技能入口文件
├── scripts/           # 可选：脚本文件
│   ├── init.py
│   └── process.sh
├── references/        # 可选：参考文档
│   └── guide.md
├── templates/         # 可选：模板文件
│   └── config.json
└── assets/           # 可选：其他资源
```

## SKILL.md 文件格式

```markdown
---
name: skill-name
description: 技能描述，用于自动触发判断
---

# 技能名称

技能的详细指令和使用说明。

## 功能

- 功能1
- 功能2

## 使用方法

说明如何使用这个技能...
```

## 各客户端使用方法

### Claude Code

**安装路径**：
- 全局：`~/.claude/skills/`

**安装方式**：

方式一：手动复制
```bash
# Linux/macOS
cp -r skills/* ~/.claude/skills/

# Windows (PowerShell)
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

方式二：通过插件安装
```bash
claude plugin install <plugin-with-skills>
```

**使用方式**：
- Claude 会根据任务自动触发相关技能
- 也可以使用 `/skill-name` 手动调用
- 技能的 `SKILL.md` 内容和目录结构会被添加到上下文中

---

### Gemini CLI

**安装路径**：
- 全局：`~/.gemini/skills/`
- 项目：`.gemini/skills/`

**安装方式**：

方式一：手动复制
```bash
# Linux/macOS
cp -r skills/* ~/.gemini/skills/

# Windows (PowerShell)
Copy-Item -Recurse skills\* $env:USERPROFILE\.gemini\skills\
```

方式二：命令安装
```bash
# 从本地目录安装
gemini skills install ./skills/skill-name

# 从 Git 仓库安装
gemini skills install https://github.com/user/skill-repo

# 从 .skill 压缩包安装
gemini skills install ./skill-name.skill
```

**管理命令**：
```bash
# 列出所有技能
gemini skills list

# 启用/禁用技能
gemini skills enable <name>
gemini skills disable <name>

# 卸载技能
gemini skills uninstall <name>
```

**交互式命令**（在会话中）：
```
/skills list
/skills enable <name>
/skills disable <name>
/skills reload
```

**优先级**：工作区技能 > 用户技能 > 扩展技能

---

### OpenCode

**安装路径**：
- 全局：`~/.config/opencode/skills/`
- 项目：`.opencode/skills/`

**安装方式**：
```bash
# Linux/macOS
cp -r skills/* ~/.config/opencode/skills/

# Windows (PowerShell)
Copy-Item -Recurse skills\* $env:APPDATA\opencode\skills\
```

**使用方式**：
- 技能会被自动发现和加载
- 可以通过配置文件启用/禁用特定技能

---

### Codex CLI

❌ **不支持** - Codex CLI 目前不支持 Skills 功能。

## 示例 Skill

### 技能创建器

目录结构：
```
skill-creator/
├── SKILL.md
├── scripts/
│   ├── init_skill.py
│   ├── package_skill.py
│   └── quick_validate.py
└── templates/
    └── SKILL.template.md
```

SKILL.md 内容：
```markdown
---
name: skill-creator
description: 创建和管理 AI 技能包的工具
---

# 技能创建器

帮助你创建、验证和打包 AI 技能。

## 功能

1. **初始化技能** - 创建技能目录结构
2. **验证技能** - 检查技能格式是否正确
3. **打包技能** - 生成可分发的技能包

## 使用方法

### 创建新技能

运行 `scripts/init_skill.py` 并提供技能名称：

```bash
python scripts/init_skill.py my-new-skill
```

### 验证技能

```bash
python scripts/quick_validate.py ./my-new-skill
```

### 打包技能

```bash
python scripts/package_skill.py ./my-new-skill
```

## 技能结构要求

- 必须包含 `SKILL.md` 文件
- `SKILL.md` 必须有 YAML frontmatter
- frontmatter 必须包含 `name` 和 `description`
```

## Skill vs Command vs Agent 的区别

| 特性 | Command | Skill | Agent |
|------|---------|-------|-------|
| 复杂度 | 简单 | 中等 | 复杂 |
| 文件结构 | 单文件 | 目录（多文件） | 单文件 |
| 用途 | 快速任务 | 功能模块 | 角色扮演 |
| 包含资源 | 仅指令 | 指令+脚本+文档 | 仅指令 |
| 触发方式 | 手动 `/cmd` | 自动或手动 | 自动或手动 |

## 目录组织建议

```
skills/
├── development/
│   ├── skill-creator/
│   │   └── SKILL.md
│   └── code-generator/
│       └── SKILL.md
├── documentation/
│   ├── doc-writer/
│   │   └── SKILL.md
│   └── api-documenter/
│       └── SKILL.md
└── README.md
```
