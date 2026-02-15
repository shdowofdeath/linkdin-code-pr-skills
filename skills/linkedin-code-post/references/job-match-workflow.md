# Job Match Workflow

End-to-end workflow for Mode 2: comparing your skills against job descriptions.

## Phase 1: Read Job Descriptions

### Input
User provides one of:
- **Single file path**: e.g., `/path/to/job-description.txt`
- **Directory path**: e.g., `/path/to/jobs/` (all supported files inside are treated as separate job listings)

### Supported Formats
- `.txt` -- plain text
- `.md` -- markdown
- `.pdf` -- use `Read` tool (supports PDF)

For `.docx` files: tell the user to export to PDF or paste content into a text file.

### Extraction
From each job description, extract:

| Field | What to Look For |
|-------|-----------------|
| **Job title** | Usually the first line or heading |
| **Required skills** | Sections labeled "Requirements", "Required", "Must have", "Qualifications" |
| **Preferred skills** | Sections labeled "Preferred", "Nice to have", "Bonus", "Desired" |
| **Years of experience** | "X+ years", "X-Y years of experience" |
| **Education** | "Bachelor's", "Master's", "CS degree", "equivalent experience" |
| **Domain knowledge** | Industry-specific terms: "fintech", "healthcare", "e-commerce", etc. |

If a job description is ambiguous about required vs. preferred, treat all skills as required.

## Phase 2: Skills Extraction from Repos

Follow `references/repo-analysis-guide.md` to analyze the current repository (or multiple repos if the user specifies additional paths).

Map the analysis output to skill categories:

| Category | Source |
|----------|--------|
| **Languages** | File extensions, manifest files |
| **Frameworks** | Dependencies in manifest files |
| **Databases** | Database drivers/ORMs in dependencies, config files |
| **Cloud / Infrastructure** | Dockerfile, Terraform, K8s manifests, cloud SDK dependencies |
| **Testing** | Test frameworks in dependencies, test file presence |
| **CI/CD** | `.github/workflows/`, `Jenkinsfile`, `.gitlab-ci.yml` |
| **Architecture** | Code structure (monorepo, microservices indicators), messaging deps (kafka, rabbitmq, etc.) |
| **Tools** | Linters, formatters, build tools from devDependencies or config files |

### Proficiency Estimation

For each detected skill, estimate proficiency:

| Signal | Proficiency |
|--------|------------|
| Primary language with high file count + recent commits | **Expert** |
| In dependencies + actively used in recent commits | **Proficient** |
| In dependencies but few recent commits touching it | **Familiar** |
| Only in devDependencies or config | **Basic** |

## Phase 3: Skills Extraction from Resume

If the user provides a resume file:

1. Read the file using `Read` tool
2. Extract:
   - **Listed skills/technologies** -- from skills sections, bullet points
   - **Work experience** -- role titles, durations, company descriptions, technologies mentioned in each role
   - **Education** -- degrees, institutions, relevant coursework
   - **Certifications** -- AWS, Azure, GCP certs, etc.
   - **Projects** -- personal or professional projects mentioned

3. Cross-reference with repo-derived skills:
   - Skills found in BOTH resume and repos get tagged as **Verified** (strongest signal)
   - Skills only on resume get tagged as **Claimed** (stated but no code evidence in current repos)
   - Skills only in repos get tagged as **Demonstrated** (evidence exists but not on resume)

## Phase 4: Match Analysis

For each job description, compare the unified skill profile against requirements.

### Categorization

For each **required skill** in the job:

| Category | Criteria | Weight |
|----------|----------|--------|
| **Strong Match** | Found in both resume and repo code, OR found in repo code with high proficiency | 1.0 |
| **Partial Match** | Found in resume only, OR found in repo but low proficiency, OR a closely related skill exists (e.g., job asks React, user has Vue) | 0.5 |
| **Missing** | Not found in either source, no closely related skill | 0.0 |

### Related Skills Map

Use these common equivalences for partial matching:

| Job Asks | Accept As Partial |
|----------|------------------|
| React | Vue, Svelte, Angular (frontend framework experience) |
| PostgreSQL | MySQL, SQLite (relational DB experience) |
| AWS | GCP, Azure (cloud provider experience) |
| Docker | Podman, containerd (container experience) |
| Kubernetes | Docker Swarm, ECS, Nomad (orchestration experience) |
| GraphQL | REST API experience (API design) |
| Redis | Memcached (caching experience) |
| Kafka | RabbitMQ, SQS, NATS (messaging experience) |
| Terraform | Pulumi, CloudFormation (IaC experience) |
| Jenkins | GitHub Actions, GitLab CI, CircleCI (CI/CD experience) |

### Scoring

```
match_score = (strong_matches * 1.0 + partial_matches * 0.5) / total_required_skills * 100
```

Preferred skills are scored separately as a bonus percentage.

### Verdict

| Score | Verdict |
|-------|---------|
| 80-100% | **Strong fit** -- you meet most/all requirements |
| 60-79% | **Good fit with gaps** -- solid foundation, some skills to develop |
| 40-59% | **Stretch role** -- significant upskilling needed but possible |
| 0-39% | **Significant gap** -- major skill development required |

## Phase 5: Generate Recommendations

For each **missing** skill:
- Suggest what related experience to highlight in an application
- Suggest a concrete learning path or resource
- Estimate ramp-up difficulty (quick learn vs. deep skill)

For each **partial** match:
- Suggest how to frame existing experience to show relevance
- Highlight transferable aspects

Overall:
- Which 2-3 skills would have the highest ROI to learn
- Which strengths to emphasize in an application
- Whether the role is worth pursuing given the current profile
