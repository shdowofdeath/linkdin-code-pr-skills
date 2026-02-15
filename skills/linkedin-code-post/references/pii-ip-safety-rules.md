# PII & IP Safety Rules

Ensure no proprietary information, personal data, or secrets leak into a public LinkedIn post. These rules apply to **Project Share (Mode 1)** when generating post content from repo analysis.

## Layer 1: Never Read

Do NOT open or read these files at all. Skip them entirely during repo analysis.

| Pattern | Reason |
|---------|--------|
| `.env`, `.env.*`, `.envrc` | Environment variables with secrets |
| `credentials.json`, `service-account.json` | Cloud credentials |
| `*.pem`, `*.key`, `*.cert`, `*.p12`, `*.pfx` | Certificates and private keys |
| `secrets/`, `credentials/`, `private/` directories | Secret storage directories |
| `*.secret`, `*.secrets` | Secret files |
| `.npmrc`, `.pypirc` (may contain tokens) | Package registry auth |
| `*.sqlite`, `*.db` | Local database files |

When scanning the repo, use manifest files (package.json, requirements.txt, etc.) and README for context. Do NOT read application source code for post content -- the post should be about the project at a high level, not about specific implementation details.

## Layer 2: Always Strip (No Override)

These items are NEVER included in a post, even if the user requests it.

- **API keys and tokens**: Any string matching patterns like `sk-`, `ghp_`, `gho_`, `AKIA`, `xox[bpras]-`, `Bearer`, `token=`, `api_key=`, long hex/base64 strings
- **Database connection strings**: `postgresql://`, `mongodb://`, `mysql://`, `redis://`, any URI with credentials
- **IP addresses**: `10.x.x.x`, `172.16-31.x.x`, `192.168.x.x`, any specific IP
- **Internal hostnames**: `*.internal.*`, `*.local`, `*.corp.*`, `*.private.*`
- **Passwords and secrets**: Anything that looks like a password, secret, or credential value
- **Email addresses found in code/config** (not the user's own professional email)
- **Customer/user data**: Real names, usernames, or personal data found in fixtures, seed data, or logs

## Layer 3: Default Strip (User Can Override)

These items are stripped by default but the user can explicitly approve them.

| Item | Default Replacement | Why |
|------|-------------------|-----|
| Company name | "my team" / "our project" | May reveal employer |
| Internal project codenames | Generic description ("a payments service") | May be confidential |
| Specific business metrics | Remove or generalize ("significant scale") | May be confidential |
| Internal ticket references | Remove (JIRA-1234, LINEAR-567, etc.) | Reveals internal tooling |
| Git remote URLs | Remove | May reveal private org/repo names |
| Specific user counts or revenue | Remove | May be under NDA |
| Internal tool names | Generic description ("our deployment tool") | May be proprietary |
| Specific dates/timelines | Generalize ("recently", "last month") | May reveal release schedules |

## Layer 4: Verification

Before presenting the post draft to the user:

1. **List what was generalized** from Layer 3:
   > "I've generalized the following for privacy:
   > - Company name -> 'my team'
   > - Project codename 'ProjectX' -> 'a real-time analytics dashboard'
   > - Metric '500k daily users' -> removed
   >
   > Would you like to restore any of these? (Company name, project name, metrics?)"

2. **Run the checklist**:
   - [ ] No API keys, tokens, or secrets anywhere in the post
   - [ ] No internal hostnames or IP addresses
   - [ ] No email addresses (unless user's own, approved)
   - [ ] No company name (unless user approved)
   - [ ] No customer or user data
   - [ ] No proprietary algorithms or business logic described in detail
   - [ ] No file paths that reveal organizational structure
   - [ ] No internal ticket or issue references
   - [ ] No specific business metrics (unless user approved)
   - [ ] Post describes the "what" and "why" at a high level, not implementation specifics

## What IS Safe to Include

- **Programming languages and versions** (TypeScript, Python 3.12, Rust)
- **Open source frameworks and libraries** (React, FastAPI, Prisma)
- **General architecture patterns** (microservices, event-driven, serverless)
- **General problem descriptions** ("building a tool to speed up log analysis")
- **Tech stack choices and rationale** ("chose Rust for the hot path because of memory safety")
- **Development stage** ("early prototype", "just hit v1")
- **General learnings** ("learned that X pattern works better than Y for this use case")
- **Open source project names** (if the repo is public)
- **The user's own name and professional title** (if they want to include it)
