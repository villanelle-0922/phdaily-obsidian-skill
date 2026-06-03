# phdaily-obsidian-skill

两个给博士生用的 Claude Code skill，配合 Obsidian 做每日科研记录。

**`phd-log`** — 每天打开 Claude Code 时自动弹出三个问题：今天读了哪些文献、做了哪些工作、有哪些新想法。回答完之后自动生成记录存进 Obsidian 的每日笔记，不需要自己手动写。

**`obsidian-add-link`** — 用 Obsidian Clipper 剪了一篇网页或论文之后，告诉 Claude「把这篇加到今日文献」，它会自动找到那个文件、生成正确的 wikilink 写进今天的日记。（解决了 Obsidian Clipper 保存的文件名用弯引号、手动写链接容易指向错误文件的问题。）

适合用 Obsidian 管理科研笔记、又懒得每天手动记录的博士生。

---

Two Claude Code skills for PhD students who use Obsidian as a daily research journal.

## Skills

### `phd-log`
Asks you three questions at the start of each session and saves your answers to today's Obsidian daily note:
- 📚 Papers read today
- 🔨 Work done today
- 💡 New ideas

### `obsidian-add-link`
Adds a wikilink to any note in your Obsidian vault into today's daily diary — handles Unicode curly quotes in filenames correctly (a common issue with Obsidian Clipper).

---

## Installation

1. Copy the skill folders into your Claude Code skills directory:

```bash
cp -r phd-log obsidian-add-link ~/.claude/skills/
```

2. Open each `SKILL.md` and update the two lines marked `# CONFIGURE`:

```python
# CONFIGURE: change these two lines to match your setup
VAULT_PATH = '/path/to/your/obsidian/vault'
DAILY_NOTES_FOLDER = 'your-daily-notes-folder'
```

---

## Usage

### Auto-trigger at session start

Add this to your `~/.claude/settings.json` to have `phd-log` run automatically every time you open Claude Code:

```json
"hooks": {
  "SessionStart": [
    {
      "hooks": [
        {
          "type": "command",
          "command": "echo 'SESSION START INSTRUCTION: The user is a PhD student. At the very start of this session, before doing anything else, invoke the phd-log skill to run the daily research journal. Do this automatically without waiting for the user to ask.'",
          "async": false
        }
      ]
    }
  ]
}
```

### Manual trigger

```
/phd-log
```

### Add a clipping to today's diary

```
把"论文标题关键词"加到今日文献
```

---

## Requirements

- [Claude Code](https://claude.ai/code)
- Obsidian with a daily notes folder
- Python 3 (pre-installed on macOS)
