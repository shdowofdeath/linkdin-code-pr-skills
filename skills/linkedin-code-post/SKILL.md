---
name: linkedin-code-post
description: Creates LinkedIn posts showcasing code or PR changes with styled code screenshots from Carbon. Use when user says "linkedin post", "share on linkedin", "post code to linkedin", "create linkedin post", or wants to share code/PR on social media.
allowed-tools: mcp__claude-in-chrome__tabs_context_mcp, mcp__claude-in-chrome__tabs_create_mcp, mcp__claude-in-chrome__navigate, mcp__claude-in-chrome__read_page, mcp__claude-in-chrome__find, mcp__claude-in-chrome__computer, mcp__claude-in-chrome__form_input, mcp__claude-in-chrome__javascript_tool, mcp__claude-in-chrome__get_page_text, mcp__claude-in-chrome__upload_image, mcp__claude-in-chrome__update_plan, Bash(git log:*), Bash(git diff:*), Bash(git show:*), Bash(gh pr view:*), Bash(gh pr diff:*), Read
---

# LinkedIn Code Post Skill

Create engaging LinkedIn posts that showcase code you wrote or PRs you worked on, complete with styled Carbon code screenshots and browser-automated posting.

## When to Use

- User says "linkedin post", "share on linkedin", "post code to linkedin"
- User wants to share a code snippet, function, or module they wrote
- User wants to share a PR they opened or merged
- User says "create a post about this code/PR"

## When NOT to Use

- User wants to post on a platform other than LinkedIn
- User wants a text-only post with no code screenshot
- User wants to schedule a post for later (not supported)
- User wants to post someone else's code without attribution

## Quick Reference: Format Selection

| Scenario | Format | Key Focus |
|----------|--------|-----------|
| Just wrote code/function/module | Code Showcase | Problem solved, approach, key decisions |
| Merged or opened a PR | PR Review | Changes summary, approach, lessons learned |
| User says "share this code" | Code Showcase | The specific code in context |
| User says "share this PR" | PR Review | PR metadata + diff summary |

If the format is unclear, ask the user.

## Workflow

### Phase 1: Context Gathering

**For Code Showcase:**
1. Read the code file(s) the user wants to showcase using `Read`
2. If needed, check recent git history for context:
   ```
   git log --oneline -10
   git diff HEAD~1 -- <file>
   ```
3. Identify the most visually interesting snippet (15-30 lines max)
4. Understand what problem the code solves

**For PR Review:**
1. Get PR metadata:
   ```
   gh pr view <number> --json title,body,additions,deletions,files,url
   ```
2. Get the full diff:
   ```
   gh pr diff <number>
   ```
3. Read key changed files for context
4. Identify the most interesting code change for the screenshot (15-30 lines)

**Output of Phase 1:** Selected code snippet + context about what it does and why.

### Phase 2: Post Content Generation

1. Choose the appropriate template from `references/post-templates.md`
2. Fill in the template using gathered context
3. Ensure post is under 1300 characters
4. Present the draft to the user:
   - Show the post text
   - Show which code snippet will be used for the screenshot
   - Ask for approval or edits
5. Iterate until user approves

**IMPORTANT:** Do NOT proceed to Phase 3 until the user explicitly approves the draft.

### Phase 3: Code Screenshot via Carbon

Follow the detailed steps in `references/carbon-screenshot-workflow.md`.

Summary:
1. Call `update_plan` with domains `["carbon.now.sh"]` and approach
2. Get browser context with `tabs_context_mcp`
3. Create a new tab with `tabs_create_mcp`
4. Build Carbon URL with the code snippet and recommended settings
5. Navigate to the URL
6. Wait for render, take screenshot
7. Store the `imageId` for Phase 4

### Phase 4: LinkedIn Post Creation

Follow the detailed steps in `references/linkedin-posting-workflow.md`.

Summary:
1. Call `update_plan` adding `"linkedin.com"` to domains
2. Navigate to LinkedIn feed
3. Verify user is logged in (STOP if not)
4. Click "Start a post"
5. Type post content in chunks (paragraph by paragraph)
6. Attach the Carbon screenshot using `upload_image`
7. Verify the preview looks correct

### Phase 5: User Review & Publish

1. Take a final screenshot of the composed post
2. Ask the user: "Your post is ready for review. Would you like me to publish it, or do you want to review and click Post yourself?"
3. **If the user says to publish** -- click the Post button
4. **If the user wants to review first** -- stop and let them handle it

## Common Mistakes

| Mistake | Why It's Wrong | What to Do Instead |
|---------|---------------|-------------------|
| Clicking Post/Publish without asking | User must confirm before publishing | Always ask user before clicking Post |
| Code snippet too long (>30 lines) | Unreadable in LinkedIn image | Trim to 15-30 most interesting lines |
| Post text too long (>1300 chars) | LinkedIn truncates, lower engagement | Keep concise, use line breaks |
| Skipping user draft approval | User may want different angle/tone | Always show draft and wait for approval |
| Typing all text at once | LinkedIn contenteditable can glitch | Type paragraph by paragraph with Enter keys |
| Not verifying LinkedIn login | Automation will fail on login page | Always check for feed content before proceeding |
| Attempting to log in for user | Security violation | Tell user to log in manually, then retry |

## Hard Rules

1. **NEVER click Post or Publish on LinkedIn without explicit user confirmation** -- always ask first
2. **Max 15-30 lines** for the code screenshot -- keep it readable
3. **Always show the draft** to the user before creating the screenshot
4. **Always verify LinkedIn login** before attempting to post
5. **NEVER attempt to log in** on behalf of the user
6. **Always use `update_plan`** before starting browser automation
7. **Post must be under 1300 characters** for optimal engagement
8. **Type content in chunks** -- paragraph by paragraph, not all at once
9. **If Carbon or LinkedIn UI changes** and automation breaks, stop and inform the user

## References

- `references/post-templates.md` -- Post templates for Code Showcase and PR Review formats
- `references/carbon-screenshot-workflow.md` -- Carbon.now.sh browser automation steps
- `references/linkedin-posting-workflow.md` -- LinkedIn browser automation steps
- `references/code-showcase-examples.md` -- Example Code Showcase posts
- `references/pr-review-examples.md` -- Example PR Review posts
