---
name: linkedin-code-post
description: "Two modes: (1) Project Share -- auto-analyzes your git repo and creates a LinkedIn text post about what you're building, with all IP/PII stripped. (2) Job Match -- compares your skills (from repos + resume) against job listings from local files. Use when user says 'linkedin post', 'share project', 'share what I'm working on', 'job match', 'match skills', 'compare resume', 'check job fit'."
allowed-tools: mcp__claude-in-chrome__tabs_context_mcp, mcp__claude-in-chrome__tabs_create_mcp, mcp__claude-in-chrome__navigate, mcp__claude-in-chrome__read_page, mcp__claude-in-chrome__find, mcp__claude-in-chrome__computer, mcp__claude-in-chrome__form_input, mcp__claude-in-chrome__javascript_tool, mcp__claude-in-chrome__get_page_text, mcp__claude-in-chrome__update_plan, Bash(git log:*), Bash(git diff:*), Bash(git show:*), Bash(find:*), Bash(wc:*), Read, Glob, Grep
---

# LinkedIn Project Share & Job Match Skill

Two modes for developers:
1. **Project Share** -- analyze your current repo and create a LinkedIn post about what you're building (text-only, no IP/PII)
2. **Job Match** -- compare your skills against job descriptions to see how well you fit

## Mode Selection

| User Says | Mode |
|-----------|------|
| "linkedin post", "share project", "share on linkedin", "post about what I'm working on" | **Project Share** |
| "job match", "match skills", "check job fit", "compare resume", "am I qualified" | **Job Match** |

If the intent is unclear, ask the user which mode they want.

## When NOT to Use

- User wants to post on a platform other than LinkedIn (Mode 1)
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

## Hard Rules

1. **NEVER click Post or Publish on LinkedIn without explicit user confirmation** -- always ask first
2. **Always show the draft** to the user before posting (Mode 1)
3. **Always verify LinkedIn login** before attempting to post (Mode 1)
4. **NEVER attempt to log in** on behalf of the user
5. **Always use `update_plan`** before starting browser automation (Mode 1)
6. **Post must be under 1300 characters** (Mode 1)
7. **Type content in chunks** -- paragraph by paragraph, not all at once (Mode 1)
8. **NEVER include code snippets** in the LinkedIn post -- text only (Mode 1)
9. **NEVER read .env or credential files** during repo analysis
10. **Always strip PII/IP** following the safety rules before generating post content (Mode 1)
11. **Mode 2 does NOT use browser automation** -- all output is in the terminal
12. **If LinkedIn UI changes** and automation breaks, stop and inform the user

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

## References

- `references/repo-analysis-guide.md` -- How to scan a repo for project metadata and skills
- `references/pii-ip-safety-rules.md` -- Rules for stripping sensitive information from posts
- `references/project-share-template.md` -- Post template for Project Share mode
- `references/project-share-examples.md` -- Example Project Share posts
- `references/job-match-workflow.md` -- End-to-end Job Match workflow
- `references/job-match-output-format.md` -- Match report output format
- `references/linkedin-posting-workflow.md` -- LinkedIn browser automation steps
