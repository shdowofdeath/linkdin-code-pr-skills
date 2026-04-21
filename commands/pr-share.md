# /pr-share

Share a specific PR you worked on as a LinkedIn post -- explain the technical change in an engaging way, without exposing code diffs or proprietary details.

## Instructions

Use the `linkedin-code-post` skill in **PR Share** mode to complete this task.

1. **Get PR info** from the user:
   - Ask for a PR URL (GitHub) or a local branch name
   - Use `gh pr view` or `git log`/`git diff` to extract: title, description, files changed, diff summary
   - Understand what the PR does: bug fix, feature, refactor, perf improvement, etc.

2. **Strip PII/IP** following `references/pii-ip-safety-rules.md`:
   - Never read .env or credential files
   - Strip company names, internal project names, ticket numbers, customer data
   - List what was generalized and ask user if they want to restore anything

3. **Generate a post draft** using `references/pr-share-template.md`:
   - Under 1300 characters, text-only (no code snippets, no diffs)
   - Focus on the engineering lesson, not the specific implementation
   - Show the draft and wait for user approval

4. **Post to LinkedIn** via browser automation:
   - Follow `references/linkedin-posting-workflow.md` (skip image attachment)
   - Always ask before clicking Post
