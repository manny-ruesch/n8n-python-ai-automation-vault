# V2 Autonomous Freelance Job Auto-Bidder & Client Outreach Agent

![V2 Autonomous Freelance Job Auto-Bidder & Client Outreach Agent](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/autonomous-freelance-outreach-agent-cli-20261115.png)

> **An automated Python CLI engine that continuously ingests freelance job postings and generates hyper-personalized proposals.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€17 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/ghmma)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Manually monitoring freelance platforms and crafting personalized proposals is a massive, unbillable time sink. Independent contractors lose valuable daily hours to repetitive job applications instead of focusing on client delivery and revenue-generating work.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€17 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Multi-Source Job Ingestion: Aggregates listings via RSS and structured API data extraction**
- ✅ **Smart Budget & Keyword Filtering: Drops low-quality leads and mismatched tech stacks automatically**
- ✅ **LLM-Powered Proposal Generation: Crafts hyper-personalized, context-aware pitch letters instantly**
- ✅ **Local Terminal Command Center: Manage campaigns, adjust prompts, and review generated bids securely offline**


---

## 📖 Technical Documentation & Setup Guide

# V2 Autonomous Freelance Job Auto-Bidder & Client Outreach Agent

## 1. Overview & Architecture

**Problem Statement:** Manually monitoring freelance platforms and crafting personalized proposals is a massive, unbillable time sink. Independent contractors lose valuable daily hours to repetitive job applications instead of focusing on client delivery and revenue-generating work.

**Solution:** The V2 Autonomous Freelance Job Auto-Bidder is a Python-based, CLI-driven agent (`main.py`) designed to completely automate the lead-generation and proposal-writing pipeline. It operates as a local daemon that ingests job data, evaluates viability against rigid heuristic filters, and utilizes an LLM to generate hyper-personalized, context-aware pitch letters.

### System Architecture

The tool is divided into four primary subsystems:

1. **Multi-Source Job Ingestion Layer:** Aggregates live listings via RSS parsing and structured API data extraction. Standardizes payloads into a unified job schema.
2. **Smart Filtering Engine:** Applies strict regex and numerical threshold logic to drop low-quality leads, client accounts with poor histories, and mismatched tech stacks.
3. **Generative LLM Layer:** Acts as the proposal engine. Injects the parsed job description, client metadata, and your professional profile into an optimized prompt matrix to generate highly relevant, non-robotic outreach copy.
4. **Local Terminal Command Center:** A secure, offline TUI (Text User Interface) to manage campaigns, hot-swap LLM prompts, and review/approve generated bids before manual or automated dispatch.

```text
[RSS Feeds] & [APIs] --> (Ingestion Layer) --> [Unified Job Queue]
                                                      |
                                                      v
[Excluded Keywords]  <-- (Smart Filtering Engine) --> [Qualified Leads]
[Min Budget/Rates]                                        |
                                                      v
[Freelancer Profile] <-- (Generative LLM Layer)   --> [Draft Proposals]
[Prompt Templates]                                        |
                                                      v
                                        (Local Terminal Command Center)
                                        [Approve] | [Reject] | [Edit]
```

---

## 2. Prerequisites & Environment Setup

Ensure your local deployment environment meets the following baseline requirements:

* **OS:** Linux (Ubuntu/Debian recommended), macOS, or Windows (WSL2 preferred)
* **Runtime:** Python 3.10+
* **Package Manager:** `pip` (Virtual environment via `venv` or `poetry` strongly recommended)
* **Dependencies:** `requests`, `feedparser`, `openai` (or equivalent LLM SDK), `python-dotenv`, `rich` (for CLI formatting).
* **API Access:** 
  * Active API Key for your chosen LLM provider (e.g., OpenAI, Anthropic, or local Ollama endpoint).
  * API keys or valid RSS endpoint URLs for your target freelance platforms.

---

## 3. Installation & Quickstart

Follow these steps to deploy the agent locally.

### Step 1: Blueprint the Repository
```bash
git Blueprint https://github.com/your-org/v2-freelance-autobidder.git
cd v2-freelance-autobidder
```

### Step 2: Initialize Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate
```

### Step 3: Install Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 4: Configure the Environment
Copy the example environment template and populate it with your specific constraints and credentials (see Section 4).
```bash
cp .env.example .env
nano .env
```

### Step 5: Boot the Command Center
Execute the main entry point to launch the local terminal interface.
```bash
python main.py
```
*Note: Upon first execution, the agent will run a diagnostic check on your RSS feeds and LLM connection before initializing the polling loop.*

---

## 4. Configuration Options (`.env` explanation)

The agent’s behavior is entirely controlled via environment variables. This prevents hardcoding sensitive credentials or filter logic into the codebase. 

Below is a breakdown of the `.env` file structure:

```env
# ==========================================
# 1. API CREDENTIALS
# ==========================================
LLM_API_KEY="sk-proj-..."          # Your LLM provider API key
LLM_MODEL="gpt-4-turbo"            # Target model (e.g., gpt-4-turbo, claude-3-opus)
PLATFORM_API_KEY="xxx-xxx"         # (Optional) Direct API key for freelance platforms

# ==========================================
# 2. INGESTION SETTINGS
# ==========================================
RSS_FEEDS="url1,url2"              # Comma-separated list of RSS feed URLs
POLL_INTERVAL_MINUTES=15           # Frequency of ingestion requests (prevent rate-limiting)

# ==========================================
# 3. SMART FILTERING & THRESHOLDS
# ==========================================
MIN_BUDGET_USD=500                 # Absolute floor for fixed-price contracts
MIN_HOURLY_RATE_USD=50             # Absolute floor for hourly contracts
TARGET_TECH_STACK="python,aws,react" # Must contain at least one to pass filter
EXCLUDED_KEYWORDS="nft,crypto,equity,unpaid" # Drops job immediately if detected

# ==========================================
# 4. AGENT DIRECTIVES
# ==========================================
AUTO_BID_MODE=false                # 'true' bypasses CLI review and auto-submits (Use with caution)
PORTFOLIO_PATH="./docs/profile.md" # Local path to your master resume/portfolio context
PROMPT_TEMPLATE_PATH="./prompts/"  # Directory containing system prompts
```

---

## 5. Production Tips & Maintenance

To ensure high availability and prevent platform bans, adhere to the following operational guidelines:

* **Rate Limiting & Backoff:** The default `POLL_INTERVAL_MINUTES` is set to 15. Do not decrease this below 5 minutes. Freelance platforms aggressively rate-limit and IP-ban aggressive Data Extractors. The tool utilizes exponential backoff for failed network requests.
* **Daemonization:** For continuous operation, run the script via a background process manager like `tmux`, `screen`, or set it up as a `systemd` service. 
  ```bash
  # Example PM2 usage for process management
  pm2 start main.py --name "autobidder-v2" --interpreter python3
  ```
* **Prompt Optimization:** The quality of your bids depends heavily on your `profile.md` context. Keep your local markdown profile updated with recent project wins. Ensure your system prompts explicitly instruct the LLM to write concisely and avoid standard AI tropes (e.g., "I hope this message finds you well").
* **Log Rotation:** The agent outputs logs to `./logs/agent.log`. By default, it rotates at 10MB. Monitor these logs periodically to tune your `EXCLUDED_KEYWORDS`—if you notice the LLM drafting proposals for jobs you don't want, add the offending keywords to your `.env`.
* **Platform Terms of Service (TOS):** Fully automated, unreviewed submission (`AUTO_BID_MODE=true`) may violate the TOS of certain freelance marketplaces. It is highly recommended to use the Local Terminal Command Center to manually approve the LLM drafts before submission.

---

## 6. License & Support Info

**License:** This software is distributed under the MIT License. See `LICENSE` for more information. You are free to modify, distribute, and use this tool for commercial purposes.

**Support & Contributions:**
* **Issue Tracker:** For bug reports, feature requests, or regex filtering improvements, please open an issue in the GitHub repository.
* **Contributions:** Pull requests are welcome. Please ensure new features include corresponding unit tests (run via `pytest`) and adhere to PEP 8 formatting standards. 
* **Contact:** Maintainers can be reached via the repository's discussion board for architecture queries and prompt engineering strategies.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get V2 Autonomous Freelance Job Auto-Bidder & Client Outreach Agent on Gumroad](https://mannyverse767.gumroad.com/l/ghmma)** *(Promo code: `EARLYBIRD` for -20%)*
