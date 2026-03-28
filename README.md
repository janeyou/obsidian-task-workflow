# Obsidian Task Workflow

A daily task management system for Obsidian powered by the Tasks plugin, daily notes, and (optionally) Claude Code. Write tasks where they happen, query them into a single daily view, never duplicate.

## What's Inside

```
skills/
  daily.md            # Claude Code skill: surface and manage tasks conversationally
  setup-obsidian.md   # Claude Code skill: automate the entire setup in one conversation
template/
  Daily Note.md       # Obsidian daily note template with live task query blocks
```

**Guides:**
- [How to Use Claude Code + Obsidian CLI for Productivity](How%20to%20use%20Claude%20Code%20for%20Obsidian%20CLI%20productivity.md) -- full walkthrough for terminal/CLI users
- [Obsidian Daily TODO Best Practices](Obsidian%20Daily%20TODO%20Best%20Practices.md) -- standalone Obsidian-only setup (no CLI required)

## How It Works

Tasks live **where they originate** -- meeting notes, project docs, 1:1 follow-ups. Your daily note uses live query blocks (via the [Tasks plugin](https://publish.obsidian.md/tasks/)) to pull them in automatically. Checking off a task in the daily view updates the original file.

```
Meeting Notes       Project Docs       Inbox (quick capture)
      \                  |                  /
       \                 |                 /
        v                v                v
          Daily Note (auto-queries all tasks)
          - Due Today / Overdue / Tomorrow
          - This Week / Next Week / Future
          - Pending / No Due Date
```

## Quick Start

### Option A: Automated setup with Claude Code

1. Copy the skills into your Claude Code commands folder:
   ```bash
   cp skills/daily.md ~/.claude/commands/
   cp skills/setup-obsidian.md ~/.claude/commands/
   ```
2. Open Claude Code and run:
   ```
   /setup-obsidian
   ```
3. Follow the prompts. Done.
4. Each morning, run `/daily` to manage your tasks conversationally.

### Option B: Manual setup (~5 minutes)

Follow the step-by-step instructions in [Obsidian Daily TODO Best Practices](Obsidian%20Daily%20TODO%20Best%20Practices.md). Summary:

1. Install the **Tasks** community plugin
2. Enable **Daily Notes** and **Templates** core plugins
3. Copy `template/Daily Note.md` into your vault's `Templates/` folder
4. Point **Settings > Daily Notes > Template** to `Templates/Daily Note`
5. Open your daily note -- done

## Task Format

```markdown
- [ ] Follow up on the migration plan 📅 2026-03-28
- [ ] Review PR for auth service ⏫ 📅 2026-03-27
- [?] Waiting on staging env access 📅 2026-03-26
```

| Emoji | Meaning |
|-------|---------|
| 📅 | Due date |
| ⏳ | Scheduled date |
| 🛫 | Start date |
| ⏫ | High priority |
| 🔼 | Medium priority |
| 🔽 | Low priority |
| 🔁 | Recurrence (e.g., `🔁 every week`) |

Use `- [?]` for tasks you're waiting on someone else for (pending status).

## Requirements

- [Obsidian](https://obsidian.md/) v1.12.7+ (for CLI support)
- [Tasks plugin](https://publish.obsidian.md/tasks/) (community plugin)
- [Claude Code](https://claude.ai/code) (optional, for `/daily` and `/setup-obsidian` skills)

## License

MIT
