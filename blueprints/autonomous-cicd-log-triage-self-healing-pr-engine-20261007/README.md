# Autonomous CI/CD Log Triage & Self-Healing PR Engine — Python & DuckDB

![Autonomous CI/CD Log Triage & Self-Healing PR Engine — Python & DuckDB](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/autonomous-cicd-log-triage-self-healing-pr-engine-20261007.png)

> **Eliminate CI log debugging: Ingest raw build logs into DuckDB, isolate stack traces, and auto-open healing PRs.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/skxhf)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Engineering teams lose 15+ senior engineering hours each week manually downloading multi-gigabyte CI build logs, hunting through cryptic stack traces, and writing trivial regression patches. Enterprise observability vendors charge thousands per month for basic log retention and flake detection, while developers still end up manually fixing routine environment, lockfile, and mock breakages.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Sub-second DuckDB log ingestion and vectorless pattern clustering across gigabyte-scale build runs**
- ✅ **Deterministic root-cause attribution isolating dependency drift, environment anomalies, and code regressions**
- ✅ **Autonomous Git patch generation that opens self-healing Pull Requests directly via GitHub REST API**
- ✅ **Zero-infrastructure runner design executing entirely inside your existing CI workflow with zero external SaaS lock-in**


---

## 📖 Technical Documentation & Setup Guide

# Autonomous CI/CD Log Triage & Self-Healing PR Engine

A zero-infrastructure, self-contained Python CLI tool (`main.py`) powered by DuckDB for sub-second CI/CD log analysis, deterministic root-cause attribution, and automated pull request generation for routine build failures.

---

## 1. Overview & Architecture

Modern engineering organizations lose dozens of senior engineering hours weekly parsing multi-gigabyte CI/CD log dumps to diagnose recurring, deterministic build breaks—such as lockfile mismatches, expired test certificates, missing mock fixtures, and stale container base layers. Centralized observability vendors charge exorbitant enterprise rates for simple log ingestion, yet leave the remediation burden on developers.

This engine bypasses external SaaS dependencies by running directly inside your CI workflow runner. It ingests massive raw build logs into an embedded DuckDB instance, executes vectorless n-gram frequency and pattern clustering to isolate the primary failure signature within milliseconds, determines the precise failure class, and uses targeted remediation strategies to generate a scoped Git patch and open a self-healing Pull Request via the GitHub REST API.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CI/CD Execution Runner                          │
└────────────────────────────────────┬───────────────────────────────────┘
                                     │ (Job Failure Triggered)
                                     ▼
┌────────────────────────────────────────────────────────────────────────┐
│                         Engine Entry: main.py                          │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Log Ingestion & Query Engine (DuckDB)                               │
│    • Streams multi-GB logs directly from disk or runner stdout         │
│    • Tokenizes error frames into columnar relational structures        │
│    • Vectorless clustering isolates anomalous error cascades (<1s)     │
├────────────────────────────────────────────────────────────────────────┤
│ 2. Deterministic Root-Cause Attribution                                │
│    • Pattern signature matcher isolates target domain:                 │
│      [Dependency Drift | Lockfile Collision | Test Flake | Env Drift]  │
│    • Validates error context against local Git workspace state         │
├────────────────────────────────────────────────────────────────────────┤
│ 3. Autonomous Remediation Synthesizer                                  │
│    • Executes deterministic self-healing handlers (e.g., poetry update,│
│      npm dedupe, fixture regeneration, environment variable alignment) │
│    • Validates generated patch boundaries against strict safety rules  │
├────────────────────────────────────────────────────────────────────────┤
│ 4. Git & PR Dispatcher (GitHub REST API)                               │
│    • Isolates changes in dedicated branch: fix/ci-auto-heal-<run_id>   │
│    • Pushes commits with detailed root-cause triage summary            │
│    • Dispatches Pull Request via GitHub REST API                       │
└────────────────────────────────────────────────────────────────────────┘
```

### Key Capabilities

* **Sub-Second DuckDB Log Ingestion & Vectorless Clustering:** Streams raw, multi-gigabyte log files into DuckDB without external vector databases or heavyweight LLM calls. Uses optimized SQL window functions, regex tokenization, and Levenshtein/n-gram clustering over columnar chunks to detect true failure needles in log haystacks.
* **Deterministic Root-Cause Attribution:** Evaluates extracted traces against rule-driven heuristics and workspace state to categorize failures into dependency drift, lockfile corruption, network/mirror timeouts, or environment mismatches.
* **Autonomous Git Patch Generation:** Contains focused remediators for common failures (regenerating lockfiles, updating strict pinned hashes, refreshing test snapshot baselines) and submits the fix as an isolated Pull Request via GitHub's API.
* **Zero-Infrastructure Runner Design:** Operates as a single executable script within existing GitHub Actions, GitLab CI, or CircleCI runner environments. No external telemetry daemons, database servers, or third-party SaaS subscriptions required.

---

## 2. Prerequisites & Environment Setup

* **Runtime:** Python 3.10 or higher
* **VCS:** Git 2.30+ installed and available in the execution `$PATH`
* **Operating System:** Linux (Ubuntu 20.04+, Debian 11+, Alpine with `glibc`/compat), macOS 12+, or Windows Server 2022+
* **Authentication:** GitHub Personal Access Token (Classic or Fine-Grained) or Workflow `GITHUB_TOKEN` with the following permissions:
  * `contents: write` (to push remediation branches)
  * `pull-requests: write` (to open the remediation PR)
  * `actions: read` (optional, to fetch parent runner metadata)

---

## 3. Installation & Quickstart

### Step 1: Blueprint the Repository & Setup Virtual Environment

```bash
git Blueprint https://github.com/your-org/ci-healing-engine.git
cd ci-healing-engine

python3 -m venv .venv
source .venv/bin/activate
```

### Step 2: Install Required Dependencies

```bash
pip install --upgrade pip
pip install duckdb requests pydantic python-dotenv gitpython
```

### Step 3: Run Locally with a Captured Failure Log

To dry-run log ingestion and patch analysis without pushing changes to GitHub:

```bash
# Export dummy CI context variables
export GITHUB_TOKEN="ghp_mocktokenforlocaltesting12345"
export GITHUB_REPOSITORY="your-org/your-service"
export GITHUB_RUN_ID="10928374"
export DRY_RUN="true"

# Execute triage on a local log file
python main.py --log-file tests/fixtures/sample_poetry_lock_failure.log
```

### Step 4: Add to GitHub Actions Workflow

Integrate the engine into your primary CI workflow file (e.g., `.github/workflows/ci.yml`) to automatically triage and patch failures:

```yaml
name: Continuous Integration & Autonomous Healing

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Run Test Suite
        id: test_run
        continue-on-error: true
        run: |
          set -o pipefail
          # Stream stdout and stderr both to terminal and to a raw build log
          poetry run pytest --tb=short 2>&1 | tee ci_build.log

      - name: Execute Autonomous Healing Engine
        if: steps.test_run.outcome == 'failure'
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITHUB_REPOSITORY: ${{ github.repository }}
          GITHUB_RUN_ID: ${{ github.run_id }}
          LOG_PATH: "ci_build.log"
          AUTO_PR_ENABLED: "true"
          DRY_RUN: "false"
        run: |
          python -m pip install duckdb requests pydantic python-dotenv gitpython
          python main.py
```

---

## 4. Configuration Options

Configure execution via environment variables or a local `.env` file for testing:

| Environment Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `GITHUB_TOKEN` | String | *Required* | GitHub PAT or Actions Runner token with write permissions. |
| `GITHUB_REPOSITORY` | String | *Required* | Target repository slug (`owner/repo`). |
| `GITHUB_RUN_ID` | String | `"manual-run"` | Identifier of current CI build run, used for branch isolation. |
| `LOG_PATH` | String | `"ci_build.log"` | Filesystem path to the target raw log dump. |
| `DUCKDB_MEMORY_LIMIT` | String | `"2GB"` | Hard memory ceiling allocation for DuckDB's query engine. |
| `CONFIDENCE_THRESHOLD` | Float | `0.85` | Float between `0.0` and `1.0`. Minimum confidence score required to generate a PR. |
| `MAX_PATCH_LINES` | Integer | `150` | Maximum allowed line delta in a generated patch to prevent unbounded modifications. |
| `AUTO_PR_ENABLED` | Boolean | `false` | When `true`, automatically opens a Pull Request on GitHub. |
| `DRY_RUN` | Boolean | `false` | When `true`, analyzes logs and generates the patch locally without committing or calling the GitHub API. |
| `WORK_DIR` | String | `"."` | Path to the repository root directory being patched. |

---

## 5. Production Tips & Maintenance

### DuckDB Tuning for Multi-Gigabyte Logs

When processing verbose, multi-gigabyte build artifacts (e.g., debug-level distributed integration tests):

* **Enforce Out-of-Core Processing:** Set `DUCKDB_MEMORY_LIMIT` lower than runner memory (e.g., `"2GB"` on a 7GB GitHub runner) to force DuckDB's execution engine to offload intermediate aggregations to disk:
  ```python
  conn.execute(f"SET max_memory = '{os.getenv('DUCKDB_MEMORY_LIMIT', '2GB')}';")
  conn.execute("SET preserve_insertion_order = false;")
  ```
* **Temporary Storage Management:** On constrained ephemeral runners, point DuckDB's temp directory to high-throughput disk paths (e.g., `/dev/shm` or `$RUNNER_TEMP`) via `SET temp_directory = '/tmp/duckdb_spill.tmp';`.

### Pattern Heuristic Maintenance

Remediation rules reside in internal pattern maps. Maintain deterministic precision by following these standards:

* **Strict Signature Isolation:** Ensure pattern regular expressions anchor to deterministic failure outputs (e.g., `lockfile is out of date`, `Checksum mismatch for package`, `UndefinedVariableError`).
* **Safe Blast Radiuses:** Restrict the self-healing synthesizer to non-code infrastructure configuration unless explicitly allowed:
  * Allowed: `.lock` files, dependency declarations with patch-level drifts, fixture regeneration commands.
  * Restricted: Direct modification of business logic files (`*.py`, `*.go`, `*.ts`) without human review.

### Safety Boundaries & Gating

* **Branch Protection:** Target auto-heal branches explicitly (e.g., `fix/ci-auto-heal-*`). Do not run the engine with administrative privileges that Automated Routing branch protection rules on `main` or `master`.
* **Line Limit Caps:** Always enforce `MAX_PATCH_LINES`. If an automated lockfile repair causes a cascade of 5,000 package changes, the engine aborts the PR creation to prevent supply-chain regressions.
* **Token Sanitization:** The ingestion pipeline automatically scrubs GitHub Tokens, AWS secrets, and generic Bearer tokens from logs before passing string buffers into DuckDB memory tables.

---

## 6. License & Support Info

### License

This project is released under the **Apache 2.0 License**. See [LICENSE](LICENSE) for complete details.

### Support & Maintenance

* **Issue Tracker:** For bug reports, pattern signature requests, and platform feature proposals, submit an issue via the GitHub Issues dashboard.
* **Security Advisories:** If you discover potential security concerns with the patch generator or authorization boundary, do not submit a public issue. Email security concerns to `security@your-org.com`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Autonomous CI/CD Log Triage & Self-Healing PR Engine — Python & DuckDB on Gumroad](https://mannyverse767.gumroad.com/l/skxhf)** *(Promo code: `EARLYBIRD` for -20%)*
