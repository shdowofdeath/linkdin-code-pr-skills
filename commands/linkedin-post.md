# /linkedin-post

Create a LinkedIn post sharing what you're building, based on auto-analysis of your current git repo. All IP and PII is stripped automatically.

## Instructions

Use the `linkedin-code-post` skill in **Project Share** mode to complete this task.

1. **Analyze the current repo** following `references/repo-analysis-guide.md`:
   - Read README, manifest files, detect tech stack
   - Check recent git activity
   - Estimate project size and maturity

2. **Strip PII/IP** following `references/pii-ip-safety-rules.md`:
   - Never read .env or credential files
   - Strip company names, internal details, secrets
   - List what was generalized and ask user if they want to restore anything

3. **Generate a post draft** using `references/project-share-template.md`:
   - Under 1300 characters, text-only (no code snippets)
   - Show the draft and wait for user approval

4. **Post to LinkedIn** via browser automation:
   - Follow `references/linkedin-posting-workflow.md` (skip image attachment)
   - Always ask before clicking Post
