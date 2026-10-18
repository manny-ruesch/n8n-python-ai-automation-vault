# Customer Support Auto-Triaging & Categorization Engine — n8n Pipeline

![Customer Support Auto-Triaging & Categorization Engine — n8n Pipeline](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/n8n-support-ticket-auto-triaging-engine-20261002.png)

> **Production n8n pipeline for ticket classification, sentiment scoring, and squad routing via Claude and Gemini.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/uhmdqc)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Manual triage of support queues creates SLA bottlenecks, inconsistent severity tagging, and misrouted tickets across engineering and billing tiers. Human sorting averages 3 to 5 minutes per inbound ticket and introduces tagging drift during peak traffic windows.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Deterministic JSON Schema extraction for urgency (P1-P4), intent, sentiment, and language**
- ✅ **Dual LLM adapter support: Zero-code switching between Anthropic Claude 3.5 Sonnet and Google Gemini 1.5 Flash**
- ✅ **Universal helpdesk compatibility with native webhook payloads for Zendesk, Freshdesk, Intercom, and Email**
- ✅ **Dynamic squad routing with automated internal note drafting and Slack/Discord high-priority escalation**


---

## 📖 Technical Documentation & Setup Guide

# Customer Support Auto-Triaging & Categorization Engine (n8n Pipeline)

[![n8n Version](https://img.shields.io/badge/n8n-≥1.40.0-EA4B71?logo=n8n)](https://n8n.io)
[![Deliverable](https://img.shields.io/badge/deliverable-workflow__n8n.json-blue)](#)
[![LLM Support](https://img.shields.io/badge/LLM-Anthropic%20Claude%20%7C%20Google%20Gemini-orange)](#)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](#license--support-info)

An enterprise-grade, event-driven orchestration pipeline built for n8n. This engine ingests unstructured inbound customer tickets from multiple helpdesk sources, extracts strictly typed metadata using constrained LLM output schemas, routes work items dynamically to specialized teams, drafts synthetic context notes, and fires low-latency alerts for critical incidents.

---

## 1. Overview & Architecture

### The Problem
Manual queue triaging introduces severe operational bottlenecks:
* **High Latency Overhead:** Human ticket review averages 3 to 5 minutes per inbound message before routing.
* **Tagging Drift:** Subjective human scoring leads to inconsistent urgency classification (P1–P4) and category variance, especially during spike events.
* **Misrouting:** High rates of bounce-backs between customer-facing Tier 1 agents, FinOps/Billing, and Tier 3 Core Engineering.

### The Solution
This workflow acts as an asynchronous middleware layer between your ingress channels and downstream ticketing/alerting infrastructure. Inbound tickets are stripped of noise, validated, normalized, and classified by a model adapter (Anthropic Claude 3.5 Sonnet or Google Gemini 1.5 Flash) via a deterministic JSON schema. Downstream routers parse the structured output to patch helpdesk metadata, append private summarization notes, and broadcast P1/P2 tickets directly to real-time operations channels.

```
                              [ Inbound Payload ]
         (Zendesk / Freshdesk / Intercom / SES / Custom Webhook)
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │  Normalize & Sanitize Payload │
                       │    (PII Masking & Truncation) │
                       └───────────────┬───────────────┘
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │     LLM Adapter Selector      │
                       │   (Switch: Claude 3.5 /       │
                       │          Gemini 1.5 Flash)    │
                       └───────────────┬───────────────┘
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │   Structured JSON Extraction  │
                       │   Urgency, Intent, Sentiment, │
                       │   Language, Target Squad      │
                       └───────────────┬───────────────┘
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │    Conditional Router Core    │
                       └───────┬───────────────┬───────┘
                               │               │
            ┌──────────────────┘               └──────────────────┐
            ▼                                                     ▼
┌───────────────────────────────┐                 ┌───────────────────────────────┐
│     Helpdesk API Dispatch     │                 │   P1 / P2 Incident Escalation │
│  - Set Custom Fields / Tags   │                 │   - Slack Ops Alert (#p1-ops) │
│  - Set Target Squad Group     │                 │   - Discord Engineering Feed  │
│  - Inject Internal AI Summary │                 │   - Direct Pager Duty Webhook │
└───────────────────────────────┘                 └───────────────────────────────┘
```

---

## 2. Prerequisites & Environment Setup

### Infrastructure Requirements
* **n8n Instance:** Version `1.40.0` or higher (Self-hosted via Docker or n8n Cloud).
* **Execution Mode:** Redis-backed `queue` mode recommended for high-volume setups (`> 5,000` tickets/day).
* **Node.js Runtime:** `v18.x` or `v20.x` (if extending via custom JS functions).

### Access Credentials & APIs
* **LLM Credentials (at least one required):**
  * Anthropic API Key with access to `claude-3-5-sonnet-20241022`.
  * Google Cloud / AI Studio API Key with access to `gemini-1.5-flash-latest`.
* **Helpdesk Integrations (one or more):**
  * **Zendesk:** API Token + Agent Email + Subdomain.
  * **Freshdesk:** API Key + Domain.
  * **Intercom:** Personal Access Token / OAuth.
* **Notification Sinks:**
  * Slack Incoming Webhook or Bot Token (`chat:write` scope).
  * Discord Webhook URL.

---

## 3. Installation & Quickstart

### Step 1: Import Workflow File
1. Blueprint this repository or download the workflow artifact:
   ```bash
   git Blueprint https://github.com/your-org/n8n-support-triage-engine.git
   cd n8n-support-triage-engine
   ```
2. In the n8n web console, navigate to **Workflows** → **Import from File**.
3. Select `workflow_n8n.json`.

### Step 2: Establish Credentials
Within n8n, navigate to **Credentials** and configure the following:
* `Anthropic API` -> Supply your `sk-ant-...` key.
* `Google Gemini API` -> Supply your Gemini API key.
* `Header Auth` (or native nodes) for your target Helpdesk API.
* `Slack API` or generic `HTTP Request` for alerts.

### Step 3: Configure Ingress Webhook
1. Open the **Webhook: Ticket Ingress** node.
2. Select your HTTP Method (Default: `POST`).
3. Set the path (e.g., `ticket-triage-intake`).
4. Copy the **Production URL** to your helpdesk's webhook trigger settings (e.g., Zendesk Triggers, Freshdesk Automations).

### Step 4: Verification Dry Run
Dispatch a raw payload to the test webhook endpoint using `curl`:

```bash
curl -X POST https://your-n8n-domain.com/webhook-test/ticket-triage-intake \
  -H "Content-Type: application/json" \
  -d '{
    "ticket_id": "ZD-982311",
    "source": "zendesk",
    "customer_email": "ops-lead@enterprise.com",
    "subject": "CRITICAL: Database connection pool exhaustion after v2.4 deployment",
    "body": "We updated our cluster to 2.4 thirty minutes ago. All database connections are dropping with 500 errors across all user checkouts. We are losing transactions rapidly. Reverting is failing.",
    "requester_tier": "enterprise"
  }'
```

Verify that the workflow validates the payload, produces the structured extraction, updates the ticket, and delivers a P1 escalation payload to the configured Slack/Discord channel.

---

## 4. Configuration Options

### Environment Variables (`.env`)
Configure your execution environment by passing the following variables to your n8n container:

| Variable | Type | Allowed Values | Description |
| :--- | :--- | :--- | :--- |
| `TRIAGE_LLM_PROVIDER` | String | `anthropic`, `gemini` | Controls which LLM engine handles structural extraction. |
| `ANTHROPIC_API_KEY` | String | Secret | Anthropic API platform key. |
| `GEMINI_API_KEY` | String | Secret | Google AI Studio platform key. |
| `FALLBACK_TO_SECONDARY_LLM` | Boolean | `true`, `false` | If `true`, fails over to the alternate provider on 429/5xx responses. |
| `PII_MASKING_ENABLED` | Boolean | `true`, `false` | Strips credit cards, emails, and SSNs via regex prior to model inference. |
| `ALERT_DISCORD_WEBHOOK_URL`| String (URL) | Valid URL | Sink destination for Discord incident alerts. |
| `ALERT_SLACK_WEBHOOK_URL`  | String (URL) | Valid URL | Sink destination for Slack incident alerts. |
| `SLACK_P1_CHANNEL`         | String | `#name` or `C123...` | Target Slack channel ID for P1/P2 broadcast. |
| `MINIMUM_CONFIDENCE_THRESHOLD` | Float | `0.0` - `1.0` | Below this threshold, ticket defaults to manual review queue. |

### Deterministic Extraction Schema
The LLM node is enforced via structured output / tool calling. The core JSON schema enforced across both adapters is structured as follows:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "urgency": {
      "type": "string",
      "enum": ["P1", "P2", "P3", "P4"],
      "description": "P1=Total outage/revenue loss; P2=Major feature blocked; P3=Degraded performance/minor bug; P4=General inquiry/cosmetic."
    },
    "intent": {
      "type": "string",
      "enum": ["billing", "bug_report", "feature_request", "auth_access", "infrastructure", "legal_churn"],
      "description": "Core primary driver of the inbound ticket."
    },
    "sentiment": {
      "type": "string",
      "enum": ["positive", "neutral", "frustrated", "hostile_churn_risk"]
    },
    "language": {
      "type": "string",
      "description": "ISO 639-1 two-letter language code (e.g., en, es, de, ja)."
    },
    "squad_routing": {
      "type": "string",
      "enum": ["core_backend", "frontend_ui", "finops_billing", "secops", "support_general"]
    },
    "confidence_score": {
      "type": "number",
      "minimum": 0.0,
      "maximum": 1.0
    },
    "internal_summary": {
      "type": "string",
      "description": "A crisp, factual 2-sentence summary tailored for an L3 triage engineer."
    }
  },
  "required": [
    "urgency",
    "intent",
    "sentiment",
    "language",
    "squad_routing",
    "confidence_score",
    "internal_summary"
  ],
  "additionalProperties": false
}
```

---

## 5. Production Tips & Maintenance

### High-Volume Throughput Optimization
* **Worker Pools:** Run n8n in **Queue Mode** with a Redis instance handling task delegation. Split instances into `webhook` nodes and dedicated `worker` processes:
  ```env
  EXECUTIONS_MODE=queue
  QUEUE_BULL_REDIS_HOST=redis-master.internal
  ```
* **LLM Concurrency & Rate Limits:** Both Claude and Gemini impose tokens-per-minute (TPM) limits. Configure the n8n **SplitInBatches** node or implement a retry loop with exponential jitter inside the HTTP/LLM Request node to absorb webhook bursts during outages.

### Resilience & Failover Strategy
* **LLM Circuit Breaker:** The pipeline includes a conditional check evaluation on the extraction output. If Claude responds with a `429` (Rate Limited) or `503` (Overloaded), the error path invokes the Gemini 1.5 Flash adapter instantly, preserving the structured output schema contract.
* **Dead-Letter Handling:** When both LLM invocations fail or output unparseable JSON, the pipeline routes the execution to an `Unprocessed Dead-Letter Queue (DLQ)` branch, tagging the ticket as `triage-failed-manual-review` without rejecting the webhook.

### Data Privacy & Pre-Processing
* **PII Sanitization:** The pipeline contains a lightweight pre-processor Code Node before LLM invocation. Ensure your inputs strip PCI/PII data to prevent compliance violations under HIPAA/GDPR:
  ```javascript
  // Pre-execution regex reduction snippet:
  item.sanitized_body = item.body
    .replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b/gi, '[REDACTED_EMAIL]')
    .replace(/\b(?:\d{4}[ -]?){3}\d{4}\b/g, '[REDACTED_CARD]');
  return item;
  ```

### Observability & Drift Auditing
* **Prometheus Integration:** Expose n8n metrics (`N8N_METRICS=true`) to monitor execution duration and failure rates.
* **Tagging Drift Audits:** Export weekly triage outputs to a data lake (Postgres/BigQuery) and cross-reference LLM-predicted `urgency` against final human escalation levels to spot semantic drift or prompt edge cases.

---

## 6. License & Support Info

### License
This project is distributed under the **Apache License 2.0**. See the `LICENSE` file for full terms and conditions.

### Support & Maintenance
* **Issue Tracker:** Submit bug reports, schema adjustments, and integration requests via GitHub Issues.
* **Workflow Updates:** Check the releases page for updated definitions of `workflow_n8n.json` incorporating new model parameterizations or additional helpdesk payload signatures.
* **Enterprise Inquiries:** For enterprise deployment strategies, custom node integration, or SLA guarantees, open an issue labeled `enterprise-inquiry`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Customer Support Auto-Triaging & Categorization Engine — n8n Pipeline on Gumroad](https://mannyverse767.gumroad.com/l/uhmdqc)** *(Promo code: `EARLYBIRD` for -20%)*
