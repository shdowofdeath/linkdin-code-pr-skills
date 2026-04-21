# /weekly-digest

Auto-generate a "what I built this week" LinkedIn post from your git activity across repos.

## Instructions

Use the `linkedin-code-post` skill in **Weekly Digest** mode to complete this task.

1. **Gather repos and date range** from the user:
   - Ask for paths to repos to scan (defaults to current repo)
   - Optional date range (defaults to last 7 days)
   - If no commits found in the date range, inform the user and stop

2. **Scan git activity** for each repo:
   - Run `git log --since="7 days ago"` (or user-specified range)
   - Extract: commits, files changed, themes of work
   - Group by theme: feature work, bug fixes, refactoring, docs, tests
   - Identify top 2-3 accomplishments

3. **Strip PII/IP** following `references/pii-ip-safety-rules.md`:
   - Strip commit messages referencing internal details, company names, ticket numbers
   - Keep only generalized descriptions of work
   - List what was generalized and ask user if they want to restore anything

4. **Generate a post draft** using `references/weekly-digest-template.md`:
   - Under 1300 characters, text-only
   - Focus on accomplishments and learnings, not task lists
   - Show the draft and wait for user approval

5. **Post to LinkedIn** via browser automation:
   - Follow `references/linkedin-posting-workflow.md` (skip image attachment)
   - Always ask before clicking Post
