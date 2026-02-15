# /job-match

Compare your skills against job listings to see how well you match. Analyzes your git repos and resume against job description requirements.

## Instructions

Use the `linkedin-code-post` skill in **Job Match** mode to complete this task.

1. **Ask the user for:**
   - Path to job description file(s) or directory (required)
   - Path to resume file (optional but recommended)
   - Any additional repos to scan (optional -- defaults to current repo)

2. **Follow the Job Match workflow** defined in the skill:
   - Phase 1: Read and parse job descriptions
   - Phase 2: Extract skills from repos (`references/repo-analysis-guide.md`)
   - Phase 3: Extract skills from resume (if provided)
   - Phase 4: Run match analysis -- categorize each skill as Strong/Partial/Missing
   - Phase 5: Output the match report (`references/job-match-output-format.md`)

3. **No browser automation** -- all output is in the terminal
