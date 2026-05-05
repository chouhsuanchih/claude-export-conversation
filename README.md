# Claude Export Conversation

A Claude Code skill to export conversation history to Markdown files.

## Why This Project?

There is an existing Python CLI tool ([claude-conversation-extractor](https://github.com/ZeroSumQuant/claude-conversation-extractor)) for exporting Claude Code conversations. This project takes a different approach:

| Feature | Claude Export Conversation | claude-conversation-extractor |
|---------|---------------------------|------------------------------|
| Type | Claude Code **Skill** | Python CLI Tool |
| Installation | Copy 1 file | pip/pipx install |
| Trigger | `/export-conversation` in Claude | claude-extract in terminal |
| Seamless Integration | ✅ Works inside Claude Code | ❌ Requires terminal |
| Merge Strategy | ✅ Combines same-day exports | ❌ Overwrites |
| Search Capability | ❌ Not included | ✅ Real-time search |
| Bulk Export | ❌ Manual | ✅ One-click all sessions |
| Output Formats | Markdown | Markdown/JSON/HTML |

This project focuses on **simplicity** and **seamless Claude Code integration** — no terminal switching, no dependencies, just `/export-conversation`.

## Features

- **Seamless Integration** — Works directly inside Claude Code as a Skill, no terminal needed
- **One-File Installation** — Just copy the SKILL.md file, no pip/pipx required
- **Merge Strategy** — Multiple exports on the same day are combined, not overwritten
- **Readable Markdown** — Clean export with timestamps and turn numbers
- **Auto Naming** — Files named automatically as `{project-name}_{YYYYMMDD}.md`

## Installation

### Option 1: Copy the SKILL.md file

1. Download `SKILL.md` from this repository
2. Copy it to your Claude Code skills directory:
   ```bash
   # For macOS/Linux
   mkdir -p ~/.claude/skills/export-conversation
   cp SKILL.md ~/.claude/skills/export-conversation/skill.md
   ```

### Option 2: Clone the repository

```bash
git clone https://github.com/chouhsuanchih/claude-export-conversation.git
cp claude-export-conversation/SKILL.md ~/.claude/skills/export-conversation/skill.md
```

## Configuration

Before first use, configure the output directory in your `~/.claude/settings.json`:

```json
{
  "skills": {
    "export-conversation": {
      "outputDir": "/path/to/your/export/directory"
    }
  }
}
```

**Example for macOS:**
```json
{
  "skills": {
    "export-conversation": {
      "outputDir": "/Users/yourname/Documents/Claude Conversations"
    }
  }
}
```

If not configured, defaults to `~/Documents/Claude Conversations/`.

## Usage

Simply trigger the skill:

```
/export-conversation
```

No arguments needed. The skill will:

1. Read your current project's conversation history
2. Export to the configured directory
3. Name the file as `{project-name}_{YYYYMMDD}.md`

### Example

For a project named "my-project" exported on May 5, 2026:
- Output file: `my-project_20260505.md`
- Location: Your configured `outputDir`

## Merge Behavior

If you export the same project multiple times on the same day:

- **First export**: Creates a new file with all conversation turns
- **Subsequent exports**: Appends only new turns to the existing file

This ensures no conversation history is lost — a key difference from other export tools that simply overwrite.

## Output Format

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

## Requirements

- Claude Code CLI
- Python 3 (for JSONL parsing)

## License

MIT License

## Contributing

Issues and pull requests are welcome!
