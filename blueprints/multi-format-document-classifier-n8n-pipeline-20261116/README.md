# Multi-Format Document Classifier & Sheets Sync — n8n Pipeline

![Multi-Format Document Classifier & Sheets Sync — n8n Pipeline](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/multi-format-document-classifier-n8n-pipeline-20261116.png)

> **Automate document sorting and structured data extraction directly into Google Sheets with this self-hosted n8n pipeline.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€17 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/ojsaaa)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Processing diverse documents like invoices, contracts, and intake forms manually drains operational bandwidth and causes costly data entry errors. Proprietary enterprise automation platforms charge exorbitant monthly per-document usage fees just to route basic text into a spreadsheet.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€17 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Omni-format ingestion (PDF, DOCX, TXT, PNG, JPG)**
- ✅ **Intelligent LLM-powered document routing and classification**
- ✅ **Precision structured data extraction and normalization**
- ✅ **Automated, real-time Google Sheets row population**


---

## 📖 Technical Documentation & Setup Guide

# Multi-Format Document Classifier & Sheets Sync — n8n Pipeline

[![n8n](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=flat-square&logo=n8n&logoColor=white)](https://n8n.io/)
[![LLM Powered](https://img.shields.io/badge/LLM-Extraction-00A67E?style=flat-square&logo=openai&logoColor=white)](#)
[![Google Sheets](https://img.shields.io/badge/Google_Sheets-Sync-34A853?style=flat-square&logo=googlesheets&logoColor=white)](#)

## 1. Overview & Architecture

Processing diverse, unstructured documents (invoices, contracts, intake forms) drains operational bandwidth and introduces costly manual data entry errors. Proprietary enterprise automation platforms typically charge exorbitant monthly per-document fees simply to route text into a tabular format. 

This n8n pipeline (`workflow_n8n.json`) provides a scalable, infrastructure-agnostic alternative. It leverages an LLM to dynamically classify documents and extract structured schemas, Automated Routing legacy template-based OCR systems, and synchronizes the normalized data directly into Google Sheets.

### Core Features
*   **Omni-format Ingestion:** Natively handles `PDF`, `DOCX`, `TXT`, `PNG`, and `JPG` via n8n's binary data management and multimodal LLM integration.
*   **Intelligent Routing & Classification:** Evaluates document context using LLMs to categorize payloads (e.g., *Invoice*, *NDA*, *Intake Form*) without pre-defined templates.
*   **Precision Structured Data Extraction & Normalization:** Casts unstructured text/images into strict JSON schemas for predictable downstream processing.
*   **Automated, Real-Time Google Sheets Sync:** Appends parsed schemas as rows to targeted spreadsheets dynamically based on the classification route.

### Pipeline Architecture
1.  **Ingestion Trigger:** Webhook or Email trigger accepts incoming payloads with binary attachments.
2.  **Binary Processing & Routing:** n8n extracts binary data. Text-based files are parsed directly; images/PDFs are routed to a multimodal LLM or OCR node.
3.  **LLM Classification & Extraction Node:** Prompts the configured LLM with a strict JSON output schema.
4.  **Data Transformation:** n8n `Item Lists` and `Code` nodes normalize the JSON payload.
5.  **Target Destination:** `Google Sheets` node appends the row to the corresponding sheet/tab.

---

## 2. Prerequisites & Environment Setup

Ensure the following dependencies and credentials are provisioned before deployment:

*   **n8n Instance:** n8n version `1.0.0` or higher (Self-hosted or n8n Cloud). 
*   **LLM Provider API Key:** 
    *   OpenAI (GPT-4o recommended for multimodal image/PDF parsing), Anthropic (Claude 3.5 Sonnet), or a local equivalent (Ollama with LLaVA).
*   **Google Cloud Platform (GCP) Credentials:**
    *   A Service Account or OAuth2 configuration with the **Google Sheets API** enabled.
*   **Target Google Sheet:** An existing Google Sheet with pre-defined headers matching your extraction schema (e.g., `Date`, `Vendor`, `Total`, `Document Type`).

---

## 3. Installation & Quickstart

Follow these steps to deploy the workflow into your n8n instance.

### Step 1: Import the Workflow
1. Download the `workflow_n8n.json` file from this repository.
2. Open your n8n workspace.
3. Click **Add Workflow** in the top right.
4. Open the workflow menu (`...` in the top right) and select **Import from File**.
5. Select `workflow_n8n.json`.

### Step 2: Configure Credentials
You must map the imported nodes to your specific credentials.
1. Open the **LLM Node** (e.g., OpenAI/Anthropic) and click **Create New Credential**. Input your API Key.
2. Open the **Google Sheets Node** and authenticate using your GCP Service Account JSON or OAuth2 flow.

### Step 3: Configure Target Destinations
1. In the **Google Sheets Node**, update the `Document ID` field with the ID of your target spreadsheet (found in the URL: `https://docs.google.com/spreadsheets/d/<DOCUMENT_ID>/edit`).
2. Specify the `Sheet Name` (e.g., `Sheet1` or dynamically mapped via `{{ $json.classification_type }}`).

### Step 4: Schema Definition (Optional but Recommended)
1. Open the **LLM Extraction Node**.
2. Modify the System Message or JSON Schema configuration to match the exact keys required for your Google Sheets columns.

### Step 5: Activate and Test
1. Toggle the workflow to **Active**.
2. Send a POST request to your Webhook URL containing a test document (e.g., a PDF invoice) as form-data, or send an email to the configured trigger address.
3. Verify the row appears in Google Sheets.

---

## 4. Configuration Options (.env explanation)

If you are self-hosting n8n via Docker, optimizing your `.env` configuration is critical for handling large document payloads reliably. Below are the recommended environment variables:

```bash
# n8n Core Settings
# Store binary data on disk instead of memory to prevent OOM errors with large PDFs
N8N_DEFAULT_BINARY_DATA_MODE=filesystem

# Increase the payload size limit for large document uploads via Webhook
N8N_PAYLOAD_SIZE_MAX=50

# Execution data management (prune successful executions to save DB space)
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168
EXECUTIONS_DATA_PRUNE_MAX_COUNT=10000

# Optional: Inject API keys directly via ENV for better credential management
CREDENTIALS_OVERWRITE_DATA={"openAiApi":{"api_key":"sk-YOUR_API_KEY_HERE"}}
```

---

## 5. Production Tips & Maintenance

To ensure high availability and data integrity in production, implement the following practices:

*   **Binary Data Management:** Always use `filesystem` binary data mode (as shown in Section 4). Processing multi-page PDFs in memory will cause Node.js heap out-of-memory (OOM) crashes.
*   **Error Handling & Dead-Letter Queue:** The current workflow should be wrapped in an Error Trigger workflow. If the LLM hallucinates a schema or the Google Sheets API rate-limits the request, catch the error and route the original payload to a "Manual Review" Slack/Discord channel or a fallback local storage node.
*   **API Rate Limiting:** If processing batch uploads, utilize n8n's `Split In Batches` (now `Loop`) node combined with a `Wait` node to respect the LLM provider's Token-Per-Minute (TPM) limits.
*   **Prompt Versioning:** Maintain your extraction JSON schemas and system prompts in a version-controlled repository. Drift in LLM models can occasionally cause schema malformation; use rigid JSON mode (e.g., OpenAI's `response_format: { type: "json_object" }`).

---

## 6. License & Support Info

**License:** This workflow and accompanying documentation are licensed under the MIT License. See the `LICENSE` file for details.

**Support:** 
This pipeline is provided as-is. For bug reports, feature requests, or contributions regarding the `workflow_n8n.json` logic, please open an issue in the repository's Issue Tracker.

For n8n-specific node execution errors, refer to the [n8n Community Forum](https://community.n8n.io/) or the [official n8n documentation](https://docs.n8n.io/).

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Multi-Format Document Classifier & Sheets Sync — n8n Pipeline on Gumroad](https://mannyverse767.gumroad.com/l/ojsaaa)** *(Promo code: `EARLYBIRD` for -20%)*
