# Local Business Data Ingestion & Qualification Pipeline (n8n + Python)

![Local Business Data Ingestion & Qualification Pipeline (n8n + Python)](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/local-business-lead-qualification-n8n-20261105.png)

> **Self-hosted n8n and Python pipeline to ingest, enrich, and qualify local business leads automatically.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/npbugi)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Building local business lead lists manually takes hundreds of hours, while enterprise B2B data platforms charge thousands per year for outdated, heavily-recycled local data. You need a self-hosted pipeline to automatically ingest, enrich, and qualify fresh local business profiles at scale, without recurring subscription fees.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Automated geospatial data ingestion via Python for precise local business targeting.**
- ✅ **Intelligent n8n routing to filter businesses by review score, website presence, and contact availability.**
- ✅ **Built-in HTTP enrichment nodes to discover missing email addresses and social profiles.**
- ✅ **Zero-recurring-cost architecture designed for self-hosted deployment.**


---

## 📖 Technical Documentation & Setup Guide

# Local Business Data Ingestion & Qualification Pipeline (n8n + Python)

Building local business lead lists manually takes hundreds of hours, while enterprise B2B data platforms charge thousands per year for outdated, heavily-recycled local data. 

This repository provides a self-hosted, zero-recurring-cost ETL pipeline to automatically ingest, enrich, and qualify fresh local business profiles at scale. By coupling Python-based geospatial data extraction with n8n's visual node-based routing, this pipeline ensures high-fidelity lead generation tailored strictly to your qualification parameters.

---

## 1. Overview & Architecture

This solution bypasses expensive proprietary B2B databases by directly extracting geospatial point-of-interest (POI) data and passing it through a strict qualification matrix. 

**Architecture Flow:**
1. **Extraction (Python):** A Python script queries geospatial data sources (e.g., OpenStreetMap/Overpass API, Google Places) via bounding boxes or radius parameters to identify local businesses matching specific categories.
2. **Ingestion (n8n Webhook):** Python pushes the raw POI data payloads to an n8n Webhook trigger.
3. **Filtering & Routing (n8n):** n8n `Switch` and `If` nodes intelligently evaluate the payload against baseline criteria (e.g., `review_score >= 4.0`, `website_exists == true`, `phone_number != null`).
4. **Enrichment (n8n HTTP Nodes):** For businesses passing the initial filter, native n8n HTTP Request nodes execute targeted scrapes or query zero-cost OSINT endpoints to discover missing email addresses and social profiles.
5. **Load (n8n):** Fully qualified and enriched profiles are routed to your destination data store (PostgreSQL, CRM, or Google Sheets).

### Key Features
* Automated geospatial data ingestion via Python for precise local business targeting.
* Intelligent n8n routing to filter businesses by review score, website presence, and contact availability.
* Built-in HTTP enrichment nodes to discover missing email addresses and social profiles.
* Zero-recurring-cost architecture designed for self-hosted deployment.

---

## 2. Prerequisites & Environment Setup

To deploy and run this pipeline, ensure the following dependencies are provisioned in your environment:

* **n8n Instance:** v1.0.0 or higher (Docker deployment recommended).
* **Python:** v3.10+ (for the geospatial ingestion engine).
* **Package Manager:** `pip` or `poetry`.
* **Network:** The Python environment must have egress access to your n8n instance's Webhook URL.

### Recommended Infrastructure
For a production self-hosted setup, we recommend using Docker Compose to spin up n8n alongside a PostgreSQL database for state management.

---

## 3. Installation & Quickstart

Follow these steps to deploy the ingestion script and workflow.

### Step 1: Blueprint the Repository
```bash
git Blueprint https://github.com/your-org/local-biz-pipeline.git
cd local-biz-pipeline
```

### Step 2: Set Up the Python Ingestion Engine
Initialize your virtual environment and install the required dependencies (e.g., `requests`, `geopandas`, `shapely`).

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Step 3: Import the n8n Workflow
The routing and enrichment logic is contained within the provided `workflow_n8n.json` file.

1. Open your n8n UI.
2. Navigate to **Workflows** -> **Add Workflow**.
3. Click the **...** (Options) menu in the top right -> **Import from File**.
4. Select the `workflow_n8n.json` file from this repository.
5. Save the workflow and toggle it to **Active**.
6. Double-click the **Webhook** trigger node and copy the **Test URL** (for debugging) or **Production URL** (for live runs).

### Step 4: Execute the Pipeline
Run the Python script, passing your target coordinates and the n8n webhook URL.

```bash
python ingest.py \
  --lat 40.7128 \
  --lon -74.0060 \
  --radius 5000 \
  --category "HVAC" \
  --webhook-url "https://n8n.yourdomain.com/webhook/local-biz-ingest"
```

---

## 4. Configuration Options

Behavior for both the Python extractor and the n8n workflow is managed via environment variables. Create a `.env` file in the root directory.

```env
# ==========================================
# PYTHON INGESTION CONFIGURATION
# ==========================================
# Target bounding box or radius (meters)
SEARCH_RADIUS=5000 
# Comma-separated list of business niches
TARGET_CATEGORIES=plumbing,hvac,roofing
# Your active n8n Webhook URL
N8N_WEBHOOK_URL=https://n8n.yourdomain.com/webhook/local-biz-ingest
# Optional: Proxy rotation string to avoid rate limits on geospatial APIs
PROXY_URL=http://user:pass@proxy.provider.com:8000

# ==========================================
# N8N QUALIFICATION THRESHOLDS (Referenced in Workflow)
# ==========================================
# Minimum acceptable review score to pass filtering (0.0 to 5.0)
MIN_REVIEW_SCORE=4.2
# Minimum number of reviews to establish statistical relevance
MIN_REVIEW_COUNT=15
# Require a website for the payload to proceed (true/false)
REQUIRE_WEBSITE=true
```

*Note: In n8n, you can reference the qualification thresholds dynamically using `$env.MIN_REVIEW_SCORE` inside your If/Switch nodes if your n8n instance is started with those variables.*

---

## 5. Production Tips & Maintenance

To ensure high throughput and reliability in a production environment, adhere to the following operational guidelines:

* **Rate Limiting & Throttling:** The Python ingestion script can generate thousands of POIs per minute. To prevent overwhelming your n8n instance or the destination database, implement batching in Python or utilize the `Split In Batches` node in n8n.
* **Error Handling:** The `workflow_n8n.json` includes an Error Trigger node. Route this to a Slack/Discord notification node to alert your DevOps team of enrichment failures or API timeouts.
* **Idempotency:** Local business data is highly duplicative. Ensure your destination data store (e.g., PostgreSQL) enforces a unique constraint on the `business_phone` or `website_domain` to prevent duplicate lead generation. Utilize n8n's `Postgres` node with an `UPSERT` operation.
* **Proxy Rotation:** If you are Data Ingestion domains for emails via the built-in HTTP nodes, aggressive polling will result in IP bans. Route the HTTP enrichment nodes through a proxy service by configuring the node's proxy settings.
* **Execution Data Pruning:** n8n stores execution data which can bloat the database during high-volume ingestion. Set `EXECUTIONS_DATA_PRUNE=true` and `EXECUTIONS_DATA_MAX_AGE=168` (7 days) in your n8n `.env` configuration.

---

## 6. License & Support Info

**License:** This project is licensed under the MIT License. You are free to use, modify, and distribute this software for commercial and non-commercial purposes without recurring fees.

**Support & Contributions:**
* For bug reports and feature requests, please open an issue on the GitHub repository.
* Pull requests for additional enrichment integrations (e.g., specific OSINT Data Extractors) are welcome. Ensure your code adheres to standard PEP-8 guidelines and that n8n workflow exports are stripped of sensitive credentials before committing.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Local Business Data Ingestion & Qualification Pipeline (n8n + Python) on Gumroad](https://mannyverse767.gumroad.com/l/npbugi)** *(Promo code: `EARLYBIRD` for -20%)*
