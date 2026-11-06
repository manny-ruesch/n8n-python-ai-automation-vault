# AI-Powered B2B Lookalike Generator & ICP Profiler — n8n Workflow

![AI-Powered B2B Lookalike Generator & ICP Profiler — n8n Workflow](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/ai-b2b-lookalike-icp-profiler-n8n-20261106.png)

> **Automate B2B prospect discovery and deep AI profiling directly on your own infrastructure.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/hzvfbi)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Finding highly specific B2B prospects that match your exact Ideal Customer Profile usually requires paying per-credit fees to expensive enterprise data providers. This workflow automates the discovery, web ingestion, and deep AI profiling of lookalike companies directly inside your own infrastructure, entirely Automated Routing credit-based SaaS traps.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Automated lookalike discovery via search engine APIs**
- ✅ **Deep AI-driven ICP scoring (0-100 fit metric)**
- ✅ **Zero-credit web ingestion and metadata extraction**
- ✅ **Export-ready CSV or direct CRM routing**


---

## 📖 Technical Documentation & Setup Guide

# AI-Powered B2B Lookalike Generator & ICP Profiler — n8n Workflow

[![n8n Workflow](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=flat-square&logo=n8n)](https://n8n.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.style=flat-square)](https://opensource.org/licenses/MIT)

## 1. Overview & Architecture

Finding highly specific B2B prospects that match an exact Ideal Customer Profile (ICP) typically relies on expensive, credit-based enterprise data providers (e.g., ZoomInfo, Apollo). This workflow provides an open, infrastructure-level alternative. 

By deploying this workflow within your own n8n instance, you can automate the discovery, web ingestion, and deep AI profiling of lookalike companies, Automated Routing SaaS data silos entirely.

### Core Capabilities
* **Automated Lookalike Discovery**: Utilizes search engine APIs (e.g., Google Custom Search, SerpApi) to identify competitor lookalikes based on seed domains or niche keywords.
* **Zero-Credit Web Ingestion**: Directly scrapes target company websites, extracting raw DOM data and metadata without relying on per-request Data Ingestion APIs.
* **Deep AI-Driven ICP Scoring**: Leverages LLMs to analyze scraped content against a predefined ICP matrix, outputting a rigid `0-100` fit metric and reasoning JSON.
* **Flexible Routing**: Outputs highly enriched data to a standard CSV format or routes directly via API to your CRM (HubSpot, Salesforce, etc.).

### Architecture Flow
```text
[Trigger/Webhook] -> [Search API Node: Find Similar Domains] 
                  -> [HTTP Request Node: Ingest Target HTML]
                  -> [HTML/Markdown Parser Node: Clean DOM payload]
                  -> [AI Agent/LLM Node: Evaluate ICP Fit (0-100)]
                  -> [Conditional Logic: Filter if Score > 75]
                  -> [CRM/CSV Export Node]
```

---

## 2. Prerequisites & Environment Setup

To run this workflow, your environment must meet the following requirements:

* **n8n Instance**: n8n v1.0.0 or higher. Self-hosted (Docker/npm) is highly recommended to avoid cloud execution limits during heavy Data Ingestion operations.
* **LLM Provider**: OpenAI API key (GPT-4o-mini recommended for cost/speed) OR Anthropic API key OR a local LLM via Ollama.
* **Search API Provider**: SerpApi, Google Custom Search API, or a locally hosted SearXNG instance.
* **Hardware Profile**: If hosting locally, ensure at least 2GB RAM. Web Data Extraction and text parsing can consume significant memory depending on batch size.

---

## 3. Installation & Quickstart

### Step 1: Blueprint the Repository
Download the workflow JSON file directly from this repository:
```bash
wget https://raw.githubusercontent.com/your-repo/workflow_n8n.json
```

### Step 2: Import into n8n
1. Open your n8n UI.
2. Navigate to **Workflows** -> **Add Workflow**.
3. Click the `...` menu in the top right corner and select **Import from File**.
4. Upload `workflow_n8n.json`.

### Step 3: Configure Credentials
You will see several nodes with missing credentials. Create and attach the following in the n8n Credentials manager:
* **Search API Credentials**: Input your API key for your chosen search provider.
* **OpenAI/LLM Credentials**: Input your API key.
* **CRM Credentials (Optional)**: If you plan to route directly to a CRM, authenticate via OAuth2 or API key.

### Step 4: Define your ICP
Open the **AI Profiler Node** (LLM prompt). Modify the system prompt's JSON schema to reflect your exact ICP. 
*Example target profile:* "B2B SaaS companies in logistics, 50-200 employees, utilizing SOC2."

### Step 5: Execute
Click **Execute Workflow**. Input a seed company or keyword array in the trigger node to begin the pipeline.

---

## 4. Configuration Options (`.env` explanation)

If you are running n8n via Docker, it is best practice to pass environment variables directly into your `docker-compose.yml` to securely manage workflow parameters and increase node limits.

Create a `.env` file in your n8n root directory:

```env
# ----------------------------------------
# n8n Core Settings
# ----------------------------------------
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
EXECUTIONS_DATA_SAVE_ON_ERROR=all
EXECUTIONS_DATA_SAVE_ON_SUCCESS=none     # Prevent database bloat during large batch runs
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168              # Keep execution logs for 7 days

# ----------------------------------------
# Workflow Integrations (referenced in n8n)
# ----------------------------------------
# These can be accessed inside the workflow via {{$env["VAR_NAME"]}}
OPENAI_API_KEY=sk-xxxx...
SEARCH_API_KEY=xxxx...
TARGET_CRM_API_URL=https://api.hubapi.com/crm/v3/objects/companies

# ----------------------------------------
# Node Performance Tuning
# ----------------------------------------
N8N_DEFAULT_TIMEOUT=300000               # Increase timeout for slow website ingestion
NODE_OPTIONS="--max-old-space-size=4096" # Allocate 4GB RAM to prevent OOM on large DOM parses
```

---

## 5. Production Tips & Maintenance

To ensure this workflow operates reliably in a production environment, implement the following guardrails:

### A. Managing Rate Limits & IP Bans
* **HTTP 429 Errors**: Target websites frequently block aggressive Data Ingestion. Utilize n8n's **Split in Batches** node to process 5-10 URLs at a time, combined with a **Wait** node (e.g., 5 seconds) to respect server rate limits.
* **Proxy Rotation**: For high-volume discovery, route the *HTTP Request* (Web Ingestion) node through a rotating residential proxy pool.

### B. Payload Optimization (Reducing Token Costs)
* Do not pass raw HTML directly to the LLM. 
* Ensure the **HTML/Markdown Parser Node** is correctly stripping `<script>`, `<style>`, and `<nav>` tags. Extracting only the `<main>` or `<body>` inner text reduces LLM token consumption by up to 90% and prevents context-window overflow.

### C. Prompt Engineering for ICP Scoring
Force the LLM to return strict JSON by utilizing n8n's structured output features or specifying the exact JSON schema in the system prompt. Example output requirement:
```json
{
  "company_name": "string",
  "icp_score": "number (0-100)",
  "reasoning": "string (max 50 words)",
  "key_technologies": ["string"]
}
```

### D. CRM Deduplication
Before writing a processed lookalike to your CRM, insert an HTTP Request node to query the CRM by the target's domain name. If a match exists, use a Switch node to route to an "Update" flow rather than a "Create" flow.

---

## 6. License & Support Info

### License
This project is licensed under the **MIT License**. See the `LICENSE` file for details. You are free to modify, distribute, and use this workflow in commercial environments.

### Disclaimer
Web Data Extraction is subject to the Terms of Service of the target websites. It is your responsibility to ensure compliance with `robots.txt`, data privacy laws (GDPR, CCPA), and local regulations when operating this workflow.

### Support
* For issues related to n8n execution, refer to the [n8n Community Forums](https://community.n8n.io/).
* To report bugs or request features specific to this workflow, please open an issue on the repository issue tracker. Include your n8n version and a sanitized copy of your execution logs.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get AI-Powered B2B Lookalike Generator & ICP Profiler — n8n Workflow on Gumroad](https://mannyverse767.gumroad.com/l/hzvfbi)** *(Promo code: `EARLYBIRD` for -20%)*
