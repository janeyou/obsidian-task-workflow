# Setup Obsidian Daily TODO Workflow

Automated setup for a daily task management system in Obsidian. Installs plugins, creates templates, and configures everything so the user is ready to go in one conversation.

## Prerequisites

Before running this skill, the user must have:
1. Obsidian installed and open with a vault
2. Obsidian CLI enabled (Settings > General > Command line interface) and available in PATH

## Workflow

### Step 0: Verify CLI

1. Run `obsidian version` to confirm the CLI is working
2. Run `obsidian vaults` to list available vaults
3. Ask the user which vault to set up (or confirm if there's only one)

### Step 1: Install Tasks Plugin

```
obsidian plugin:install id=obsidian-tasks-plugin enable
```

This is the core plugin — it adds task query blocks that pull tasks from across the vault into a single view.

### Step 2: Configure Custom Task Status (Pending)

Read the Tasks plugin config at `<vault>/.obsidian/plugins/obsidian-tasks-plugin/data.json` and add a custom status for "Pending" (waiting on others):

```json
{
  "symbol": "?",
  "name": "Pending",
  "nextStatusSymbol": "x",
  "availableAsCommand": true,
  "type": "IN_PROGRESS"
}
```

Add this to the `statusSettings.customStatuses` array. Also ensure `setDoneDate` is `true` so completed tasks get a completion date.

### Step 3: Create Templates Folder and Daily Note Template

1. Create `Templates/` folder in the vault if it doesn't exist
2. Create `Templates/Daily Note.md` with this content:

```markdown
# {{date}}

## Focus
-

## Due Today
` ` `tasks
not done
due on {{date}}
status.name does not include Pending
sort by priority
` ` `

## Pending (waiting on others)
` ` `tasks
status.name includes Pending
sort by priority
group by folder
` ` `

## Overdue
` ` `tasks
not done
due before {{date}}
sort by priority
group by folder
` ` `

## Quick Capture
-

## Notes


## Completed Today
` ` `tasks
done
done on {{date}}
sort by done reverse
group by folder
` ` `

## Tomorrow
` ` `tasks
not done
due on tomorrow
sort by priority
group by folder
` ` `

## This Week
` ` `tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().add(1, 'day'), 'day') && task.due.moment?.isSameOrBefore(moment().endOf('isoWeek'), 'day'))
sort by priority
group by folder
` ` `

## Next Week
` ` `tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().endOf('isoWeek'), 'day') && task.due.moment?.isSameOrBefore(moment().add(1, 'week').endOf('isoWeek'), 'day'))
sort by priority
group by folder
` ` `

## Future
` ` `tasks
not done
filter by function !!task.due.moment?.isAfter(moment().add(1, 'week').endOf('isoWeek'), 'day')
sort by due
group by folder
` ` `

## No Due Date
` ` `tasks
not done
no due date
sort by path
group by folder
` ` `
```

IMPORTANT: The backticks above are shown with spaces for escaping — write them as proper triple backticks (```) in the actual file.

### Step 4: Configure Daily Notes

Write to `<vault>/.obsidian/daily-notes.json`:

```json
{
  "format": "YYYY-MM-DD",
  "template": "Templates/Daily Note"
}
```

Write to `<vault>/.obsidian/templates.json`:

```json
{
  "folder": "Templates"
}
```

### Step 5: Create Inbox

Create `Inbox.md` in the vault root:

```markdown
# Inbox

Quick capture space for tasks, thoughts, and ideas. Process these during your daily review.

---

```

### Step 6: Create Completed View

Create `Completed.md` in the vault root with task queries for:
- **This Week**: `done` + `done after 7 days ago`
- **This Month**: `done` + `done after 30 days ago` + `done before 7 days ago`
- **Older**: `done` + `done before 30 days ago`
- **No Completion Date**: `done` + `no done date`

All sorted by `done reverse`, grouped by `done` then `folder`.

IMPORTANT: Use `7 days ago` / `30 days ago` for relative dates — NOT `today -7 days` (that syntax doesn't work).

### Step 7: Theme and Appearance (Optional)

Present all four options at once. **Default is No for all** — the user opts in to what they want. Say something like:

"Your task workflow is fully set up! There are a few optional appearance tweaks you can apply now or do later. All default to **No** — pick any you'd like:"

| Option | What it does | Default |
|--------|-------------|---------|
| **a. Things theme** | Clean, task-focused theme + Style Settings plugin for customization | No |
| **b. Hide inline title** | Removes the large title at the top of each note, saves vertical space | No |
| **c. Compact zoom (83%)** | Reset zoom to 83% for a more compact task list view | No |
| **d. Custom sidebar sort** | Pin Inbox, Completed, Daily Notes to the top of the sidebar | No |

Apply only what the user selects:

**If a (theme):**
```
obsidian theme:install name="Things"
obsidian theme:set name="Things"
obsidian plugin:install id=obsidian-style-settings enable
```

**If b (hide inline title):**
Update `<vault>/.obsidian/app.json` to set `showInlineTitle`: false

**If c (compact zoom):**
Reset zoom to 83% for a more compact task list view. First reset to default, then zoom out 3 times:
```
obsidian command id="window:reset-zoom"
obsidian command id="window:zoom-out"
obsidian command id="window:zoom-out"
obsidian command id="window:zoom-out"
```
To adjust later: `Cmd + -` / `Cmd + +` to zoom, `Cmd + 0` to reset.

**If d (sidebar sort):**

1. Install the plugin:
   ```
   obsidian plugin:install id=custom-sort enable
   ```

2. List the user's existing vault folders (use `obsidian folders`) and create a `sortspec.md` in the vault root with all folders/files in a sensible order — pin Inbox, Completed, and Templates near the top:
   ```yaml
   ---
   sorting-spec: |-
     target-folder: /
     Inbox
     Completed
     Templates
     (list remaining folders/files by name, no .md extension)
   ---
   ```

3. Tell the user to activate it: `Cmd+P` > type "custom s" > select **"Custom File Explorer sorting: Enable and apply the custom sorting, (re)parsing the sorting configuration first. Sort-on."**

4. Reload the vault: `obsidian reload`

**After presenting results, always include this "do it later" note:**

> Skipped some options? You can apply them anytime:
> - **Theme**: Settings > Appearance > Themes > Manage > search "Things"
> - **Hide inline title**: Settings > Appearance > toggle off "Show inline title"
> - **Compact zoom**: `Cmd + -` to zoom out (3 times for ~83%), `Cmd + 0` to reset
> - **Sidebar sort**: Install "Custom Sort" plugin (Settings > Community plugins > Browse), create `sortspec.md` in your vault root listing folders in your preferred order, then `Cmd+P` > "custom s" > select "Custom File Explorer sorting: Enable and apply the custom sorting... Sort-on."

### Step 8: Reload and Open

```
obsidian reload
obsidian daily
```

### Step 9: Summary

Tell the user what was set up and how to use it:

1. **Open your daily note** each morning — click the calendar icon or `Cmd+P` > "Open daily note"
2. **Write tasks where they happen** — in meeting notes, project docs, 1:1s — with `📅 YYYY-MM-DD` due dates
3. **Check off tasks from the daily note** — the Tasks plugin updates the original file
4. **Use `- [?]` for pending** — tasks you're waiting on someone for (right-click checkbox > Pending)
5. **Priority emojis**: `⏫` high, `🔼` medium, `🔽` low
6. **Task GUI**: `Cmd+P` > "Tasks: Create or edit task" for a form with date pickers
7. **Completed.md** shows all completed tasks across the vault

Optionally suggest the `/daily` Claude Code skill for conversational task management.

## Notes

- The daily note template uses `{{date}}` which Obsidian's core Templates plugin replaces with today's date
- Task queries are live — they re-scan the vault every time you view the note
- Checking off a task in a query block updates the original source file
- The `status.name does not include Pending` filter keeps pending tasks out of Due Today (they show in their own section)
