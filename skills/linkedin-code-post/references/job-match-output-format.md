# Job Match Output Format

The match report is displayed directly in the terminal. No browser automation is needed for this mode.

## Single Job Report

```
================================================================
JOB MATCH REPORT
================================================================

Job: {job_title}
Source: {file_name}

OVERALL MATCH: {percentage}% -- {verdict}

----------------------------------------------------------------
MATCHED SKILLS ({count})
----------------------------------------------------------------
  [STRONG]  TypeScript       -- 3 repos, 2 years on resume
  [STRONG]  React            -- active in current repo, listed on resume
  [STRONG]  PostgreSQL       -- found in 2 repo dependencies, resume lists SQL databases
  [PARTIAL] GraphQL          -- resume mentions it, no repo evidence
  [PARTIAL] AWS              -- Dockerfile found, no direct AWS config; resume lists AWS

----------------------------------------------------------------
MISSING SKILLS ({count})
----------------------------------------------------------------
  [MISSING] Kubernetes
            Your experience: Docker usage found in 2 repos
            Recommendation: Your container experience is a strong foundation.
            K8s basics are learnable in 2-4 weeks with hands-on practice.

  [MISSING] Kafka
            Your experience: Event-driven patterns found in repo (WebSocket, pub/sub)
            Recommendation: Your messaging architecture knowledge transfers well.
            Consider building a small Kafka consumer/producer project.

----------------------------------------------------------------
PREFERRED SKILLS (BONUS)
----------------------------------------------------------------
  [HAVE]    Terraform        -- found in repo infra/ directory
  [HAVE]    CI/CD            -- GitHub Actions workflows found
  [MISSING] Datadog          -- not found

----------------------------------------------------------------
EXPERIENCE ALIGNMENT
----------------------------------------------------------------
  Required: 5+ years backend development
  Your profile: 4 years on resume, 6 repos with backend code spanning 3 years of git history
  Assessment: Likely meets requirement based on combined evidence

  Required: Bachelor's in CS or equivalent
  Your profile: Listed on resume
  Assessment: Meets requirement

----------------------------------------------------------------
RECOMMENDATIONS
----------------------------------------------------------------
  1. Highlight your TypeScript + React full-stack experience prominently
  2. Frame your Docker knowledge as container orchestration readiness
  3. Your event-driven architecture experience maps to their Kafka requirement
  4. Top skill to learn: Kubernetes (highest ROI given your Docker foundation)
================================================================
```

## Field Descriptions

| Field | Content |
|-------|---------|
| **Job title** | Extracted from the job description |
| **Source** | Filename of the job description |
| **Overall match** | Percentage score + verdict (Strong fit / Good fit with gaps / Stretch role / Significant gap) |
| **Matched skills** | Each skill tagged as [STRONG] or [PARTIAL] with evidence source |
| **Missing skills** | Each skill tagged as [MISSING] with related experience (if any) and a concrete recommendation |
| **Preferred skills** | Bonus skills tagged as [HAVE] or [MISSING] |
| **Experience alignment** | Comparison of years/education requirements vs. user's profile |
| **Recommendations** | 3-5 actionable items: what to highlight, what to learn, how to frame experience |

## Multiple Jobs: Comparison Summary

When the user provides multiple job descriptions, output one full report per job, then append a summary comparison table:

```
================================================================
COMPARISON SUMMARY
================================================================

| #  | Job Title                | Match % | Verdict            | Top Gap         |
|----|--------------------------|---------|--------------------|-----------------|
| 1  | Senior Backend Engineer  | 78%     | Good fit w/ gaps   | Kubernetes      |
| 2  | Full Stack Developer     | 92%     | Strong fit         | GraphQL         |
| 3  | Platform Engineer        | 45%     | Stretch role       | Terraform, K8s  |

BEST FIT: #2 Full Stack Developer (92%)

TOP SKILLS TO DEVELOP (across all jobs):
  1. Kubernetes -- required by 2 of 3 jobs
  2. GraphQL -- required by 2 of 3 jobs
  3. Kafka -- required by 1 of 3 jobs

================================================================
```

## Formatting Rules

- Use `=` lines for major section dividers
- Use `-` lines for subsection dividers
- Indent skill entries with 2 spaces
- Tags are always uppercase in brackets: `[STRONG]`, `[PARTIAL]`, `[MISSING]`, `[HAVE]`
- Keep recommendations concise (1-2 lines each)
- Always end with actionable next steps
- When multiple jobs share missing skills, call that out in the comparison summary
