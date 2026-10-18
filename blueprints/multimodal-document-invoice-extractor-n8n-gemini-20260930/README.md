# Multi-Modal Document & Invoice Intelligence Extractor — n8n Workflow

![Multi-Modal Document & Invoice Intelligence Extractor — n8n Workflow](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/multimodal-document-invoice-extractor-n8n-gemini-20260930.png)

> **Production-ready n8n workflow extracting line-item JSON from invoices and receipts using Gemini Vision.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/elzaeh)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Manual invoice and receipt processing is error-prone and brittle when handled via conventional OCR regex pipelines. Most existing automated solutions fail on complex multi-page tables, rotated mobile scans, and multi-currency line items, requiring expensive proprietary SaaS subscriptions with usage-based markups.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Native multi-modal parsing via Google Gemini 1.5/2.0 Flash with zero external OCR dependencies**
- ✅ **Deterministic JSON schema enforcement for line items, tax breakdowns, vendor metadata, and IBANs**
- ✅ **Automated handling for multi-page PDFs, mobile photos (HEIC/JPG/PNG), and distorted receipts**
- ✅ **Self-contained webhook-ready n8n architecture with Google Sheets, PostgreSQL, and ERP-ready output nodes**


---

## 📖 Technical Documentation & Setup Guide

# Multi-Modal Document & Invoice Intelligence Extractor (n8n Workflow)

[![n8n Version](https://img.shields.io/badge/n8n-≥1.40.0-EA4B71?style=flat-square&logo=n8n)](https://n8n.io/)
[![Model](https://img.shields.io/badge/Model-Gemini%201.5%20%2F%202.0%20Flash-4285F4?style=flat-square&logo=google)](https://ai.google.dev/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

An enterprise-grade, deterministic document processing pipeline built inside n8n. This workflow ingests complex visual financial records—including multi-page PDF invoices, skewed smartphone receipts, and multi-currency billing statements—and extracts structured data directly into relational databases, spreadsheets, and downstream ERPs with zero traditional OCR dependencies.

---

## 1. Overview & Architecture

### The Problem
Traditional document extraction relies on optical character recognition (OCR) layered with heuristic spatial mapping and regular expressions (Regex). This approach introduces structural brittleness:
- Non-standard table layouts split line items incorrectly.
- Low-dpi mobile scans, folded receipts, and skewed perspectives drop character recognition accuracy.
- Multi-currency invoices with heterogeneous tax models (e.g., composite VAT/GST) break brittle regex parsers.
- Enterprise SaaS alternatives (e.g., AWS Textract, Google Document AI) introduce opaque recurring consumption costs and steep per-page markups.

### The Solution
This workflow replaces layered OCR/heuristic systems with direct multi-modal LLM document understanding using **Google Gemini 1.5/2.0 Flash**. Gemini processes high-resolution raw document bytes natively, parsing structural spatial relationships, arbitrary tabular layouts, and multi-lingual typography in a single inference pass while enforcing a strict JSON Schema output.

```
                  INCOMING INVOICE / RECEIPT
          [Webhook / S3 / Drive / REST API Payload]
                            │
                            ▼
              ┌───────────────────────────┐
              │ 01. Binary Normalization   │
              │  - MIME Detection         │
              │  - Base64 Encoding        │
              └─────────────┬─────────────┘
                            │
                            ▼
              ┌───────────────────────────┐
              │ 02. Gemini Flash Vision    │
              │  - Zero-OCR Parsing       │
              │  - Deterministic Schema   │
              │  - In-Context Formatting  │
              └─────────────┬─────────────┘
                            │
                            ▼
              ┌───────────────────────────┐
              │ 03. Validation & Parsing  │
              │  - Schema Integrity Check │
              │  - Currency & Date Normal.│
              │  - Tax Reconciliation     │
              └─────────────┬─────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
┌─────────────────┐ ┌───────────────┐ ┌────────────────┐
│  PostgreSQL     │ │ Google Sheets │ │ ERP Webhook    │
│  (Relational    │ │ (Audit / Ops) │ │ (NetSuite /    │
│   Ledger)       │ │               │ │  SAP / Odoo)   │
└─────────────────┘ └───────────────┘ └────────────────┘
```

### Data Pipeline Stages
1. **Binary Ingestion & Normalization**: The workflow accepts `application/pdf`, `image/heic`, `image/png`, and `image/jpeg`. Files are buffered, validated against size thresholds, and converted to base64 inline without disk spooling.
2. **Deterministic Multi-Modal Inference**: Payloads are routed to the Gemini Flash endpoint with dynamic system prompts and a JSON-schema constrained response parameter (`response_mime_type: "application/json"`).
3. **Data Normalization & Sanitization**: Downstream code sub-nodes validate cross-calculations (line-item subtotal vs. documented grand total), enforce ISO-8601 timestamps, and cast ISO-4217 currency identifiers.
4. **Multi-Target Dispatch**: The processed output splits concurrently to an operational Google Sheet, an ACID-compliant PostgreSQL schema, and an outbound ERP-ready webhook payload.

---

## 2. Prerequisites & Environment Setup

### Required Components
- **n8n**: Version `1.40.0` or higher (Self-hosted via Docker or n8n Cloud).
- **Google Cloud Platform / Google AI Studio API Key**: Access to `gemini-1.5-flash-latest` or `gemini-2.0-flash`.
- **PostgreSQL**: Version 14+ instance (self-hosted, Neon, Supabase, or AWS RDS).
- **Google Cloud Console Service Account** (Optional): With Google Sheets API permissions if utilizing the Sheets dispatch node.

### System Specifications
- **CPU**: Minimum 2 cores (for concurrent binary payload translation).
- **RAM**: Minimum 4GB (8GB recommended if processing high-resolution PDFs > 20 MB).
- **Disk**: Dependent on n8n operational logging settings. Ensure `EXECUTIONS_DATA_PRUNE=true` to prevent disk bloat from binary execution data.

---

## 3. Installation & Quickstart

### Step 1: Blueprint Repository & Import Workflow
Download the workflow artifact from this repository:
```bash
git Blueprint https://github.com/your-org/n8n-invoice-intelligence.git
cd n8n-invoice-intelligence
```

Import `workflow_n8n.json` into n8n:
1. Open your n8n Dashboard.
2. Click **Workflows** -> **Add Workflow** -> **Import from File...**
3. Select `workflow_n8n.json`.

### Step 2: Configure Credentials in n8n UI
Navigate to **Credentials** in the n8n UI and register the following:

1. **Google Gemini API**:
   - Type: `Header Auth` (or custom Gemini node credential depending on n8n version).
   - Key: `x-goog-api-key`
   - Value: `<YOUR_GEMINI_API_KEY>`
2. **PostgreSQL**:
   - Provide host, database, user, password, and SSL mode settings.
3. **Google Sheets OAuth2 / Service Account**:
   - Authenticate with your designated Google Workspace service account.

### Step 3: Initialize Database Schema
Run the following DDL query inside your target PostgreSQL database:

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE invoices (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    invoice_number VARCHAR(128) NOT NULL,
    invoice_date DATE,
    due_date DATE,
    currency VARCHAR(3) DEFAULT 'USD',
    vendor_name VARCHAR(255),
    vendor_tax_id VARCHAR(64),
    vendor_iban VARCHAR(64),
    vendor_swift_bic VARCHAR(32),
    vendor_address TEXT,
    customer_name VARCHAR(255),
    customer_tax_id VARCHAR(64),
    subtotal NUMERIC(12, 4),
    tax_amount NUMERIC(12, 4),
    total_amount NUMERIC(12, 4),
    raw_response JSONB,
    processed_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE invoice_line_items (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    invoice_id UUID REFERENCES invoices(id) ON DELETE CASCADE,
    description TEXT,
    quantity NUMERIC(10, 4),
    unit_price NUMERIC(12, 4),
    line_total NUMERIC(12, 4),
    tax_rate NUMERIC(5, 2)
);
```

### Step 4: Test via Webhook
Activate the workflow in n8n and fire a test document using `curl`:

```bash
curl -X POST "https://your-n8n-instance.com/webhook/invoice-extract" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@/path/to/invoice_sample.pdf"
```

---

## 4. Configuration Options

### Environment Variables (.env)
Add or tune these variables within your n8n execution environment:

```env
# n8n General Execution Configurations
N8N_PAYLOAD_SIZE_MAX=64
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168

# API Access
GEMINI_API_KEY=AIzaSy...
GEMINI_MODEL=gemini-1.5-flash

# Processing Safeguards
AUTO_RECONCILE_TOTALS=true
STRICT_SCHEMA_ENFORCEMENT=true
DEFAULT_CURRENCY=USD
```

### Extraction Schema Reference
The Gemini inference engine is locked to this output signature via strict JSON Schema mode:

```json
{
  "vendor": {
    "name": "string",
    "tax_id": "string",
    "address": "string",
    "iban": "string",
    "swift_bic": "string"
  },
  "customer": {
    "name": "string",
    "tax_id": "string",
    "address": "string"
  },
  "metadata": {
    "invoice_number": "string",
    "issue_date": "YYYY-MM-DD",
    "due_date": "YYYY-MM-DD",
    "currency": "ISO-4217"
  },
  "financials": {
    "subtotal": 0.0,
    "tax_amount": 0.0,
    "total_amount": 0.0,
    "discounts": 0.0
  },
  "line_items": [
    {
      "description": "string",
      "quantity": 0.0,
      "unit_price": 0.0,
      "line_total": 0.0,
      "tax_rate_percent": 0.0
    }
  ]
}
```

---

## 5. Production Tips & Maintenance

### High-Volume PDF Chunking & Input Limits
- **Native Context Window**: Gemini 1.5/2.0 Flash easily accommodates multi-page invoices up to 100+ pages. However, payload limits within reverse proxies (e.g., NGINX, Cloudflare) often reject large multipart payloads. Configure your reverse proxy to allow client payloads up to `64MB`.
- **Split Invoices**: If your incoming files combine multiple unrelated invoices into one monolithic scan, prepend a PDF-splitting sub-workflow via `pdf-lib` or a headless Python worker before ingestion.

### Rate Limiting & Transient Error Backoff
The workflow includes an n8n **Wait Node** set to exponential backoff when an HTTP `429` (Resource Exhausted) or `503` (Model Overloaded) status returns from Google AI Studio. 

Configure internal retries directly inside the Gemini HTTP Request node:
- **Max Tries**: `4`
- **Wait Between Tries**: `2500 ms`
- **Retry on Failures**: `429, 5xx`

### Data Retention & Privacy Boundaries
By using standard Google Cloud Vertex AI or Google AI Studio paid tier endpoints, submitted customer payloads are **not used for model training**. 
- To maintain enterprise regulatory compliance (GDPR/SOC2/HIPAA), run n8n inside an isolated VPC, establish direct peering to GCP via Vertex AI endpoints, and activate disk scrubbing on the n8n binary filesystem (`N8N_DEFAULT_BINARY_DATA_MODE=filesystem`).

### Observability & Dead-Letter Queue (DLQ)
This workflow defines an explicit `Error Trigger` node. Any unhandled inference failure, validation mismatch, or database connection drop triggers a routing branch to an error queue (`DLQ`) table in PostgreSQL, preventing silent workflow interruptions and simplifying audit procedures:

```sql
CREATE TABLE extraction_failures (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    error_message TEXT,
    failed_payload_base64 TEXT,
    received_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

---

## 6. License & Support

### License
Distributed under the **MIT License**. See `LICENSE` for structural details.

### Maintenance & Support
- **Issue Tracking**: Submit technical bug reports, edge-case invoice samples, or feature enhancement requests directly via GitHub Issues.
- **Enterprise Implementations**: For custom SAP/NetSuite connector tailoring, SLA-backed integration design, or high-throughput batch optimizations, contact our solutions engineering team at `support@yourdomain.com`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Multi-Modal Document & Invoice Intelligence Extractor — n8n Workflow on Gumroad](https://mannyverse767.gumroad.com/l/elzaeh)** *(Promo code: `EARLYBIRD` for -20%)*
