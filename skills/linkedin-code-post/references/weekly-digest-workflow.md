# Weekly Digest Workflow

How to gather weekly git activity across repos and generate a "what I built this week" LinkedIn post.

## Phase 1: Gather Repos and Date Range

Ask the user for:
1. **Repo paths** -- one or more local repo paths to scan (defaults to current repo)
2. **Date range** -- defaults to last 7 days, user can override (e.g., "last 2 weeks", "since Monday")

## Phase 2: Git Activity Scan

For each repo, run:

```bash
git log --since="7 days ago" --author="$(git config user.name)" --oneline --stat
```

Adjust the `--since` flag based on user-specified date range.

Extract per repo:
- **Commit count** -- how many commits in the period
- **Files changed** -- which files/directories were touched
- **Commit messages** -- themes and descriptions of work
- **Languages touched** -- based on file extensions

**Requirement:** At least 1 commit must exist in the date range. If no commits are found across all repos, inform the user and stop -- don't generate a post about inactivity.

## Phase 3: Activity Aggregation

### Group by Theme
Categorize commits into:
- **Feature work** -- new functionality, user-facing changes
- **Bug fixes** -- defect corrections, error handling
- **Refactoring** -- code improvements, tech debt reduction
- **Testing** -- new tests, test infrastructure
- **Documentation** -- docs, comments, READMEs
- **Infrastructure** -- CI/CD, deployment, tooling
- **Dependencies** -- upgrades, migrations

### Identify Top Accomplishments
Pick the 2-3 most significant/interesting items:
- Prioritize features and bug fixes over dependency bumps
- Look for themes (e.g., "spent the week on performance" is more interesting than a list of 12 small fixes)
- Identify any interesting challenges or learnings from commit messages

### Cross-Repo Summary
If multiple repos, note the breadth: "Worked across 3 projects this week" adds credibility.

## Phase 4: PII/IP Stripping

Follow `pii-ip-safety-rules.md`. Additionally for Weekly Digest:
- Strip ticket numbers from commit messages (e.g., "PROJ-123: Fix login" -> "Fixed authentication bug")
- Strip internal project names and codenames
- Strip specific file paths
- Generalize feature descriptions ("added Stripe webhook handler" -> "added payment integration")
- Remove references to internal teams, standups, sprint numbers

## Phase 5: Post Generation

Use `weekly-digest-template.md` to generate the post.

Key principles:
- **Conversational tone** -- "This week I..." not "Completed the following tasks:"
- **Focus on accomplishments** -- what you built/fixed/improved, not what you worked on
- **Include a learning or challenge** -- makes it relatable
- **Depth over breadth** -- if the week was mostly one big thing, that's fine
- **Look forward** -- mention what's next (creates continuity for followers)

## Phase 6: User Approval

Present the draft:
- Show the full post text
- Show the activity summary (commits, repos, themes)
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until approved

**Do NOT proceed to posting until the user explicitly approves.**

## Phase 7: LinkedIn Posting

Follow `linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).
