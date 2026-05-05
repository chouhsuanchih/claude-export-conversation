# Claude Export Conversation

A Claude Code skill to export conversation history to Markdown files.

## Features

- Export conversation history to readable Markdown format
- Automatic filename based on project name and date
- Merge strategy: multiple exports on the same day are combined, not overwritten
- All conversation turns are preserved with timestamps

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

This ensures no conversation history is lost.

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
