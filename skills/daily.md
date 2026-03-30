# Daily Focus — Obsidian Task Workflow

Surface today's tasks, open the daily note, and optionally add new items.

## Vault

- **Name**: YOUR_VAULT_NAME
- **Path**: /path/to/your/vault

> Update the vault name and path above to match your Obsidian vault. Run `obsidian vaults` to find yours.

## Workflow

1. **Get today's date** using `date +%Y-%m-%d`

2. **Check if today's daily note exists** using `obsidian read path="{date}.md"`. If it doesn't exist, create it using the Obsidian CLI: `obsidian daily`

3. **Surface tasks due today and overdue** using the Obsidian CLI:
   ```
   obsidian tasks todo verbose
   ```
   Filter and present tasks in this order:
   - **Overdue** — tasks with due dates before today (show the due date and source file)
   - **Due Today** — tasks due today

   Upcoming tasks (Tomorrow, This Week, Next Week, Future, No Due Date) live in **Inbox.md** — the Today section there links to the daily note. Mention Inbox if the user asks about future tasks.

4. **Present a summary** to the user:
   - Count of overdue / due today
   - List the tasks grouped as above, with source file links
   - Ask if the user wants to: (a) add new tasks, (b) mark tasks done, (c) open the daily note in Obsidian, (d) open Inbox for upcoming tasks

5. **Adding new tasks**: When the user wants to add a task:
   - Ask what it is and when it's due (default: today)
   - Ask which file it belongs in (suggest Inbox.md for standalone items, or a specific meeting/project file)
   - Use `obsidian append path="<file>" content="- [ ] <task> 📅 <date>"` to add it
   - Confirm it was added

6. **Marking tasks done**: When the user wants to complete a task:
   - Use `obsidian task path="<file>" line=<n> done` to mark it complete
   - Confirm it was marked done

7. **Open daily note**: Use `obsidian daily` to open it in the app

## Task Format

Always use the Obsidian Tasks plugin emoji format:
- `📅 YYYY-MM-DD` for due dates
- `⏳ YYYY-MM-DD` for scheduled dates
- `⏫` high priority, `🔼` medium priority, `🔽` low priority

## Tips

- Tasks live at their source (meeting notes, 1:1 follow-ups, project docs) — never duplicate them
- The daily note focuses on today: Due Today, Overdue, Pending, Completed
- Upcoming views (Tomorrow → Future → No Due Date) live in Inbox.md — one place for planning ahead
- The daily note links to Inbox at the bottom for quick navigation
- Use Inbox.md for quick standalone captures
- The user can also add tasks directly via: `obsidian daily:append content="- [ ] task 📅 date"`
- When a new day starts, update the Today section link in Inbox.md to `[[Daily Notes/YYYY-MM-DD]]`

## Folder Filtering

Some folders contain grab-and-go checklists (e.g., packing lists in Travel/) that shouldn't clutter daily task views. These are excluded from Inbox's "No Due Date" query using `path does not include`. Actionable folders (Life/, Blogging/, Projects/) surface all tasks. Check the vault's `CLAUDE.md` for the current filtering rules.

## Changelog

| Date | Change |
|------|--------|
| 2026-03-28 | Moved Tomorrow/This Week/Next Week/Future/No Due Date from daily note to Inbox.md; daily note now links back to Inbox; added Today section in Inbox linking to daily note |
| 2026-03-28 | Removed redundant Today task query from Inbox — Today section is just a link to the daily note |
