---
name: obsidian-add-link
description: 把 Obsidian vault 里的某篇笔记以正确 wikilink 加入今日日记。Use when user wants to add a note/clipping to today's diary, says "把X加到今日日记", "加到今天的文献", or wants to link a note in today's daily note.
---

# Add Obsidian Note to Daily Diary

## ⚙️ Setup

Before using, set your Obsidian vault path and daily notes folder (search for `CONFIGURE` in this file):

```
VAULT_PATH = /Users/yourname/path/to/your/obsidian/vault
DAILY_NOTES_FOLDER = 50.每日笔记   ← change to your daily notes folder name
```

---

## Workflow

### 1. 找到目标文件

**Always use Python** to search and extract filenames — never hardcode or copy-paste filenames from shell output. Obsidian Clipper saves files with Unicode curly quotes (`"` U+201C / `"` U+201D); shell tools may display them as straight quotes, causing wikilinks to point to the wrong file.

```python
import os

# CONFIGURE: change these two lines to match your setup
VAULT_PATH = '/Users/villanelle/Desktop/obsidian/second brain demo'
DAILY_NOTES_FOLDER = '50.每日笔记'

keyword = '<用户提供的关键词>'

matches = []
for root, dirs, files in os.walk(VAULT_PATH):
    for f in files:
        if keyword in f and f.endswith('.md'):
            matches.append(os.path.join(root, f))

for m in matches:
    print(m)
```

- 如果匹配多个，列出给用户选择
- 如果只有一个，直接用

### 2. 提取正确标题

```python
title = os.path.basename(matched_path).replace('.md', '')
wikilink = f'[[{title}]]'
```

### 3. 确认写入位置

问用户这个链接加到今日日记哪一节：
- `### 📚 今日文献`（默认，如果是论文/文章）
- `### 🔨 今日工作`
- `### 💡 新想法`

### 4. 写入今日日记

```python
import re
from datetime import date

today = date.today().strftime('%Y-%m-%d')
note_path = os.path.join(VAULT_PATH, DAILY_NOTES_FOLDER, f'{today}.md')

with open(note_path, 'r', encoding='utf-8') as f:
    content = f.read()

section = '### 📚 今日文献'  # 根据用户选择替换

# 在目标 section 末尾、下一个 ### 之前插入
pattern = rf'({re.escape(section)}.*?)(\n###|\Z)'
replacement = rf'\1\n- {wikilink}\2'
content = re.sub(pattern, replacement, content, flags=re.DOTALL)

with open(note_path, 'w', encoding='utf-8') as f:
    f.write(content)
```

### 5. 完成后告知用户

说明链接已加入哪一节，显示 wikilink 文本。

## ⚠️ 关键注意事项

- **永远用 Python 操作文件名**，不要手写引号。Obsidian Clipper 保存的文件名使用 Unicode 弯引号（U+201C/U+201D），shell 显示可能看起来一样但编码不同，手写会导致 Obsidian 新建一个空文件。
- 如果今日日记不存在，先创建（`# YYYY-MM-DD` 为标题）再追加
- 如果目标 section 不存在于今日日记，直接追加到文件末尾
