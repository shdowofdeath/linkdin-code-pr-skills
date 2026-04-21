---
name: linkedin-code-post
description: "Five modes: (1) Project Share -- auto-analyzes your git repo and creates a LinkedIn text post about what you're building. (2) Job Match -- compares your skills against job listings. (3) PR Share -- share a specific PR as a LinkedIn post about the engineering lesson. (4) Skills Portfolio -- analyze repos to generate a skills summary post. (5) Weekly Digest -- auto-generate a 'what I built this week' post from git activity. Use when user says 'linkedin post', 'share project', 'job match', 'match skills', 'share PR', 'post about PR', 'skills portfolio', 'share my skills', 'weekly digest', 'what I built this week'."
allowed-tools: mcp__claude-in-chrome__tabs_context_mcp, mcp__claude-in-chrome__tabs_create_mcp, mcp__claude-in-chrome__navigate, mcp__claude-in-chrome__read_page, mcp__claude-in-chrome__find, mcp__claude-in-chrome__computer, mcp__claude-in-chrome__form_input, mcp__claude-in-chrome__javascript_tool, mcp__claude-in-chrome__get_page_text, mcp__claude-in-chrome__update_plan, Bash(git log:*), Bash(git diff:*), Bash(git show:*), Bash(gh pr view:*), Bash(find:*), Bash(wc:*), Read, Glob, Grep
---

# LinkedIn Developer Skills

Five modes for developers:
1. **Project Share** -- analyze your current repo and create a LinkedIn post about what you're building (text-only, no IP/PII)
2. **Job Match** -- compare your skills against job descriptions to see how well you fit
3. **PR Share** -- share a specific PR as a LinkedIn post about the engineering lesson behind the change
4. **Skills Portfolio** -- analyze your repos and generate a comprehensive skills summary post
5. **Weekly Digest** -- auto-generate a "what I built this week" post from your git activity

## Mode Selection

| User Says | Mode |
|-----------|------|
| "linkedin post", "share project", "share on linkedin", "post about what I'm working on" | **Project Share** |
| "job match", "match skills", "check job fit", "compare resume", "am I qualified" | **Job Match** |
| "share PR", "post about PR", "share pull request", "pr share" | **PR Share** |
| "skills portfolio", "share my skills", "skills summary", "tech profile" | **Skills Portfolio** |
| "weekly digest", "what I built this week", "weekly update", "share my week" | **Weekly Digest** |

If the intent is unclear, ask the user which mode they want.

## When NOT to Use

- User wants to post on a platform other than LinkedIn (posting modes)
- User wants to schedule a post for later (not supported)
- User wants to share someone else's code
- User wants to post a code screenshot (this skill is text-only posts)

---

## Mode 1: Project Share

Create a LinkedIn post that lets people know what you're building, without exposing any intellectual property or personal information.

### Phase 1: Repo Analysis

Follow `references/repo-analysis-guide.md` to analyze the current git repository.

Gather:
- Project name and description
- Primary languages and frameworks
- Key dependencies and tech stack
- Recent git activity and focus areas
- Project size and maturity

### Phase 2: PII/IP Stripping

Follow `references/pii-ip-safety-rules.md` to ensure no sensitive information leaks.

Key rules:
- **Never read** `.env`, credential files, secrets directories
- **Always strip** API keys, tokens, connection strings, IP addresses
- **Default strip** company name, project codenames, business metrics (user can override)
- **Before showing draft**, list what was generalized and ask user if they want to restore anything

### Phase 3: Post Generation

Use the template from `references/project-share-template.md`.

Fill in:
- **hook_line** -- what makes the project interesting (problem being solved)
- **what_im_building** -- high-level project description
- **tech_approach** -- interesting technical decisions
- **current_status** -- where you are, what's next
- **cta_question** -- engagement hook
- **hashtags** -- 3-5 relevant tags

See `references/project-share-examples.md` for reference.

Constraints:
- Under 1300 characters total
- No code snippets in the post
- Focus on "why" and "what", not implementation details
- First person, conversational tone

### Phase 4: User Approval

Present the draft to the user:
- Show the full post text
- Show what was stripped/generalized (from Phase 2)
- Ask for approval or edits
- Iterate until the user approves

**IMPORTANT:** Do NOT proceed to Phase 5 until the user explicitly approves the draft.

### Phase 5: LinkedIn Posting

Follow `references/linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).

Summary:
1. Call `update_plan` with domains `["linkedin.com"]`
2. Get browser context, create a new tab
3. Navigate to LinkedIn feed
4. Verify user is logged in (STOP if not)
5. Click "Start a post"
6. Type post content in chunks (paragraph by paragraph)
7. Verify the post preview

### Phase 6: User Review & Publish

1. Take a final screenshot of the composed post
2. Ask the user: "Your post is ready. Would you like me to publish it, or do you want to review and click Post yourself?"
3. **If user says to publish** -- click the Post button
4. **If user wants to review** -- stop and let them handle it

---

## Mode 2: Job Match

Read job descriptions from local files, analyze your skills from repos and resume, and output a detailed match report.

**No browser automation is used in this mode.** All output is in the terminal.

### Phase 1: Gather Inputs

Ask the user for:
1. **Job description file(s)** -- path to a file or directory (required)
2. **Resume file** -- path to resume (optional but recommended)
3. **Additional repos** -- paths to other repos to scan (optional, defaults to current repo only)

### Phase 2: Read Job Descriptions

Follow `references/job-match-workflow.md` Phase 1.

From each job description, extract:
- Job title
- Required skills
- Preferred skills
- Years of experience
- Education requirements
- Domain knowledge

### Phase 3: Skills Extraction from Repos

Follow `references/repo-analysis-guide.md` to analyze repos.
Follow `references/job-match-workflow.md` Phase 2 to map findings to skill categories.

Categories: Languages, Frameworks, Databases, Cloud/Infrastructure, Testing, CI/CD, Architecture, Tools.

### Phase 4: Skills Extraction from Resume

Follow `references/job-match-workflow.md` Phase 3.

Extract skills, experience, education, certifications from the resume file.
Cross-reference with repo skills: tag as Verified / Claimed / Demonstrated.

### Phase 5: Match Analysis

Follow `references/job-match-workflow.md` Phase 4.

For each required skill: categorize as Strong Match / Partial Match / Missing.
Calculate match percentage and assign verdict.

### Phase 6: Output Report

Follow `references/job-match-output-format.md`.

Display:
- Per-job match report with evidence for each skill
- Missing skill recommendations
- Experience alignment assessment
- If multiple jobs: comparison summary table with best fit and top skills to develop

---

## Mode 3: PR Share

Share a specific PR you worked on as a LinkedIn post -- explain the technical change in an engaging way without exposing code or proprietary details.

### Phase 1: Get PR Information

Follow `references/pr-share-workflow.md` Phase 1.

Get from the user:
- A GitHub PR URL, or a local branch name, or "my latest PR"
- Use `gh pr view` or `git log`/`git diff` to extract: title, description, files changed, diff summary

### Phase 2: Analyze the Change

Follow `references/pr-share-workflow.md` Phase 2.

Categorize the PR:
- Bug fix, new feature, refactor, performance improvement, migration, or infrastructure change
- Identify interesting technical decisions, tradeoffs, and non-obvious aspects

### Phase 3: PII/IP Stripping

Follow `references/pii-ip-safety-rules.md` and `references/pr-share-workflow.md` Phase 3.

Additional PR-specific rules:
- Strip PR numbers and ticket references
- Strip specific file paths that reveal internal architecture
- Generalize business logic descriptions
- Remove reviewer names and team references
- **Never include code diffs or snippets**

### Phase 4: Post Generation

Use the template from `references/pr-share-template.md`.

Fill in:
- **hook_line** -- the engineering insight or problem
- **the_problem** -- what situation prompted the change
- **the_approach** -- the key technical decision
- **the_lesson** -- the takeaway for other engineers
- **cta_question** -- engagement hook
- **hashtags** -- 3-5 relevant tags

Constraints:
- Under 1300 characters total
- **NEVER include code diffs or snippets** -- the post is about the lesson, not the code
- **Generalize the change** -- describe the engineering approach, not the specific implementation
- First person, conversational tone

### Phase 5: User Approval

Present the draft to the user:
- Show the full post text
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until the user approves

**IMPORTANT:** Do NOT proceed to Phase 6 until the user explicitly approves the draft.

### Phase 6: LinkedIn Posting

Follow `references/linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).

### Phase 7: User Review & Publish

1. Take a final screenshot of the composed post
2. Ask the user: "Your post is ready. Would you like me to publish it, or do you want to review and click Post yourself?"
3. **If user says to publish** -- click the Post button
4. **If user wants to review** -- stop and let them handle it

---

## Mode 4: Skills Portfolio

Analyze your repos (and optionally resume) to generate a comprehensive skills summary post for LinkedIn.

### Phase 1: Gather Repos

Follow `references/skills-portfolio-workflow.md` Phase 1.

Ask the user for:
1. **Repo paths** -- one or more local repos to analyze (defaults to current repo)
2. **Resume file** -- optional path to resume for additional context

**Requirement:** Must scan at least one repo. Do not generate from resume alone.

### Phase 2: Multi-Repo Analysis

Follow `references/repo-analysis-guide.md` for each repo.
Follow `references/skills-portfolio-workflow.md` Phase 2.

Extract per repo: languages, frameworks, databases, infra, testing, CI/CD, architecture patterns.

### Phase 3: Skills Aggregation

Follow `references/skills-portfolio-workflow.md` Phase 3.

- Deduplicate skills across repos
- Rank by evidence strength (Strong / Solid / Emerging)
- Group into categories
- Cross-reference with resume if provided (Verified / Claimed / Demonstrated)

### Phase 4: PII/IP Stripping

Follow `references/pii-ip-safety-rules.md` and `references/skills-portfolio-workflow.md` Phase 4.

- Strip company names, project names, internal tool names
- Keep technology names (public knowledge)
- Generalize project descriptions

### Phase 5: Post Generation

Use the template from `references/skills-portfolio-template.md`.

Fill in:
- **hook_line** -- personal reflection on your skills journey
- **top_skills_with_context** -- 8-10 skills with evidence and experience context
- **currently_learning** -- what you're actively exploring
- **cta_question** -- engagement hook
- **hashtags** -- 3-5 relevant tags

Constraints:
- Under 1300 characters total
- Focus on breadth with context, not just lists
- Frame skills with experience ("3 years of TypeScript across 5 projects")
- Include what you're currently learning (shows growth mindset)
- First person, conversational tone

### Phase 6: User Approval

Present the draft to the user:
- Show the full post text
- Show the skills evidence summary
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until the user approves

**IMPORTANT:** Do NOT proceed to Phase 7 until the user explicitly approves the draft.

### Phase 7: LinkedIn Posting

Follow `references/linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).

### Phase 8: User Review & Publish

1. Take a final screenshot of the composed post
2. Ask the user: "Your post is ready. Would you like me to publish it, or do you want to review and click Post yourself?"
3. **If user says to publish** -- click the Post button
4. **If user wants to review** -- stop and let them handle it

---

## Mode 5: Weekly Digest

Auto-generate a "what I built this week" LinkedIn post from your git activity across repos.

### Phase 1: Gather Repos and Date Range

Follow `references/weekly-digest-workflow.md` Phase 1.

Ask the user for:
1. **Repo paths** -- one or more local repos to scan (defaults to current repo)
2. **Date range** -- defaults to last 7 days, user can override

### Phase 2: Git Activity Scan

Follow `references/weekly-digest-workflow.md` Phase 2.

For each repo, run `git log --since="7 days ago"` (or user-specified range) to extract:
- Commit count, files changed, commit messages, languages touched

**Requirement:** At least 1 commit must exist in the date range. If no commits found, inform the user and stop.

### Phase 3: Activity Aggregation

Follow `references/weekly-digest-workflow.md` Phase 3.

- Group commits by theme: feature work, bug fixes, refactoring, testing, docs, infrastructure
- Identify top 2-3 accomplishments
- Note cross-repo breadth if applicable

### Phase 4: PII/IP Stripping

Follow `references/pii-ip-safety-rules.md` and `references/weekly-digest-workflow.md` Phase 4.

- Strip ticket numbers from commit messages
- Strip internal project names and codenames
- Generalize feature descriptions
- Remove references to internal teams, sprint numbers

### Phase 5: Post Generation

Use the template from `references/weekly-digest-template.md`.

Fill in:
- **week_intro_hook** -- framing of the week's tone
- **top_accomplishments** -- 2-3 things you built/fixed/improved
- **challenge_or_learning** -- something that was hard or surprising
- **whats_next** -- what's coming next week
- **cta_question** -- engagement hook
- **hashtags** -- 3-5 relevant tags

Constraints:
- Under 1300 characters total
- Conversational tone ("This week I..." not "Completed the following:")
- Focus on accomplishments and learnings, not task lists
- Depth over breadth -- 2-3 things explored well beats 8 bullet points
- First person, conversational tone

### Phase 6: User Approval

Present the draft to the user:
- Show the full post text
- Show the activity summary
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until the user approves

**IMPORTANT:** Do NOT proceed to Phase 7 until the user explicitly approves the draft.

### Phase 7: LinkedIn Posting

Follow `references/linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).

### Phase 8: User Review & Publish

1. Take a final screenshot of the composed post
2. Ask the user: "Your post is ready. Would you like me to publish it, or do you want to review and click Post yourself?"
3. **If user says to publish** -- click the Post button
4. **If user wants to review** -- stop and let them handle it

---

## Hard Rules

1. **NEVER click Post or Publish on LinkedIn without explicit user confirmation** -- always ask first
2. **Always show the draft** to the user before posting (all posting modes)
3. **Always verify LinkedIn login** before attempting to post (all posting modes)
4. **NEVER attempt to log in** on behalf of the user
5. **Always use `update_plan`** before starting browser automation (all posting modes)
6. **Post must be under 1300 characters** (all posting modes)
7. **Type content in chunks** -- paragraph by paragraph, not all at once (all posting modes)
8. **NEVER include code snippets** in the LinkedIn post -- text only (all posting modes)
9. **NEVER read .env or credential files** during repo analysis
10. **Always strip PII/IP** following the safety rules before generating post content (all posting modes)
11. **Mode 2 does NOT use browser automation** -- all output is in the terminal
12. **If LinkedIn UI changes** and automation breaks, stop and inform the user
13. **PR Share must NEVER include code diffs** in the LinkedIn post
14. **PR Share must generalize the change** -- describe the engineering lesson, not the specific implementation
15. **Skills Portfolio must scan at least one repo** -- don't generate from resume alone
16. **Weekly Digest defaults to 7 days** -- user can override the date range
17. **Weekly Digest requires at least 1 commit** in the date range -- if no activity, inform the user

## Common Mistakes

| Mistake | Why It's Wrong | What to Do Instead |
|---------|---------------|-------------------|
| Including proprietary code in post | Exposes IP | Only describe projects at a high level |
| Mentioning company name without asking | May violate employment policies | Default to "my team", ask user to override |
| Clicking Post without asking | User must confirm | Always ask before publishing |
| Skipping user draft approval | User may want different angle | Always show draft first |
| Reading .env files during analysis | May contain secrets | Never read secret files |
| Using browser tools in Job Match mode | Not needed, wastes time | Job Match is terminal-only |
| Post text too long (>1300 chars) | LinkedIn truncates | Keep concise |
| Including specific business metrics | May be confidential | Generalize or remove |
| Including code diffs in PR Share | Exposes implementation details | Describe the lesson, not the code |
| Generating skills post without scanning repos | No evidence to back claims | Always scan at least one repo |
| Generating weekly digest with no commits | Nothing to share | Inform user and stop |
| Listing skills without context | Reads like a resume | Add experience context to each skill |

## References

- `references/repo-analysis-guide.md` -- How to scan a repo for project metadata and skills
- `references/pii-ip-safety-rules.md` -- Rules for stripping sensitive information from posts
- `references/project-share-template.md` -- Post template for Project Share mode
- `references/project-share-examples.md` -- Example Project Share posts
- `references/job-match-workflow.md` -- End-to-end Job Match workflow
- `references/job-match-output-format.md` -- Match report output format
- `references/linkedin-posting-workflow.md` -- LinkedIn browser automation steps
- `references/pr-share-workflow.md` -- PR Share end-to-end workflow
- `references/pr-share-template.md` -- Post template and examples for PR Share mode
- `references/skills-portfolio-workflow.md` -- Skills Portfolio end-to-end workflow
- `references/skills-portfolio-template.md` -- Post template and examples for Skills Portfolio mode
- `references/weekly-digest-workflow.md` -- Weekly Digest end-to-end workflow
- `references/weekly-digest-template.md` -- Post template and examples for Weekly Digest mode
