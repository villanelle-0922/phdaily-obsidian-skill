# phdaily-obsidian-skill

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
