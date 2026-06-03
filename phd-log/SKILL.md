---
name: phd-log
description: PhD daily research journal — asks the user about papers read, work done, and new ideas, then saves a structured entry to Obsidian Daily Notes. Use when user types /phd-log, or invoke automatically at session start.
---

# PhD Daily Research Log

## ⚙️ Setup

Before using, set your Obsidian vault path and daily notes folder in the two variables below (search for `CONFIGURE` in this file):

```
VAULT_PATH = /Users/yourname/path/to/your/obsidian/vault
DAILY_NOTES_FOLDER = 50.每日笔记   ← change to your daily notes folder name
```

---

## Workflow

Ask the user these three questions **one at a time**, wait for each answer before asking the next:

1. **文献** — 今天读了哪些文献？（标题、作者、或简要内容都可以）
2. **工作** — 今天做了哪些工作？
3. **想法** — 有哪些新的想法或灵感？

If the user hasn't done something (e.g., didn't read papers today), note it as "无" — don't skip the question.

## Saving to Obsidian

After collecting all three answers, write the entry using Python:

```python
from datetime import datetime
import os

# CONFIGURE: change these two lines to match your setup
VAULT_PATH = '/Users/villanelle/Desktop/obsidian/second brain demo'
DAILY_NOTES_FOLDER = '50.每日笔记'

note_dir = os.path.join(VAULT_PATH, DAILY_NOTES_FOLDER)
today = datetime.now().strftime('%Y-%m-%d')
now = datetime.now().strftime('%H:%M')
note_path = os.path.join(note_dir, f'{today}.md')

entry = f"""
## {now} 科研日志

### 📚 今日文献
{lit_answer}

### 🔨 今日工作
{work_answer}

### 💡 新想法
{ideas_answer}
"""

if os.path.exists(note_path):
    with open(note_path, 'a', encoding='utf-8') as f:
        f.write(entry)
else:
    with open(note_path, 'w', encoding='utf-8') as f:
        f.write(f'# {today}\n{entry}')
```

- **If the file already exists**: append the new entry at the end.
- **If the file doesn't exist**: create it with a `# YYYY-MM-DD` header first.

## After saving

Tell the user the note was saved and show the file path.
