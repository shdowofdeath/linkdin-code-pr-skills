# LinkedIn Developer Skills

Claude Code skills for sharing what you're building on LinkedIn, matching your skills against job listings, and showcasing your engineering work.

## Why

Developers build cool stuff every day but rarely share it. Writing a LinkedIn post feels like a chore -- figuring out what to say, making sure you don't leak anything proprietary. So most work stays invisible.

On the flip side, when you're job hunting, it's hard to objectively assess how well your skills match a role's requirements.

These skills remove those frictions:
1. **Project Share** turns your current repo into a ready-to-publish LinkedIn post in seconds, with all IP and PII stripped automatically
2. **Job Match** analyzes your repos and resume against job descriptions and tells you exactly where you stand
3. **PR Share** turns a specific PR into an engaging post about the engineering lesson behind the change
4. **Skills Portfolio** scans your repos and generates a comprehensive skills summary post
5. **Weekly Digest** auto-generates a "what I built this week" post from your git activity

## What It Does

### Project Share (`/linkedin-post`)

Auto-analyzes your current git repo and creates a LinkedIn text post about what you're building:
- Scans README, package files, git history, and tech stack
- Strips all proprietary information, secrets, and PII automatically
- Generates an engaging post draft for your approval
- Posts to LinkedIn via browser automation (you confirm before publishing)

### Job Match (`/job-match`)

Reads job descriptions from local files and compares them against your skills:
- Extracts skills from your git repos (languages, frameworks, databases, infra, etc.)
- Extracts skills from your resume (if provided)
- Cross-references both sources for verified skill evidence
- Outputs a detailed match report with percentage, gaps, and recommendations

### PR Share (`/pr-share`)

Share a specific PR you worked on as a LinkedIn post:
- Provide a GitHub PR URL, branch name, or just say "my latest PR"
- Analyzes the change: what it does, why it matters, what the engineering lesson is
- Strips all proprietary details, code diffs, ticket numbers, and internal references
- Generates a post about the engineering insight, not the specific code
- Posts to LinkedIn via browser automation (you confirm before publishing)

### Skills Portfolio (`/skills-portfolio`)

Analyze your repos and generate a skills summary post for LinkedIn:
- Scans one or more repos for languages, frameworks, databases, infra, testing, CI/CD
- Aggregates and ranks skills by evidence strength across repos
- Optionally cross-references with your resume
- Generates a post with skills in context (not just a list)
- Posts to LinkedIn via browser automation (you confirm before publishing)

### Weekly Digest (`/weekly-digest`)

Auto-generate a "what I built this week" post from your git activity:
- Scans git history across one or more repos (defaults to last 7 days)
- Groups activity by theme: features, bug fixes, refactoring, tests, docs
- Identifies your top 2-3 accomplishments and an interesting challenge
- Generates a conversational weekly update post
- Posts to LinkedIn via browser automation (you confirm before publishing)

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- For posting modes (Project Share, PR Share, Skills Portfolio, Weekly Digest): [Claude in Chrome](https://chromewebstore.google.com/detail/claude-in-chrome/) extension installed and connected, logged into LinkedIn
- For Job Match: no additional requirements (runs entirely in the terminal)

## Installation

Copy the skills and commands to your Claude Code config directory:

```bash
cp -R skills/* ~/.claude/skills/
cp -R commands/* ~/.claude/commands/
```

## Usage

### Share what you're building

In any Claude Code session inside a git repo:

```
/linkedin-post
```

Claude will:
1. Analyze your repo (README, dependencies, git history, tech stack)
2. Strip all IP/PII and show you what was generalized
3. Draft a post and show it for your approval
4. Open LinkedIn and compose the post
5. **Ask if you want to publish or review first**

### Match your skills to jobs

```
/job-match
```

Claude will:
1. Ask for your job description file(s) and optional resume
2. Scan your repo(s) for technical skills
3. Parse your resume for experience and listed skills
4. Output a match report with Strong/Partial/Missing skill analysis
5. Provide recommendations for closing gaps

### Share a PR you worked on

```
/pr-share
```

Claude will:
1. Ask for a PR URL, branch name, or use your latest PR
2. Analyze the change -- what it does, why, and what's interesting
3. Strip all proprietary details and code diffs
4. Draft a post about the engineering lesson
5. Open LinkedIn and compose the post
6. **Ask if you want to publish or review first**

### Generate a skills portfolio post

```
/skills-portfolio
```

Claude will:
1. Ask for repo paths to analyze (defaults to current repo)
2. Scan repos for technical skills and aggregate across projects
3. Rank skills by evidence strength with context
4. Draft a skills summary post
5. Open LinkedIn and compose the post
6. **Ask if you want to publish or review first**

### Generate a weekly digest post

```
/weekly-digest
```

Claude will:
1. Ask for repo paths and date range (defaults to last 7 days)
2. Scan git history for commits and group by theme
3. Identify top accomplishments and learnings
4. Draft a weekly update post
5. Open LinkedIn and compose the post
6. **Ask if you want to publish or review first**

## Safety

- **IP/PII Protection**: The skill never reads `.env`, credential files, or secrets. Company names, internal metrics, and proprietary details are stripped by default.
- **User Control**: You always see what was stripped and can choose to restore specific items. You always approve the draft before posting.
- **LinkedIn Safety**: The skill always asks before clicking Post. You can review and publish manually if you prefer.
- **No Code Diffs**: PR Share never includes code in the post -- it's about the engineering lesson, not the implementation.

## Project Structure

```
skills/
  linkedin-code-post/
    SKILL.md                              # Core skill definition (five modes)
    references/
      repo-analysis-guide.md              # How to scan repos for metadata and skills
      pii-ip-safety-rules.md              # IP/PII stripping rules
      project-share-template.md           # Post template for Project Share
      project-share-examples.md           # Example Project Share posts
      job-match-workflow.md               # Job Match end-to-end workflow
      job-match-output-format.md          # Match report output format
      linkedin-posting-workflow.md        # LinkedIn browser automation steps
      pr-share-workflow.md                # PR Share end-to-end workflow
      pr-share-template.md               # Post template + examples for PR Share
      skills-portfolio-workflow.md        # Skills Portfolio end-to-end workflow
      skills-portfolio-template.md        # Post template + examples for Skills Portfolio
      weekly-digest-workflow.md           # Weekly Digest end-to-end workflow
      weekly-digest-template.md           # Post template + examples for Weekly Digest
commands/
  linkedin-post.md                        # /linkedin-post slash command
  job-match.md                            # /job-match slash command
  pr-share.md                             # /pr-share slash command
  skills-portfolio.md                     # /skills-portfolio slash command
  weekly-digest.md                        # /weekly-digest slash command
```
