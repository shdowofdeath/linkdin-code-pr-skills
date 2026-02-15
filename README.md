# LinkedIn Project Share & Job Match Skills

Claude Code skills for sharing what you're building on LinkedIn and matching your skills against job listings.

## Why

Developers build cool stuff every day but rarely share it. Writing a LinkedIn post feels like a chore -- figuring out what to say, making sure you don't leak anything proprietary. So most work stays invisible.

On the flip side, when you're job hunting, it's hard to objectively assess how well your skills match a role's requirements.

These skills remove both frictions:
1. **Project Share** turns your current repo into a ready-to-publish LinkedIn post in seconds, with all IP and PII stripped automatically
2. **Job Match** analyzes your repos and resume against job descriptions and tells you exactly where you stand

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

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- For Project Share: [Claude in Chrome](https://chromewebstore.google.com/detail/claude-in-chrome/) extension installed and connected, logged into LinkedIn
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

## Safety

- **IP/PII Protection**: The skill never reads `.env`, credential files, or secrets. Company names, internal metrics, and proprietary details are stripped by default.
- **User Control**: You always see what was stripped and can choose to restore specific items. You always approve the draft before posting.
- **LinkedIn Safety**: The skill always asks before clicking Post. You can review and publish manually if you prefer.

## Project Structure

```
skills/
  linkedin-code-post/
    SKILL.md                              # Core skill definition (two modes)
    references/
      repo-analysis-guide.md              # How to scan repos for metadata and skills
      pii-ip-safety-rules.md              # IP/PII stripping rules
      project-share-template.md           # Post template for Project Share
      project-share-examples.md           # Example Project Share posts
      job-match-workflow.md               # Job Match end-to-end workflow
      job-match-output-format.md          # Match report output format
      linkedin-posting-workflow.md        # LinkedIn browser automation steps
commands/
  linkedin-post.md                        # /linkedin-post slash command
  job-match.md                            # /job-match slash command
```
