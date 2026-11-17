# Open-Source AI Code Publishing Pipeline — n8n Workflow

![Open-Source AI Code Publishing Pipeline — n8n Workflow](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/open-source-ai-code-publishing-n8n-20261115.png)

> **Instantly convert internal AI-generated code snippets into documented, licensed open-source Git commits automatically.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/mspfgt)** | ⚡ **[Instant Access on Whop](https://whop.com/mr-studio-7faa/open-source-ai-code-publishing-pipeline-n8n-workflow)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Engineering teams generate thousands of lines of valuable utility code using AI, but it gets permanently lost in chat histories or isolated private repositories. Manually documenting, licensing, and pushing this AI-generated code to public open-source repositories requires tedious context-switching and breaks developer momentum.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Automated Markdown README generation from raw AI code snippets**
- ✅ **Automatic injection of standard Open-Source licenses (MIT, Apache 2.0)**
- ✅ **Direct remote Git repository committing via secure API integration**
- ✅ **Webhook triggers for seamless integration with internal chat or developer tools**


---

## 📖 Technical Documentation & Setup Guide

# Open-Source AI Code Publishing Pipeline — n8n Workflow

[![n8n](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=flat-square&logo=n8n&logoColor=white)](https://n8n.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)

Engineering teams generate thousands of lines of valuable utility code using AI, which frequently becomes permanently siloed in chat histories or isolated private repositories. Manually documenting, licensing, and migrating this AI-generated code to public open-source repositories requires tedious context-switching and disrupts developer momentum. 

This n8n workflow provides an automated, zero-friction pipeline to instantly publish AI-generated utility code to public repositories directly from your existing developer tools.

---

## 1. Overview & Architecture

The **Open-Source AI Code Publishing Pipeline** is an event-driven n8n workflow designed to process raw code payloads, enrich them via LLM, and commit them directly to a version control system.

### Key Features
* **Webhook Triggers:** Accepts raw POST requests for seamless integration with internal chat platforms (Slack/Discord/Teams) or developer tools (CLI, Raycast, IDE extensions).
* **Automated Markdown Generation:** Leverages AI nodes to parse raw code snippets and automatically generate comprehensive, standardized `README.md` files (including usage examples and API documentation).
* **Automated License Injection:** Dynamically appends standard Open-Source licenses (MIT, Apache 2.0) based on payload parameters.
* **Direct Git Provider Integration:** Commits files directly to remote repositories via secure API integration (GitHub/GitLab REST APIs), Automated Routing the need for local git cloning or shell execution.

### Architecture Flow
1. **Webhook Node:** Listens for incoming `POST` requests containing the raw code snippet, language, filename, and desired license.
2. **AI Processing Node:** Sends the code context to an LLM (e.g., OpenAI/Anthropic) with a strict prompt to return a structured Markdown README.
3. **Data Transformation:** Merges the raw code, generated README, and standard license text into a commit-ready JSON array.
4. **Git HTTP Request Node:** Pushes the file tree to the designated Git provider, creating a new commit on the `main` branch.
5. **Notification (Optional):** Returns the public repository URL back to the webhook caller or an internal chat channel.

---

## 2. Prerequisites & Environment Setup

To deploy and execute this workflow, ensure the following prerequisites are met:

* **n8n Instance:** n8n version `1.0.0` or higher (self-hosted or n8n Cloud).
* **AI Provider API Key:** Active credentials for OpenAI, Anthropic, or an equivalent LLM provider configured in n8n.
* **Git Provider Credentials:** A Personal Access Token (PAT) with `repo` scope for GitHub or `api` scope for GitLab.
* **Target Repository:** An existing, initialized Git repository (e.g., `your-org/ai-utilities`) with a default branch.

---

## 3. Installation & Quickstart

### Step 1: Import the Workflow
1. Download the `workflow_n8n.json` file from this repository.
2. Open your n8n workspace.
3. In the Workflows dashboard, click **Add Workflow**.
4. Click the **Options (gear) icon** in the top right > **Import from File**.
5. Select `workflow_n8n.json`.

### Step 2: Configure Credentials
1. **Webhook Node:** Configure authentication (Basic Auth or Header Auth) to secure the endpoint. Note the Production URL.
2. **AI Node:** Select your preferred LLM provider and attach your pre-configured API credentials.
3. **Git API Node:** Set up HTTP Request credentials using Header Auth:
   * **Name:** `Authorization`
   * **Value:** `Bearer YOUR_GIT_PERSONAL_ACCESS_TOKEN`

### Step 3: Test the Pipeline
Execute the following `curl` command to simulate a payload from a developer tool. (Update the URL to match your n8n webhook endpoint).

```bash
curl -X POST "https://your-n8n-instance.com/webhook/publish-ai-code" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer YOUR_WEBHOOK_SECRET" \
     -d '{
           "filename": "json_flattener.py",
           "language": "python",
           "license": "MIT",
           "description": "Flattens deeply nested JSON objects into a single-level dictionary with dot-notation keys.",
           "code": "def flatten_json(y): out = {}; def flatten(x, name=\"\"): ... return out",
           "repo": "your-org/ai-utilities"
         }'
```

Check the n8n execution log to verify the AI transformation and subsequent Git commit.

---

## 4. Configuration Options

While the workflow dictates the operational logic, several environment variables and payload parameters control its behavior. 

### Webhook Payload Schema

| Key | Type | Description | Required |
| :--- | :--- | :--- | :--- |
| `filename` | String | Target filename (e.g., `utils.js`). | Yes |
| `language` | String | Programming language for syntax highlighting. | Yes |
| `license` | String | Open-source license type (`MIT` or `Apache-2.0`). | Yes |
| `code` | String | The raw AI-generated code snippet. | Yes |
| `description`| String | Brief context to aid the LLM in README generation. | Optional |
| `repo` | String | Target repository path (e.g., `user/repo`). | Yes |

### n8n Environment Variables (`.env`)
If you are self-hosting n8n, ensure your container exposes the following variables to allow large payloads and stable AI execution:

```env
# Allow larger raw code payloads
N8N_PAYLOAD_SIZE_MAX=10MB

# Increase timeout limits for LLM generation
N8N_TIMEOUT_HTTP_REQUEST=120000

# Recommended: Enable execution pruning to save database space
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168
```

---

## 5. Production Tips & Maintenance

* **Security & Authentication:** Never expose the Webhook node without authentication. Enforce Header Auth or basic IP whitelisting. If integrating with Slack, validate the Slack signing secret using a Crypto node before processing.
* **LLM Prompts & Formatting:** The AI node prompt is pre-configured for standard Markdown, but you should adjust the system prompt within the workflow to match your organization's specific documentation standards or internal style guides.
* **Monorepo Directory Routing:** To avoid cluttering the repository root, modify the Git API Node's path configuration to commit files into language-specific subdirectories dynamically (e.g., `/python/${filename}`).
* **Error Handling & Alerting:** The workflow includes a global Error Trigger. Attach this to a Slack/Discord node to notify the engineering team if an API rate limit is hit or a Git commit fails due to merge conflicts.
* **Model Selection:** For simple scripts, `gpt-3.5-turbo` or `claude-3-haiku` is sufficient and fast. For complex multi-file structures, upgrade the AI node model to `gpt-4o` or `claude-3.5-sonnet` for better documentation accuracy.

---

## 6. License & Support Info

**License:** 
This workflow is licensed under the **MIT License**. You are free to modify, distribute, and integrate it into your proprietary systems. 

**Support:**
* For issues related to workflow execution, verify your API payload structure and LLM rate limits.
* To report bugs or request features regarding this pipeline, please open an Issue in the source repository.
* For general n8n troubleshooting, refer to the [n8n Community Forums](https://community.n8n.io/).

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Open-Source AI Code Publishing Pipeline — n8n Workflow on Gumroad](https://mannyverse767.gumroad.com/l/mspfgt)** *(Promo code: `EARLYBIRD` for -20%)*
