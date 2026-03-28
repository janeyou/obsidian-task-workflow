# Obsidian Daily TODO Best Practices

A standalone guide to managing tasks in Obsidian using daily notes and the Tasks plugin. No CLI or external tools required — everything is done inside the Obsidian app.

---

## The Core Idea

**Write tasks where they happen. Query them into a daily view. Never duplicate.**

Instead of maintaining a separate TODO list, you write tasks directly in meeting notes, project docs, and 1:1 follow-ups. Your daily note automatically pulls them in using live query blocks. When you check off a task in the daily view, it updates the original file.

```
Meeting Notes          Project Docs          Inbox (quick capture)
      \                     |                     /
       \                    |                    /
        v                   v                   v
           Daily Note (auto-queries all tasks)
           - Due Today
           - Overdue
           - Tomorrow / This Week / Next Week / Future / No Due Date
```

---

## Setup (One-Time, ~5 Minutes)

### Step 1: Install the Tasks Community Plugin

1. Open Obsidian > **Settings** > **Community plugins**
2. Click **Browse**, search for **"Tasks"** (by obsidian-tasks-group)
3. Click **Install**, then **Enable**

This plugin adds a query engine that can find and display tasks from across your entire vault.

### Step 2: Enable Daily Notes

1. Open **Settings** > **Core plugins**
2. Enable **Daily notes** (toggle on)
3. Enable **Templates** (toggle on)

### Step 3: Create a Templates Folder

1. In your vault, create a new folder called `Templates`
2. Go to **Settings** > **Templates** (under Core plugins)
3. Set **Template folder location** to `Templates`

### Step 4: Create the Daily Note Template

Create a new note in `Templates/` called **Daily Note** with this content:

````markdown
# {{date}}

## Focus
-

## Due Today
```tasks
not done
due on {{date}}
status.name does not include Pending
sort by priority
```

## Pending (waiting on others)
```tasks
status.name includes Pending
sort by priority
group by folder
```

## Overdue
```tasks
not done
due before {{date}}
sort by priority
group by folder
```

## Quick Capture
-

## Notes


## Completed Today
```tasks
done
done on {{date}}
sort by done reverse
group by folder
```

## Tomorrow
```tasks
not done
due on tomorrow
sort by priority
group by folder
```

## This Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().add(1, 'day'), 'day') && task.due.moment?.isSameOrBefore(moment().endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Next Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().endOf('isoWeek'), 'day') && task.due.moment?.isSameOrBefore(moment().add(1, 'week').endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Future
```tasks
not done
filter by function !!task.due.moment?.isAfter(moment().add(1, 'week').endOf('isoWeek'), 'day')
sort by due
group by folder
```

## No Due Date
```tasks
not done
no due date
sort by path
group by folder
```
````

### Step 5: Configure Daily Notes to Use the Template

1. Go to **Settings** > **Daily notes**
2. Set **Date format** to `YYYY-MM-DD`
3. Set **New file location** to wherever you want daily notes (vault root or a `Daily/` folder)
4. Set **Template file location** to `Templates/Daily Note`

### Step 6: Make It Look Good (Optional but Recommended)

A few tweaks to make Obsidian feel compact and IDE-like:

1. Go to **Settings > Appearance > Themes > Manage**
2. Search for **"Things"** — install and activate it (clean, task-focused design)
3. Set **Zoom level** to around **83%** (drag the slider left) — makes everything tighter
4. Toggle off **Show inline title** — saves vertical space
5. Adjust **Font size** to 14-15 if the default feels too large
6. Install **Style Settings** plugin (Community plugins > Browse > "Style Settings") — gives you a GUI to customize the Things theme colors, heading styles, and more without writing CSS. Find it under **Settings > Style Settings > Things Theme**.

This takes a minute and makes a big difference in how usable the daily note feels.

### Step 6b: Custom Folder Sorting (Optional)

By default, Obsidian sorts folders alphabetically. Two ways to customize:

**Option A — Number prefixes** (no plugin): Rename folders like `01 Projects`, `02 People`, `03 Archive`. Simple, works everywhere.

**Option B — Custom Sort plugin** (recommended): Define your own sidebar order via a `sortspec.md` file.

1. Install **"Custom Sort"** by SebastianMC (Community plugins > Browse > "Custom Sort")
2. Enable it
3. Create a `sortspec.md` file in your vault root with YAML frontmatter:

```yaml
---
sorting-spec: |-
  target-folder: /
  Inbox
  Completed
  Daily Notes
  Projects
  People
  Templates
---
```

4. `Cmd+P` > type "custom s" > select **"Custom File Explorer sorting: Enable and apply the custom sorting... Sort-on."**

List folders and files by name (without `.md` extension) in the order you want. Files and folders are mixed together — no more files stuck at the bottom.

### Step 7: Create an Inbox Note

Create a note called `Inbox` in your vault root. This is your quick-capture space:

```markdown
# Inbox

Quick capture space for tasks, thoughts, and ideas. Process these during your daily review.

---

```

---

## How to Use It

### Opening Your Daily Note

- Click the **calendar icon** in the left sidebar, or
- Press `Cmd+P` (Mac) / `Ctrl+P` (Windows) > type **"Open daily note"**

This creates today's note from the template. The query blocks automatically show your tasks.

### Writing Tasks (The Right Way)

Write tasks **in context** — in meeting notes, project docs, 1:1 follow-ups — not in a separate TODO file.

Always add a due date so the daily note can find them:

```markdown
## Team Standup — 2026-03-25
- Discussed deployment timeline
- [ ] Review PR for auth service 📅 2026-03-27
- [ ] Send staging env access to the new hire 📅 2026-03-26
- Decision: postpone launch to April
```

The `📅 YYYY-MM-DD` is how the Tasks plugin knows when something is due. Without it, the task won't appear in the "Due Today" or "Overdue" sections (it will still appear in the "No Due Date" section at the bottom).

### Task Format Reference

| What to type | Meaning |
|-------------|---------|
| `📅 2026-03-25` | Due date |
| `⏳ 2026-03-25` | Scheduled date (when you plan to work on it) |
| `🛫 2026-03-25` | Start date (don't show before this date) |
| `⏫` | High priority |
| `🔼` | Medium priority |
| `🔽` | Low priority |
| `🔁 every week` | Recurring task |

### Task Statuses

| Checkbox | Meaning | How to type |
|----------|---------|-------------|
| `- [ ]` | To do | Normal checkbox |
| `- [?]` | Pending / waiting on someone | Type `?` inside the brackets |
| `- [x]` | Done | Click checkbox or type `x` |

Mark a task as **pending** when you're waiting on someone else — change `[ ]` to `[?]` and it moves from "Due Today" into the "Pending" section of your daily note. Right-click any checkbox to see "Pending" as an option. Useful for tracking what's blocked without losing sight of it.

**Don't want to memorize emojis?** Use the GUI:
1. Put your cursor on an empty line
2. Press `Cmd+P` > type **"Tasks: Create or edit task"**
3. A form pops up with fields for description, due date, priority, recurrence — all with dropdowns and date pickers

### Quick Capture

For thoughts that come up outside of a meeting or project context:

1. Open your daily note
2. Scroll to the **Quick Capture** section
3. Type your task:
   ```
   - [ ] Research options for the Q2 offsite 📅 2026-03-28
   ```

Or add it to `Inbox.md` if you want to process it later.

### Checking Off Tasks

- **In the daily note**: In Reading view, click the checkbox next to any task in a query block. The Tasks plugin updates the **original file** automatically.
- **In the source file**: Navigate to the meeting note / project doc and check it off there.

Both work — the query is live, so changes are reflected everywhere instantly.

### Reading View vs. Editing View

- **Reading view** (default): Query blocks render as nice task lists. You can click checkboxes. But you can't type into the query blocks — they're read-only.
- **Editing view** (`Cmd+E` to toggle): You see the raw markdown. Query blocks show as code. You can edit anything.

Use Reading view for your daily workflow. Switch to Editing view when you need to modify the template structure or add content in specific sections.

---

## Daily Routine

### Morning (~2 min)
1. Open your daily note (`Cmd+P` > "Open daily note")
2. Glance at **Due Today** and **Overdue** sections
3. Write your top 1-3 priorities in the **Focus** section

### During the Day
- In meetings: write tasks inline with `📅` due dates
- Random thought? Add it to **Quick Capture** in the daily note or to `Inbox.md`
- Use `Cmd+P` > "Tasks: Create or edit task" for the GUI form

### End of Day (~2 min)
1. Check off anything you completed — they move into the **Completed Today** section automatically
2. Scroll down for the time-based breakdown: Tomorrow, This Week, Next Week, Future
3. Optionally jot a note in the **Notes** section about what carries forward

**Bonus:** Create a `Completed.md` note in your vault root for a bird's-eye view of all completed tasks across the vault, broken down by This Week / This Month / Older. Use `done after 7 days ago` date syntax in the Tasks queries.

---

## Tips and Best Practices

### Do
- **Write tasks at their source** — meeting notes, project docs, 1:1 follow-ups. The daily note queries pull them in automatically.
- **Always add `📅 YYYY-MM-DD`** to tasks you want surfaced by date. Tasks without due dates still appear in the "No Due Date" section but won't show in "Due Today."
- **Use Inbox.md** for quick captures that don't belong to a specific meeting or project.
- **Check off tasks from the daily note** — the Tasks plugin updates the original file for you.

### Don't
- **Don't copy tasks between files** — that's what queries solve. You'll end up with stale duplicates.
- **Don't put all tasks in one giant TODO file** — scatter them at their point of origin and let queries aggregate them.
- **Don't over-engineer your template** — start simple. Add sections only when you find yourself needing them repeatedly.

---

## Optional: Advanced Setup

### Dataview Plugin (for power users)
Dataview lets you write more flexible queries using its own query language (DQL). Useful for weekly reviews, project dashboards, or custom aggregations.

Install: **Settings** > **Community plugins** > Browse > search **"Dataview"** > Install & Enable

Example — all incomplete tasks from this week's meeting notes:
````markdown
```dataview
TASK
FROM "Meetings"
WHERE !completed
SORT file.name ASC
```
````

### Templater Plugin (for dynamic templates)
Templater lets you use JavaScript expressions in templates — date math, conditionals, prompts. Useful if you want daily notes that reference yesterday's note or calculate "next Friday."

Install: **Settings** > **Community plugins** > Browse > search **"Templater"** > Install & Enable

### Weekly Review Template
Create a `Templates/Weekly Review.md` that summarizes completed tasks:
````markdown
# Weekly Review — {{date}}

## Completed This Week
```tasks
done
done after {{date:YYYY-MM-DD}} - 7 days
done before {{date:YYYY-MM-DD}} + 1 day
group by filename
```

## Still Open
```tasks
not done
due before {{date:YYYY-MM-DD}} + 1 day
sort by due
```

## Reflections
-
````

---

## The Template (Copy-Paste Ready)

If you want to set this up from scratch, create `Templates/Daily Note.md` with this exact content:

````markdown
# {{date}}

## Focus
-

## Due Today
```tasks
not done
due on {{date}}
status.name does not include Pending
sort by priority
```

## Pending (waiting on others)
```tasks
status.name includes Pending
sort by priority
group by folder
```

## Overdue
```tasks
not done
due before {{date}}
sort by priority
group by folder
```

## Quick Capture
-

## Notes


## Completed Today
```tasks
done
done on {{date}}
sort by done reverse
group by folder
```

## Tomorrow
```tasks
not done
due on tomorrow
sort by priority
group by folder
```

## This Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().add(1, 'day'), 'day') && task.due.moment?.isSameOrBefore(moment().endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Next Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().endOf('isoWeek'), 'day') && task.due.moment?.isSameOrBefore(moment().add(1, 'week').endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Future
```tasks
not done
filter by function !!task.due.moment?.isAfter(moment().add(1, 'week').endOf('isoWeek'), 'day')
sort by due
group by folder
```

## No Due Date
```tasks
not done
no due date
sort by path
group by folder
```
````

Then configure: **Settings** > **Daily notes** > Template file location > `Templates/Daily Note`

That's it. Open your daily note tomorrow and start writing tasks with `📅` dates. They'll show up automatically.
