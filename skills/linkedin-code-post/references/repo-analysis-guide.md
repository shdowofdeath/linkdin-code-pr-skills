# Repo Analysis Guide

Shared logic for scanning a git repository to extract project metadata and tech stack. Used by both Project Share (Mode 1) and Job Match (Mode 2).

## What to Analyze

### 1. Project Identity

Read these files (if they exist) to understand the project:

```
Read README.md (first 100 lines)
```

Extract:
- Project name (from manifest file or directory name)
- One-line description (from README first paragraph or manifest `description` field)

### 2. Manifest File Detection

Check for these files to determine project type and dependencies:

| File | Project Type | Extract |
|------|-------------|---------|
| `package.json` | Node.js / JavaScript / TypeScript | `dependencies`, `devDependencies`, `scripts`, `name`, `description` |
| `requirements.txt` | Python | Package names and versions |
| `pyproject.toml` | Python | `[project.dependencies]`, `[tool.*]` sections |
| `setup.py` / `setup.cfg` | Python | `install_requires` |
| `Pipfile` | Python | `[packages]`, `[dev-packages]` |
| `Cargo.toml` | Rust | `[dependencies]`, `[package]` |
| `go.mod` | Go | Module path, `require` block |
| `pom.xml` | Java / Kotlin | `<dependencies>` |
| `build.gradle` / `build.gradle.kts` | Java / Kotlin | `dependencies` block |
| `Gemfile` | Ruby | Gem names |
| `composer.json` | PHP | `require`, `require-dev` |
| `*.csproj` / `*.sln` | .NET | `<PackageReference>` elements |
| `mix.exs` | Elixir | `deps` function |
| `Package.swift` | Swift | `dependencies` |

Use `Glob` to check which manifest files exist:
```
Glob("package.json")
Glob("requirements.txt")
Glob("pyproject.toml")
Glob("Cargo.toml")
Glob("go.mod")
...
```

### 3. Language Distribution

Use `Glob` to count files by extension:

```
Glob("**/*.ts")
Glob("**/*.tsx")
Glob("**/*.js")
Glob("**/*.jsx")
Glob("**/*.py")
Glob("**/*.rs")
Glob("**/*.go")
Glob("**/*.java")
Glob("**/*.rb")
Glob("**/*.swift")
```

Report the top 2-3 languages by file count.

### 4. Framework and Library Detection

Map dependency names to well-known frameworks:

| Dependency Pattern | Framework/Tool |
|-------------------|----------------|
| `react`, `react-dom` | React |
| `next` | Next.js |
| `vue` | Vue.js |
| `nuxt` | Nuxt |
| `svelte` | Svelte |
| `angular` | Angular |
| `express` | Express.js |
| `fastify` | Fastify |
| `nestjs`, `@nestjs/*` | NestJS |
| `django` | Django |
| `flask` | Flask |
| `fastapi` | FastAPI |
| `spring-boot*` | Spring Boot |
| `rails`, `activerecord` | Ruby on Rails |
| `actix-web`, `axum`, `rocket` | Rust web frameworks |
| `gin`, `echo`, `fiber` | Go web frameworks |
| `tailwindcss` | Tailwind CSS |
| `prisma`, `@prisma/client` | Prisma ORM |
| `sqlalchemy` | SQLAlchemy |
| `typeorm` | TypeORM |
| `drizzle-orm` | Drizzle ORM |
| `mongoose` | Mongoose (MongoDB) |

### 5. Database Detection

Look for database-related dependencies and config files:

| Signal | Database |
|--------|----------|
| `pg`, `postgres`, `psycopg2` | PostgreSQL |
| `mysql2`, `mysqlclient` | MySQL |
| `mongodb`, `mongoose`, `pymongo` | MongoDB |
| `redis`, `ioredis` | Redis |
| `sqlite3`, `better-sqlite3` | SQLite |
| `@elastic/elasticsearch` | Elasticsearch |
| `prisma/schema.prisma` containing `provider` | Whatever the provider specifies |

### 6. Infrastructure Detection

Check for these files/directories:

| File/Pattern | Infrastructure |
|-------------|---------------|
| `Dockerfile`, `docker-compose.yml` | Docker |
| `k8s/`, `kubernetes/`, `*.yaml` with `kind: Deployment` | Kubernetes |
| `*.tf`, `terraform/` | Terraform |
| `.github/workflows/` | GitHub Actions CI/CD |
| `Jenkinsfile` | Jenkins |
| `.gitlab-ci.yml` | GitLab CI |
| `.circleci/` | CircleCI |
| `serverless.yml` | Serverless Framework |
| `vercel.json` | Vercel |
| `netlify.toml` | Netlify |
| `fly.toml` | Fly.io |

### 7. Testing Detection

| Signal | Testing Tool |
|--------|-------------|
| `jest`, `@jest/core` in deps | Jest |
| `pytest` in deps, `conftest.py` exists | pytest |
| `mocha`, `chai` in deps | Mocha |
| `vitest` in deps | Vitest |
| `cypress` in deps | Cypress |
| `playwright`, `@playwright/test` | Playwright |
| `test/`, `tests/`, `__tests__/`, `*_test.go` | Test directories/files |

### 8. Git Activity

```bash
git log --oneline -20
```

Summarize:
- What areas of the codebase are being actively worked on
- General themes of recent commits (feature work, bug fixes, refactoring, etc.)

```bash
git log --format='%H' --since='30 days ago' | wc -l
```

Report commit frequency (e.g., "15 commits in the last 30 days").

### 9. Project Size

```bash
find . -type f -not -path './.git/*' -not -path './node_modules/*' -not -path './.venv/*' -not -path './target/*' -not -path './dist/*' -not -path './build/*' | wc -l
```

Report approximate file count (excluding common build/dependency directories).

## Output Format

Produce a structured summary:

```
Project Name: <name>
Description: <one-line description>
Primary Language(s): <e.g., TypeScript, Python>
Frameworks: <e.g., React, FastAPI>
Key Dependencies: <top 10 most significant>
Databases: <if any>
Infrastructure: <Docker, CI/CD, etc.>
Testing: <test frameworks found>
Recent Focus: <summary from git log>
Project Size: ~<N> files
Commit Activity: <N> commits in last 30 days
```

This summary is consumed by:
- **Project Share (Mode 1):** to generate the LinkedIn post content
- **Job Match (Mode 2):** to extract skills and technologies from the user's codebase
