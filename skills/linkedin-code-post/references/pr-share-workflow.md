# PR Share Workflow

How to analyze a PR and generate an engaging LinkedIn post about the technical change.

## Phase 1: Get PR Information

### From a GitHub PR URL

Use the `gh` CLI to extract PR details:

```bash
gh pr view <PR_URL> --json title,body,files,additions,deletions,labels,mergedAt,headRefName
```

Extract:
- **Title** -- what the PR claims to do
- **Description** -- the author's explanation of the change
- **Files changed** -- which parts of the codebase were touched
- **Diff stats** -- additions/deletions to gauge scope
- **Labels** -- bug, feature, refactor, etc.

### From a Local Branch

If the user provides a branch name instead of a URL:

```bash
git log main..<branch> --oneline --stat
git diff main..<branch> --stat
git diff main..<branch>
```

Extract the same information from the commit messages and diff.

### From the Current Branch

If the user just says "share my latest PR" without specifying:

```bash
git log --oneline -10
git diff HEAD~1 --stat
```

Ask the user to confirm which commits/changes to share.

## Phase 2: Analyze the Change

Categorize the PR into one or more of:
- **Bug fix** -- what was broken, why, and how it was fixed
- **New feature** -- what capability was added and why
- **Refactor** -- what was improved structurally and why
- **Performance** -- what was slow, why, and what improved
- **Infrastructure** -- deployment, CI/CD, tooling changes
- **Migration** -- moving from one technology/approach to another

Identify the **interesting technical decisions**:
- Why was this approach chosen over alternatives?
- What was surprising or non-obvious about the problem?
- What tradeoffs were made?
- What was the hardest part?

## Phase 3: PII/IP Stripping

Follow `pii-ip-safety-rules.md`. Additionally for PR Share:
- Strip PR numbers and ticket references (e.g., "JIRA-1234", "#456")
- Strip specific file paths that reveal internal architecture
- Generalize business logic descriptions ("fixed billing calculation" -> "fixed a critical calculation bug")
- Remove reviewer names and team references
- Never include code diffs or snippets in the post

## Phase 4: Post Generation

Use `pr-share-template.md` to generate the post.

The post should tell a story:
1. **The problem** -- what situation prompted this change?
2. **The approach** -- what was the key insight or decision?
3. **The lesson** -- what's the takeaway for other engineers?

**Critical rule:** The PR details are context for YOU to understand the change. The post should be about the **engineering lesson or approach**, not a description of the specific PR.

## Phase 5: User Approval

Present the draft:
- Show the full post text
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until approved

**Do NOT proceed to posting until the user explicitly approves.**

## Phase 6: LinkedIn Posting

Follow `linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).
