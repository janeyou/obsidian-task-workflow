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
   - **Due This Week** — tasks due in the next 7 days
   - **No Due Date** — remaining open tasks (show source file for context)

4. **Present a summary** to the user:
   - Count of overdue / due today / due this week / no date
   - List the tasks grouped as above, with source file links
   - Ask if the user wants to: (a) add new tasks, (b) mark tasks done, (c) open the daily note in Obsidian

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
- The daily note in Obsidian shows live query results pulling from all files
- Use Inbox.md for quick standalone captures
- The user can also add tasks directly via: `obsidian daily:append content="- [ ] task 📅 date"`
