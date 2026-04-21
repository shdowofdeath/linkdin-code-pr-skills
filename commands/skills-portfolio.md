# /skills-portfolio

Analyze your repos and generate a comprehensive skills summary post for LinkedIn -- showcase your technical breadth and depth.

## Instructions

Use the `linkedin-code-post` skill in **Skills Portfolio** mode to complete this task.

1. **Gather repos** from the user:
   - Ask for paths to repos to analyze (defaults to current repo)
   - Optionally accept a resume file path for additional context
   - Must scan at least one repo -- don't generate from resume alone

2. **Analyze repos** following `references/repo-analysis-guide.md`:
   - Scan each repo for languages, frameworks, databases, infra, testing, CI/CD
   - Aggregate and deduplicate skills across repos
   - Rank by evidence strength (multiple repos > single repo, recent > old)

3. **Strip PII/IP** following `references/pii-ip-safety-rules.md`:
   - Strip company names, project names, internal tools
   - Keep only tech stack and general descriptions
   - List what was generalized and ask user if they want to restore anything

4. **Generate a post draft** using `references/skills-portfolio-template.md`:
   - Under 1300 characters, text-only
   - Focus on breadth of skills with context, include what you're learning
   - Show the draft and wait for user approval

5. **Post to LinkedIn** via browser automation:
   - Follow `references/linkedin-posting-workflow.md` (skip image attachment)
   - Always ask before clicking Post
