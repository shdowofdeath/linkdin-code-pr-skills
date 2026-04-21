# Supply Chain Attack: The litellm Incident — What Your Team Needs to Know

**Date:** March 24–25, 2026
**Severity:** Critical
**Affected:** Any team that runs `pip install litellm` or auto-updates Python dependencies

---

## What Happened

On March 24, 2026, attackers published two malicious versions of **litellm** — a popular Python library used by AI/LLM applications — directly to PyPI (the official Python package registry):

- `litellm==1.82.7`
- `litellm==1.82.8`

These packages were live for approximately **5.5 hours** (10:39–16:00 UTC).

The attack was not a hack of litellm's GitHub repository. The attacker obtained a **PyPI publish token** — most likely stolen from Aqua Security's CI/CD infrastructure (linked to the Trivy scanner compromise) — and used it to upload malicious packages directly, bypassing litellm's official release process entirely.

**The litellm GitHub repo was not compromised. Only the PyPI packages were.**

---

## Who Is at Risk

You are potentially affected if **any** of the following is true:

| Scenario | At Risk? |
|---|---|
| Ran `pip install litellm --upgrade` between 10:39–16:00 UTC on March 24 | **Yes — rotate all secrets immediately** |
| CI/CD pipeline auto-updates Python dependencies daily | **Yes — check your install logs** |
| Used `pip install litellm` without a pinned version during the window | **Yes** |
| Installed from the official `ghcr.io/berriai/litellm` Docker image | No — image pins deps via `requirements.txt` |
| Used LiteLLM Cloud (hosted service) | No |
| Pinned to `litellm==1.82.6` or earlier | No |
| Installed directly from GitHub source | No |

---

## What the Malicious Code Did

### Version 1.82.7 — Payload in `proxy_server.py`

The attacker embedded credential-harvesting code inside a core library file. Any process that imported litellm would trigger it.

### Version 1.82.8 — The `.pth` Escalation

This version added a file called `litellm_init.pth` to Python's `site-packages` directory.

```
# litellm_init.pth
import os; exec(open('/path/to/payload.py').read())
```

**Why this is especially dangerous:** Python automatically executes `.pth` files at interpreter startup — no `import` statement needed. Every Python process in the environment would run the payload, including:

- CI/CD jobs
- Cron jobs
- Background workers
- Django/FastAPI servers
- Developer scripts

### What It Harvested

The payload systematically collected:

```
Environment variables      → AWS_SECRET_ACCESS_KEY, OPENAI_API_KEY, DATABASE_URL, etc.
.env files                 → ~/.env, ./.env, project-level env files
SSH private keys           → ~/.ssh/id_rsa, ~/.ssh/id_ed25519
AWS credentials            → ~/.aws/credentials, ~/.aws/config
GCP credentials            → ~/.config/gcloud/application_default_credentials.json
Kubernetes configs         → ~/.kube/config (cluster tokens, certs)
CI/CD environment secrets  → All env vars injected by GitHub Actions, GitLab CI, etc.
SSL/TLS private keys       → Common key file paths
Database passwords         → From env vars and config files
```

### How It Got Out

Collected data was:
1. Encrypted with AES-256-CBC + RSA-4096
2. Sent via HTTP POST to `models.litellm.cloud` — an **attacker-controlled domain**, not affiliated with BerriAI

---

## How the Attacker Got the PyPI Token

Understanding this matters because it reveals the real attack surface.

### Step 1 — Compromise the CI/CD pipeline that *runs* litellm's workflows

litellm's CI used **Trivy** (Aqua Security's open-source vulnerability scanner) to scan for vulnerabilities. The suspected path:

```
Aqua Security infrastructure compromised
        ↓
Attacker gains access to Aqua's GitHub org or build runners
        ↓
Harvests PyPI tokens stored as CI/CD secrets
        ↓
Uses token to publish directly to PyPI as a "trusted" maintainer
```

### Step 2 — Why CI/CD Secrets Are the Crown Jewels

GitHub Actions secrets, GitLab CI variables, and similar systems inject sensitive tokens as environment variables at runtime. Any code that runs in that environment — including third-party tools, test runners, or scanners — can read `os.environ` and find:

```
PYPI_TOKEN=pypi-...
AWS_ACCESS_KEY_ID=AKIA...
NPM_TOKEN=npm_...
DOCKER_PASSWORD=...
```

### The "Poisoned PR" Variant

A related vector (and one teams often overlook):

1. Attacker opens a PR to an open-source repo with an innocent-looking change
2. CI runs tests on the PR branch — with secrets available in the environment
3. A malicious file in the PR (test file, `conftest.py`, `setup.py`) reads and exfiltrates the secrets
4. **The PR never needs to be merged**

This is why `pull_request_target` in GitHub Actions is particularly dangerous — it runs with full repo permissions even for fork PRs.

---

## Immediate Actions (If You Were in the Window)

### 1. Check Your Exposure

```bash
# Check if the malicious version was installed
pip show litellm | grep Version

# Check if the .pth file was planted
find $(python -c "import site; print(site.getsitepackages()[0])") -name "litellm_init.pth"

# Check pip install logs (if available)
pip install litellm==1.82.7  # will fail — already quarantined, confirms you're checking right version
```

### 2. Rotate All Secrets — Assume Full Compromise

If you installed during the window, treat every secret in that environment as exposed:

- [ ] Rotate all API keys (OpenAI, Anthropic, AWS, GCP, Azure, etc.)
- [ ] Rotate SSH keys used on affected machines
- [ ] Rotate database passwords
- [ ] Revoke and reissue Kubernetes service account tokens
- [ ] Rotate CI/CD secrets (GitHub Actions, GitLab CI, etc.)
- [ ] Invalidate any JWT signing keys

### 3. Audit Cloud Access Logs

```bash
# AWS — look for unusual API calls in the window 10:39–16:00 UTC March 24
aws cloudtrail lookup-events \
  --start-time 2026-03-24T10:30:00Z \
  --end-time 2026-03-24T17:00:00Z \
  --query 'Events[?Username!=`your-expected-user`]'

# GCP — check audit logs in Cloud Console
# Look for: unusual service account usage, new IAM bindings, data exports
```

### 4. Clean the Environment

```bash
# Upgrade to a clean version
pip install litellm==1.82.9  # or latest confirmed clean version

# Remove the .pth file if present
find $(python -c "import site; print(site.getsitepackages()[0])") \
  -name "litellm_init.pth" -delete

# In Docker — rebuild from scratch, do not patch in place
docker build --no-cache .
```

---

## Long-Term Hardening — What Your Team Should Do

### 1. Pin All Dependencies

```txt
# requirements.txt — always pin exact versions
litellm==1.82.6          # known-good, explicit
openai==1.12.0
anthropic==0.18.1
```

Never use `litellm>=1.0` in production. Unpinned dependencies are a standing invitation for this attack.

### 2. Use a Lock File and Verify Hashes

```bash
# Generate lock file with hashes
pip-compile --generate-hashes requirements.in

# requirements.txt will contain:
# litellm==1.82.6 \
#     --hash=sha256:abc123... \
#     --hash=sha256:def456...

# Install with hash verification
pip install --require-hashes -r requirements.txt
```

If an attacker publishes a new version, the hash won't match and the install fails.

### 3. Fix GitHub Actions — Least Privilege

```yaml
# Every workflow should declare minimal permissions
permissions:
  contents: read        # read-only by default

jobs:
  test:
    runs-on: ubuntu-latest
    # NO secrets here — tests don't need PYPI_TOKEN
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683  # pinned SHA
      - run: pip install -r requirements.txt
      - run: pytest

  publish:
    needs: test
    if: startsWith(github.ref, 'refs/tags/')  # only on release tags
    permissions:
      id-token: write   # for OIDC — better than storing tokens
    steps:
      - uses: pypa/gh-action-pypi-publish@release/v1
```

### 4. Never Use `pull_request_target` with Untrusted Code

```yaml
# DANGEROUS — do not do this
on:
  pull_request_target:
jobs:
  test:
    steps:
      - uses: actions/checkout@v4
        with:
          ref: ${{ github.event.pull_request.head.sha }}  # ← attacker's code

# SAFE — use pull_request (no secrets for fork PRs)
on:
  pull_request:
```

### 5. Use OIDC Instead of Long-Lived Tokens

```yaml
# Instead of storing PYPI_TOKEN as a secret:
- uses: pypa/gh-action-pypi-publish@release/v1
  with:
    # Uses short-lived OIDC token — no stored secret to steal
```

OIDC tokens expire in minutes. A stolen OIDC token is useless by the time an attacker tries to use it.

### 6. Pin Third-Party GitHub Actions to SHA

```yaml
# Bad — tag can be moved to point to malicious code
- uses: actions/checkout@v4

# Good — SHA is immutable
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683
```

### 7. Monitor PyPI Publishes

If you maintain packages on PyPI:
- Enable PyPI two-factor authentication
- Use per-project API tokens (not account-wide tokens)
- Set up alerts for unexpected publishes
- Use Trusted Publishers (OIDC) — eliminates the need for stored tokens entirely

### 8. Network Egress Controls in CI

```yaml
# Use a network policy or proxy that blocks unexpected outbound connections
# Any CI job trying to POST to an unknown domain should alert
```

If the malicious payload had tried to exfiltrate to `models.litellm.cloud` in an environment with egress controls, it would have been blocked.

---

## Summary — The Key Lessons

| Lesson | Action |
|---|---|
| Supply chain attacks bypass code review entirely | Pin dependencies + verify hashes |
| CI/CD secrets are the most valuable target | Least-privilege permissions + OIDC |
| `.pth` files execute without any import | Audit `site-packages` after installs |
| Auto-updates in production are dangerous | Use lock files, update deliberately |
| Malicious PRs don't need to be merged | Never use `pull_request_target` with untrusted checkout |
| Third-party actions are attack surface | Pin to SHA, not tags |

---

## References

- [litellm official security blog post](https://github.com/BerriAI/litellm/blob/main/docs/my-website/blog/security_update_march_2026/index.md)
- [litellm incident response commits](https://github.com/BerriAI/litellm/commits/main/)
- [PyPI — Understanding `.pth` files](https://docs.python.org/3/library/site.html)
- [GitHub Actions — Security hardening](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [PyPI Trusted Publishers (OIDC)](https://docs.pypi.org/trusted-publishers/)
- [Codecov supply chain incident (2021)](https://about.codecov.io/security-update/) — earlier example of the same attack pattern
