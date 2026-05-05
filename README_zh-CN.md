# Claude Export Conversation

将 Claude Code 对话记录导出为 Markdown 文件的技能。

## 为什么选择这个项目？

已有一个 Python CLI 工具（[claude-conversation-extractor](https://github.com/ZeroSumQuant/claude-conversation-extractor)）用于导出 Claude Code 对话。本项目采用了不同的方式：

| 功能 | Claude Export Conversation | claude-conversation-extractor |
|------|---------------------------|------------------------------|
| 类型 | Claude Code **Skill** | Python CLI 工具 |
| 安装方式 | 复制 1 个文件 | pip/pipx 安装 |
| 触发方式 | 在 Claude 内执行 `/export-conversation` | 在终端执行 claude-extract |
| 无缝集成 | ✅ 直接在 Claude Code 内运行 | ❌ 需要切换到终端 |
| 合并策略 | ✅ 同日多次导出自动合并 | ❌ 覆盖文件 |
| 搜索功能 | ❌ 不包含 | ✅ 实时搜索 |
| 批量导出 | ❌ 需手动处理 | ✅ 一键导出所有 |
| 输出格式 | Markdown | Markdown/JSON/HTML |

本项目专注于**简单**和**无缝集成 Claude Code** — 无需切换终端、无依赖，直接 `/export-conversation`。

## 功能特点

- **无缝集成** — 作为 Claude Code Skill 直接运行，无需切换到终端
- **单文件安装** — 只需复制 SKILL.md，无需 pip/pipx
- **合并策略** — 同一天的多次导出会合并，而非覆盖
- **可读格式** — 清晰导出，包含时间戳和轮次编号
- **自动命名** — 文件自动命名为 `{项目名}_{YYYYMMDD}.md`

## 安装方法

### 方法一：复制 SKILL.md 文件

1. 从本仓库下载 `SKILL.md`
2. 复制到 Claude Code 的 skills 目录：
   ```bash
   # macOS / Linux
   mkdir -p ~/.claude/skills/export-conversation
   cp SKILL.md ~/.claude/skills/export-conversation/skill.md
   ```

### 方法二：克隆仓库

```bash
git clone https://github.com/chouhsuanchih/claude-export-conversation.git
cp claude-export-conversation/SKILL.md ~/.claude/skills/export-conversation/skill.md
```

## 配置

首次使用前，需要在 `~/.claude/settings.json` 中配置输出目录：

```json
{
  "skills": {
    "export-conversation": {
      "outputDir": "/你的/导出/目录/路径"
    }
  }
}
```

**macOS 示例：**
```json
{
  "skills": {
    "export-conversation": {
      "outputDir": "/Users/你的用户名/Documents/Claude对话记录"
    }
  }
}
```

如果未配置，默认为 `~/Documents/Claude Conversations/`。

## 使用方法

直接执行命令：

```
/export-conversation
```

无需任何参数。技能会自动：

1. 读取当前项目的对话历史
2. 导出到配置的目录
3. 文件命名为 `{项目名}_{YYYYMMDD}.md`

### 示例

假设项目名为 "my-project"，在 2026 年 5 月 5 日导出：
- 输出文件：`my-project_20260505.md`
- 保存位置：你配置的 `outputDir`

## 合并机制

如果同一天对同一项目进行多次导出：

- **首次导出**：创建新文件，包含所有对话轮次
- **后续导出**：只追加新对话轮次到现有文件

这确保了对话历史不会被丢失 — 这是与其他导出工具的关键区别。

## 输出格式

```markdown
# 与 Claude 的对话记录

**项目**：/path/to/project

**导出时间**：2026-05-05 14:30:00

**对话轮数**：25

---

## 第 1 轮

**时间**：2026-05-05 10:00:00

**用户**：
消息内容...

**时间**：2026-05-05 10:01:00

**Claude**：
回复内容...

---
```

## 系统要求

- Claude Code CLI
- Python 3（用于解析 JSONL 文件）

## 许可证

MIT License

## 参与贡献

欢迎提交 Issue 和 Pull Request！
