# How to Use Claude Code + Obsidian CLI for Productivity

## Background
Obsidian has an official CLI, and paired with Claude Code, it becomes a powerful task management system you can drive entirely from the terminal. This guide covers the full setup.

> **Don't use Claude Code?** See the companion guide: [Obsidian Daily TODO Best Practices](Obsidian%20Daily%20TODO%20Best%20Practices.md) for a standalone Obsidian-only setup.

## What is Obsidian CLI?
- Official command-line interface from Obsidian (v1.12.7+)
- "Anything you can do in Obsidian you can do from the command line"
- Supports: searching vaults, opening daily notes, managing tasks, creating notes from templates, and more

## Installation (macOS)

1. **Update Obsidian** — download the latest installer from obsidian.md or check for updates in Settings > General
2. **Enable the CLI** — in Obsidian go to **Settings > General > Command line interface** and toggle it on
3. **Register the CLI in PATH** — Obsidian shows instructions; on macOS it adds a line to `~/.zprofile`
4. **Restart your terminal**
5. **Verify**: run `obsidian version` (note: NOT `--version`)

---

## Daily TODO Workflow

### How It Works

Tasks live **where they originate** — meeting notes, 1:1 follow-ups, project docs. They are never duplicated. Instead, your daily note uses **live query blocks** (via the Tasks plugin) to pull in tasks from across the vault automatically.

```
Meeting Notes (standups, 1:1s, etc.)     Inbox.md (quick capture)
        \                                    /
         \                                  /
          v                                v
     Daily Note (YYYY-MM-DD.md)
     - Shows: Due Today, Overdue, All Open Tasks
     - Checking off a task here updates the ORIGINAL file
```

### What Was Set Up

| Component | What | Where |
|-----------|------|-------|
| Tasks plugin | Community plugin for task queries | `.obsidian/plugins/obsidian-tasks-plugin/` |
| Templates folder | Holds daily note template | `Templates/` |
| Daily note template | Template with task query blocks | `Templates/Daily Note.md` |
| Daily notes config | Auto-creates `YYYY-MM-DD.md` from template | `.obsidian/daily-notes.json` |
| Inbox | Quick capture for standalone tasks | `Inbox.md` |
| Completed view | Bird's-eye view of all completed tasks | `Completed.md` |
| Claude Code `/setup-obsidian` skill | Automates the entire setup in one conversation | `~/.claude/commands/setup-obsidian.md` |
| Claude Code `/daily` skill | Surface and manage tasks conversationally | `~/.claude/commands/daily.md` |

### Task Format (Obsidian Tasks Plugin)

Add these emoji annotations to any task to make it queryable:

```markdown
- [ ] Do the thing 📅 2026-03-25           # due date
- [ ] Scheduled item ⏳ 2026-03-25          # scheduled date
- [ ] Important thing ⏫ 📅 2026-03-26      # high priority + due date
- [ ] Less urgent 🔽 📅 2026-04-01          # low priority + due date
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

**Tip**: You don't need to memorize emojis. In Obsidian, use `Cmd+P` > "Tasks: Create or edit task" for a GUI form with date pickers and priority dropdowns.

### The Daily Note Template

Located at `Templates/Daily Note.md`. The daily note focuses on **today**:

1. **Focus** — manually write your top 1-3 priorities
2. **Due Today** — auto-query: tasks with `📅` matching today's date (excludes pending, sorted by priority)
3. **Pending** — auto-query: tasks marked as waiting on others (`- [?]` checkbox)
4. **Overdue** — auto-query: tasks with `📅` before today
5. **Quick Capture** — editable space for new tasks and thoughts
6. **Notes** — freeform area
7. **Completed Today** — auto-query: tasks checked off today
8. **Link to Inbox** — for planning ahead

### Inbox.md

Located at the vault root. Inbox handles forward-planning views that don't belong in the daily note:

1. **Quick capture** — tasks, thoughts, ideas at the top
2. **Today** — link back to the current daily note
3. **Tomorrow / This Week / Next Week / Future** — auto-queries for upcoming tasks by time horizon
4. **No Due Date** — open tasks without a `📅` date, grouped by folder
5. **Ideas** — longer-term ideas section at the bottom

The daily note and Inbox link to each other — the daily note has a `→ [[Inbox]]` link at the bottom, and Inbox has a `→ [[Daily Notes/YYYY-MM-DD]]` link in its Today section.

**Folder filtering:** If your vault has folders with grab-and-go checklists (e.g., packing lists), add `path does not include FolderName` to the "No Due Date" query in Inbox to keep them from cluttering the view.

To mark a task as pending (waiting on someone), change `- [ ]` to `- [?]` — type `?` inside the brackets, or right-click the checkbox and select "Pending." It moves from "Due Today" into the "Pending" section.

**Note**: The query blocks render as read-only in Reading view. To type in the Quick Capture or Notes sections, either scroll past the query blocks or press `Cmd+E` to toggle to Editing view.

### Your Daily Routine

#### Morning: Open your daily note

**Option A — Claude Code** (recommended):
```
/daily
```
This surfaces overdue/due today/due this week tasks, lets you add new ones or mark tasks done — all conversationally.

**Option B — Terminal**:
```bash
obsidian daily                    # opens today's daily note in Obsidian
obsidian tasks todo verbose       # lists all open tasks grouped by file
```

**Option C — Obsidian app**:
Click the calendar icon or use `Cmd+P` > "Open daily note"

#### During the day: Capture tasks where they happen

**In meeting notes** — add tasks inline with a due date:
```markdown
## Meeting with Platform Team — 2026-03-25
- Discussed migration timeline
- [ ] Follow up with infra team on staging env 📅 2026-03-28
- Decision: delay rollout to April
```

**Quick capture from terminal**:
```bash
obsidian daily:append content="- [ ] Quick task 📅 2026-03-26"
obsidian append path="Inbox.md" content="- [ ] Idea to explore later"
```

**Quick capture from Claude Code**:
Just tell Claude: "add a task: follow up with Alex about the project review, due Thursday"

**Quick capture in Obsidian app**:
Open the daily note, scroll to Quick Capture, and type your task with a `📅 YYYY-MM-DD` due date. Or use `Cmd+P` > "Tasks: Create or edit task" for the GUI.

#### End of day: Review

Open the daily note — check off what's done and they'll appear in the **Completed Today** section. Open **Inbox.md** for a time-based breakdown: Tomorrow, This Week, Next Week, Future, and No Due Date. Check off tasks in Obsidian (click the checkbox) or via CLI:
```bash
obsidian task path="Meetings/Weekly Standup.md" line=12 done
```

---

## CLI Quick Reference

**Note**: The correct version command is `obsidian version` (not `--version`).

| Command | What it does |
|---|---|
| `obsidian version` | Show version (v1.12.7) |
| `obsidian vaults` | List known vaults |
| `obsidian tasks todo` | List incomplete tasks across vault |
| `obsidian tasks done` | List completed tasks |
| `obsidian tasks todo verbose` | Tasks grouped by file with line numbers |
| `obsidian tasks daily` | Tasks from today's daily note |
| `obsidian daily` | Open today's daily note |
| `obsidian daily:read` | Read today's daily note |
| `obsidian daily:append content="text"` | Append to daily note |
| `obsidian search query="keyword"` | Search vault for text |
| `obsidian search:context query="keyword"` | Search with line context |
| `obsidian read file="Note Name"` | Read a note by name |
| `obsidian create name="Note" content="text"` | Create a new note |
| `obsidian append file="Note" content="text"` | Append to a note |
| `obsidian task path="file.md" line=5 done` | Mark a specific task done |
| `obsidian tags counts sort=count` | List tags by frequency |
| `obsidian files folder="Subfolder"` | List files in a folder |
| `obsidian reload` | Reload vault (after config changes) |

---

## Bonus: Obsidian Look & Feel

A few tweaks to make Obsidian feel more like an IDE — compact, clean, fast:

| Setting | Where | Value |
|---------|-------|-------|
| Theme | Settings > Appearance > Themes > Manage | **Things** (task-focused, clean) |
| Zoom level | Settings > Appearance > Zoom level | **83%** (tighter layout) |
| Inline title | Settings > Appearance > Show inline title | **Off** (saves vertical space) |
| Font size | Settings > Appearance > Font size | **14-15** (adjust to taste) |
| Style Settings plugin | Community plugins > "Style Settings" | Customize theme colors without CSS |

Install the **Style Settings** plugin to get a GUI for tweaking the Things theme — colors, heading styles, checkbox styles, etc. Go to **Settings > Style Settings > Things Theme** after installing.

**Custom folder sorting**: Install the **Custom Sort** plugin (`obsidian plugin:install id=custom-sort enable`) to define your own sidebar order. Create a `sortspec.md` in the vault root with a `sorting-spec` YAML frontmatter listing items by name (without `.md`). Then `Cmd+P` > type "custom s" > select "Custom File Explorer sorting: Enable and apply the custom sorting... Sort-on." Files and folders mix together in your specified order. Alternatively, prefix folders with numbers (`01 Projects`, `02 People`) for a no-plugin approach.

Or via CLI:
```bash
obsidian theme:install name="Things"
obsidian theme:set name="Things"
obsidian plugin:install id=obsidian-style-settings enable
obsidian plugin:install id=custom-sort enable
```

Keyboard shortcuts: `Cmd +` / `Cmd -` to zoom, `Cmd 0` to reset.

---

## Claude Code Settings

- **Auto mode**: `~/.claude/settings.json` > `permissions.defaultMode: "auto"`
- **`/setup-obsidian` skill**: `~/.claude/commands/setup-obsidian.md` — run `/setup-obsidian` to set up the entire workflow automatically (plugins, template, theme, everything)
- **`/daily` skill**: `~/.claude/commands/daily.md` — run `/daily` to surface tasks conversationally
- You can create additional skills in `~/.claude/commands/` for other recurring workflows

---

## Things To Do Next (Optional)

- [ ] **Try the `/daily` skill** — run `/daily` in Claude Code to see it in action
- [x] **Add due dates to new tasks** — any time you write `- [ ]` in a note, add `📅 YYYY-MM-DD` so the daily note picks it up ✅ 2026-03-25
- [ ] **Explore Dataview plugin** — for advanced queries (e.g., weekly review grouped by project); install via `obsidian plugin:install id=dataview enable`
- [ ] **Explore Templater plugin** — for dynamic templates with date math, conditionals; install via `obsidian plugin:install id=templater-obsidian enable`
- [ ] **Set up a weekly review note** — a template that queries tasks completed this week + outstanding items
