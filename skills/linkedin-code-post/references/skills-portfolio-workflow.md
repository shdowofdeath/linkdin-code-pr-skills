# Skills Portfolio Workflow

How to scan multiple repos, aggregate technical skills, and generate a comprehensive LinkedIn skills summary post.

## Phase 1: Gather Repos

Ask the user for:
1. **Repo paths** -- one or more local repo paths to analyze (defaults to current repo)
2. **Resume file** -- optional path to a resume/CV file for additional context

**Requirement:** At least one repo must be scanned. Do not generate a skills post from a resume alone -- the post must be grounded in demonstrated work.

## Phase 2: Multi-Repo Analysis

For each repo, follow `repo-analysis-guide.md` to extract:
- Primary languages and line counts
- Frameworks and libraries
- Databases and data stores
- Infrastructure and deployment tools
- Testing frameworks and coverage approach
- CI/CD pipelines
- Architecture patterns (monorepo, microservices, event-driven, etc.)

Track which repo each skill was found in for evidence strength.

## Phase 3: Skills Aggregation

### Deduplication
- Merge identical skills found across repos (e.g., "TypeScript" in 3 repos = one entry)
- Merge related skills (e.g., "Jest" + "Vitest" = "JavaScript testing")

### Ranking
Rank skills by evidence strength:
1. **Strong** -- found in 3+ repos or heavy usage in 2+ repos
2. **Solid** -- found in 2 repos or heavy usage in 1 repo
3. **Emerging** -- found in 1 repo with light usage, or listed on resume but limited repo evidence

### Categories
Group into:
- Languages
- Frontend frameworks
- Backend frameworks
- Databases & data stores
- Cloud & infrastructure
- Testing & quality
- CI/CD & DevOps
- Architecture & patterns

### Resume Cross-Reference (if provided)
- Skills on resume AND in repos -> "Verified"
- Skills on resume but NOT in repos -> "Claimed" (include cautiously)
- Skills in repos but NOT on resume -> "Demonstrated" (highlight these -- they show real work)

## Phase 4: PII/IP Stripping

Follow `pii-ip-safety-rules.md`. Additionally for Skills Portfolio:
- Strip all company names and employer references
- Strip project names and codenames
- Strip internal tool names (replace with generic descriptions)
- Keep technology names (these are public knowledge)
- Generalize project descriptions ("built an internal billing system" -> "built a financial data processing system")

## Phase 5: Post Generation

Use `skills-portfolio-template.md` to generate the post.

Key principles:
- **Show context, not just lists** -- "TypeScript across 5 projects over 3 years" not just "TypeScript"
- **Highlight what's current** -- emphasize recent activity over old repos
- **Include what you're learning** -- shows growth mindset
- **Don't list everything** -- pick the top 8-10 most impressive/relevant skills

## Phase 6: User Approval

Present the draft:
- Show the full post text
- Show the skills evidence summary (which repos contributed which skills)
- Show what was stripped/generalized
- Ask for approval or edits
- Iterate until approved

**Do NOT proceed to posting until the user explicitly approves.**

## Phase 7: LinkedIn Posting

Follow `linkedin-posting-workflow.md` (skip Step 6 -- no image attachment).
