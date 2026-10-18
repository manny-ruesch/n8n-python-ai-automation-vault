# Local Repo Refactoring Agent: AI Codebase Migration Engine

![Local Repo Refactoring Agent: AI Codebase Migration Engine](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/local-repo-refactoring-agent-20261016.png)

> **Secure, local-first AI engine for automated codebase migrations and large-scale refactoring with absolute IP privacy.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/avdqx)** | ⚡ **[Instant Access on Whop](https://whop.com/mr-studio-7faa/local-repo-refactoring-agent-ai-codebase-migration-engine)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Migrating legacy codebases or enforcing sweeping architectural changes typically requires hundreds of costly manual engineering hours. Cloud-based AI tools pose severe security risks by exposing proprietary IP, while manual refactoring leads to prolonged downtime and inevitable human error.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Local-First Execution: Processes repositories entirely on your local machine to guarantee zero IP leakage.**
- ✅ **Strict Permission Gates: Granular CLI approval flows (Y/N/Modify) for every file modification.**
- ✅ **Automated Tech-Stack Migration: Systematically translate syntax and update deprecated frameworks across hundreds of files.**
- ✅ **Abstract Syntax Tree (AST) Validation: Ensures structural integrity and syntactical correctness before applying code mutations.**


---

## 📖 Technical Documentation & Setup Guide

# Local Repo Refactoring Agent: AI Codebase Migration Engine

[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Execution](https://img.shields.io/badge/Execution-Local_First-success.svg)]()

## 1. Overview & Architecture

Migrating legacy codebases or enforcing sweeping architectural changes typically requires hundreds of costly manual engineering hours. Relying on cloud-based AI tools to accelerate this process introduces severe security risks by exposing proprietary intellectual property (IP). Conversely, manual refactoring leads to prolonged downtime, merge conflicts, and inevitable human error. 

The **Local Repo Refactoring Agent** is a localized, AI-driven codebase migration engine designed to execute large-scale structural transformations without exposing source code to external servers.

### Core Features
*   **Local-First Execution:** Processes repositories entirely on your local machine using local LLM inference, guaranteeing zero IP leakage.
*   **Automated Tech-Stack Migration:** Systematically translates syntax, updates deprecated frameworks, and implements architectural design patterns across hundreds of files simultaneously.
*   **Abstract Syntax Tree (AST) Validation:** Parses and evaluates the structural integrity of both the source and the generated code. Mutations are rejected in memory if syntactical correctness is compromised, preventing broken builds.
*   **Strict Permission Gates:** Granular CLI approval flows (`[Y]es / [N]o / [M]odify / [A]bort`) for every file mutation ensure engineers maintain absolute control over the codebase.

### System Architecture
1.  **Ingestion:** Recursively walks the target directory, filtering files via `.gitignore` and `.agentignore` policies.
2.  **AST Parsing:** Constructs an AST of the target files to understand scope, dependencies, and structure.
3.  **Inference Engine:** Passes code chunks and migration directives to the local AI backend (e.g., Ollama, vLLM, or LM Studio) to generate transformations.
4.  **Validation Pipeline:** Converts the AI-generated output back into an AST. Compares syntax validity against the target language specifications.
5.  **Interactive Gate:** Presents a unified `diff` to the user in the terminal. Awaits manual confirmation before writing changes to disk.

---

## 2. Prerequisites & Environment Setup

Ensure your local development environment meets the following baseline requirements before deploying the agent.

*   **Python:** `3.9.x` or higher.
*   **Git:** Required for pre-execution state validation and diff generation.
*   **Local Inference Runtime:** 
    *   [Ollama](https://ollama.ai/) (Recommended for rapid setup)
    *   [vLLM](https://github.com/vllm-project/vllm) or [llama.cpp](https://github.com/ggerganov/llama.cpp) (For advanced GPU utilization)
*   **Hardware Setup:** Minimum 16GB RAM (32GB+ recommended for running 13B+ parameter models locally). Apple Silicon (M1/M2/M3 Max) or discrete NVIDIA GPUs (RTX 3090/4090) are highly recommended to accelerate AST generation and inference.

---

## 3. Installation & Quickstart

### Step 1: Blueprint the Repository
```bash
git Blueprint https://github.com/your-org/local-repo-refactoring-agent.git
cd local-repo-refactoring-agent
```

### Step 2: Establish Virtual Environment
```bash
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 3: Start Local Inference Backend
*Example using Ollama with a coding-specific model:*
```bash
ollama run codellama:13b  # or phind-codellama, deepseek-coder, etc.
```

### Step 4: Execute a Migration Run
Invoke the main application targeting a specific directory and providing a natural language migration rule.

```bash
python main.py \
  --target-dir "../legacy-service/src" \
  --rule "Migrate all class-based React components to functional components using hooks." \
  --lang "javascript"
```

### Interactive CLI Flow Example
During execution, the agent will pause at the **Strict Permission Gate**:

```diff
[AST Validated] Processing: ../legacy-service/src/components/Header.js

- class Header extends React.Component {
-   render() {
-     return <div>{this.props.title}</div>;
-   }
- }
+ const Header = ({ title }) => {
+   return <div>{title}</div>;
+ };

? Apply this mutation? [Y]es / [N]o / [M]odify in editor / [A]bort: 
```

---

## 4. Configuration Options

Configuration is managed via environment variables. Copy the provided template to establish your local overrides.

```bash
cp .env.example .env
```

### `.env` File Reference

| Variable | Description | Default |
| :--- | :--- | :--- |
| `LLM_BACKEND_URI` | HTTP endpoint for the local inference engine. | `http://localhost:11434/api/generate` |
| `LLM_MODEL_NAME` | The specific model identifier to be queried. | `codellama:13b` |
| `CONTEXT_WINDOW` | Maximum token context allowed per request. | `8192` |
| `AST_STRICT_MODE` | Boolean. If `true`, the agent strictly rejects code that fails AST parsing. | `true` |
| `AUTO_APPROVE` | **DANGER:** Bypasses the CLI approval gate. Only use in CI/CD sandbox environments. | `false` |
| `BACKUP_DIR` | Local path where pre-mutation file copies are stored. | `./.agent_backups` |
| `MAX_CONCURRENT_WORKERS` | Number of files to process simultaneously. Keep low for local LLMs. | `1` |

---

## 5. Production Tips & Maintenance

To maximize efficiency and safeguard your codebase, adhere to the following operational best practices:

*   **Enforce Pre-Execution Commits:** The agent will check `git status` by default and warn you if there are uncommitted changes. Always start a migration on a clean, isolated branch (e.g., `feature/ai-migration-react-hooks`).
*   **Batch Processing:** Do not attempt to process 10,000 files in a single run. Use granular target directories (e.g., `--target-dir ./src/auth`) to maintain predictable inference latency and prevent context degradation.
*   **AST Parser Tuning:** If you are migrating custom DSLs (Domain Specific Languages) or heavily macro-dependent C++ code, you may need to register custom AST parsing rules in `parsers/custom_rules.py` to prevent false-positive validation failures.
*   **Audit Logging:** All accepted, rejected, and modified transactions are appended to `agent_audit.log`. Ingest this file into your observability stack for post-migration reviews.
*   **Clearing Backups:** The agent writes pre-mutation states to `BACKUP_DIR`. Run `python main.py --clean-backups` periodically to free up disk space after successful merges.

---

## 6. License & Support Info

### License
This project is distributed under the **Apache License, Version 2.0**. You may obtain a copy of the License at:
[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

### Support & Contributions
*   **Issues:** Report bugs, AST parsing failures, or LLM context issues via the [GitHub Issue Tracker](#).
*   **Contributing:** We welcome pull requests for additional AST language parsers (e.g., Rust, Go, Ruby) and integrations with other local LLM runtimes. Please review `CONTRIBUTING.md` for architectural guidelines and PR formatting requirements.
*   **Security:** If you discover a vulnerability that could Automated Routing local execution or result in unintended telemetry leakage, please email `security@your-org.com` directly rather than opening a public issue.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Local Repo Refactoring Agent: AI Codebase Migration Engine on Gumroad](https://mannyverse767.gumroad.com/l/avdqx)** *(Promo code: `EARLYBIRD` for -20%)*
