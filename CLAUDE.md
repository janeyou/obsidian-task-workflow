# Project — obsidian-task-workflow

## What
Open-source Obsidian task management system: Claude Code skills (`/daily`, `/setup-obsidian`), a daily note template, and two guide docs. Published at https://github.com/janeyou/obsidian-task-workflow

## Repo Structure
| Path | Purpose |
|------|---------|
| `skills/daily.md` | Claude Code `/daily` skill — surfaces tasks, adds/completes them conversationally |
| `skills/setup-obsidian.md` | Claude Code `/setup-obsidian` skill — automates full setup in one conversation |
| `template/Daily Note.md` | Obsidian daily note template with Tasks plugin query blocks |
| `How to use Claude Code for Obsidian CLI productivity.md` | Full guide: CLI + Claude Code workflow |
| `Obsidian Daily TODO Best Practices.md` | Standalone guide: Obsidian-only setup, no CLI |
| `README.md` | GitHub landing page with quick start instructions |
| `Twitter Thread Drafts - Obsidian TODO Workflow.md` | Personal — gitignored, not in repo |

## Key Details
- Vault location: `/Users/janeyou/Obsidian/Personal/obsidian-task-workflow`
- GitHub remote: `https://github.com/janeyou/obsidian-task-workflow.git` (HTTPS, not SSH)
- Obsidian Tasks plugin emoji format: `📅` due, `⏳` scheduled, `⏫⏫🔽` priority, `🔁` recurrence
- `- [?]` = pending/waiting status (custom Tasks plugin config)
- The `setup-obsidian.md` skill has escaped backticks (`\` \` \``) for nested code fences — intentional

## Preferences
- Keep skills generic (vault name/path as placeholders, not hardcoded)
- Twitter drafts stay gitignored — personal content
- MIT license
