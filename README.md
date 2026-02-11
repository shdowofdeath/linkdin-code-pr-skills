# LinkedIn Code Post Skills

Claude Code skills for creating LinkedIn posts that showcase your code and PRs with styled code screenshots.

## What It Does

Takes code you wrote (or a PR you worked on) and creates a LinkedIn post with:
- A polished post draft tailored for LinkedIn engagement
- A styled code screenshot from [Carbon.now.sh](https://carbon.now.sh)
- Browser-automated posting via Claude in Chrome (you click Publish)

## Supported Formats

| Format | Use When |
|--------|----------|
| **Code Showcase** | You want to share a specific code snippet, function, or pattern |
| **PR Review** | You want to share a PR you opened, reviewed, or merged |

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- [Claude in Chrome](https://chromewebstore.google.com/detail/claude-in-chrome/) extension installed and connected
- Logged into LinkedIn in Chrome
- For PR Review format: `gh` CLI authenticated with GitHub

## Installation

Copy the skills and commands to your Claude Code config directory:

```bash
cp -R skills/* ~/.claude/skills/
cp -R commands/* ~/.claude/commands/
```

## Usage

In any Claude Code session:

```
/linkedin-post
```

Claude will:
1. Ask whether you want a Code Showcase or PR Review post
2. Gather context from your code or PR
3. Draft a post and show it to you for approval
4. Create a styled code screenshot on Carbon.now.sh
5. Open LinkedIn and compose the post with the screenshot attached
6. **Stop and let you review before publishing**

## Safety

The skill will **never** click Post or Publish on LinkedIn. You always have the final review and publish step.

## Project Structure

```
skills/
  linkedin-code-post/
    SKILL.md                         # Core skill definition
    references/
      post-templates.md              # Post templates for both formats
      carbon-screenshot-workflow.md   # Carbon browser automation steps
      linkedin-posting-workflow.md    # LinkedIn browser automation steps
      code-showcase-examples.md      # Example code showcase posts
      pr-review-examples.md          # Example PR review posts
commands/
  linkedin-post.md                   # /linkedin-post slash command
```
