---
name: export-conversation
description: Export the current Claude Code conversation to a Markdown file. Use when user asks to "export conversation", "导出对话", "save conversation", "导出聊天记录", or wants to export/save the current chat history to a file.
version: 3.0.0
---

# Export Conversation

Export Claude Code conversation history to a Markdown file.

## Configuration

Before first use, set the output directory in your `~/.claude/settings.json`:

```json
{
  "skills": {
    "export-conversation": {
      "outputDir": "/path/to/your/export/directory"
    }
  }
}
```

If not configured, defaults to `~/Documents/Claude Conversations/`.

## How to Use

```
/export-conversation
```

No arguments needed. Output path and filename are determined automatically.

## Output Rules

1. **Output directory**: Configurable via `skills.export-conversation.outputDir` in settings.json
   - All projects' conversations are exported to this unified directory
   - Create the directory if it doesn't exist

2. **Filename format**: `{项目文件夹名}_{YYYYMMDD}.md`
   - 项目文件夹名 = the name of the current project's root folder (e.g., "my-project", "work-docs")
   - Date = the date of export in YYYYMMDD format
   - Example: `my-project_20260505.md`, `work-docs_20260413.md`

3. **Merge strategy (same-day multiple exports)**:
   - If the target file already exists, DO NOT overwrite it
   - Instead, read the existing file and determine the last exported turn number
   - Append new conversation turns starting from the next turn number
   - Update the metadata at the top of the file (导出时间, 对话轮数)
   - This ensures all conversation records are preserved across multiple exports on the same day

## What It Does

1. Reads the current project's conversation JSONL file from:
   `~/.claude/projects/{project_hash}/conversation.jsonl`

2. Parses all user and assistant messages, filtering by:
   - `type`: "user" or "assistant" only
   - `message.content`: extract text from content blocks with `type: "text"`
   - Skip empty messages, system messages, and tool-only messages

3. Formats them as a readable Markdown document

4. Checks if the target file already exists:
   - **If not exists**: create new file with full content
   - **If exists**: parse existing file to find the last turn number, then append only new turns and update metadata

5. Saves to the configured output directory

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

## 第 2 轮
...
```

## Merge Implementation Details

When the target file already exists:

1. Read the existing markdown file
2. Find the last `## 第 N 轮` heading to determine N (the last exported turn number)
3. Parse the current JSONL and format ALL turns as markdown
4. Take only turns with number > N (the new turns)
5. Append these new turns to the existing file (before any trailing content)
6. Update the metadata:
   - **导出时间**: update to current time
   - **对话轮数**: update to the new total count
7. Write the merged result back to the file

## Implementation

This skill parses the Claude Code conversation JSONL format and converts it to readable Markdown.

The JSONL format has objects with:
- `type`: "user" or "assistant" (also "system", "queue-operation", "ai-title", etc. — skip these)
- `message.content`: array of content blocks; use blocks with `type: "text"` and extract the `text` field
- `timestamp`: ISO format timestamp (e.g., "2026-05-05T06:44:38.466Z")

A "turn" is defined as a user message followed by its corresponding assistant response. Group consecutive user+assistant pairs into turns.
