# Cross-Platform Inventory Sync Engine: Shopify, WooCommerce & Airtable (n8n Workflow)

![Cross-Platform Inventory Sync Engine: Shopify, WooCommerce & Airtable (n8n Workflow)](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/shopify-woocommerce-airtable-inventory-sync-n8n-20261003.png)

> **Production-ready n8n engine providing real-time, bidirectional inventory sync between Shopify, WooCommerce, and Airtable.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/sxgmfj)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Managing stock levels across disconnected platforms like Shopify and WooCommerce leads to overselling, stockouts, and manual reconciliation errors. Standard SaaS sync connectors charge recurring monthly fees per order or SKU, introducing latency and platform lock-in. This workflow resolves discrepancy by establishing Airtable as a single source of truth with bidirectional, sub-minute delta synchronization.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **n8n JSON Workflow** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Bidirectional stock sync triggering across Shopify, WooCommerce, and Airtable via webhooks**
- ✅ **Conflict resolution engine preventing infinite webhook loops and race conditions**
- ✅ **Batched delta updates minimizing API rate-limit consumption across all platforms**
- ✅ **Automated audit logging and Slack/Discord alerting for sync failures or negative stock states**


---

## 📖 Technical Documentation & Setup Guide

# Cross-Platform Inventory Sync Engine: Shopify, WooCommerce & Airtable

An enterprise-grade, event-driven inventory synchronization engine built on [n8n](https://n8n.io/). This workflow treats Airtable as a normalized Single Source of Truth (SSOT) to orchestrate bidirectional, sub-minute inventory delta updates across independent Shopify and WooCommerce storefronts.

By leveraging direct webhooks, transactional conflict resolution, and batch processing, this engine bypasses the high recurring costs, sync latency, and SKU limits associated with proprietary multi-channel SaaS middleware.

---

## 1. Overview & Architecture

### The Problem
Operating multi-channel e-commerce architectures across decoupled platforms like Shopify and WooCommerce introduces:
- **Inventory Drift & Overselling:** Asynchronous order fulfillment creates inventory lag, depleting stock across multiple storefronts before sync engines complete updates.
- **SaaS Extraction & Rate Limits:** Third-party aggregators impose per-order or per-SKU pricing tiers while running slow polling cycles (5–15 minutes) that trigger HTTP 429 (Rate Limit Exceeded) responses.
- **Webhook Echo Loops:** Naive bidirectional webhooks generate infinite event loops (Platform A triggers B, which triggers A).

### The Solution Architecture
This workflow uses an event-driven, decoupled ingest-normalize-reconcile architecture:

```
+-----------------------------------------------------------------------------------+
|                                 INGRESS LAYER                                     |
|  [Shopify Webhook]         [WooCommerce Webhook]             [Airtable Webhook]   |
|  inventory_levels/update   woocommerce_rest_insert_shop_order    Record Updated   |
+---------------------+--------------------+----------------------------+----------+
                      |                    |                            |
                      +--------------------+----------------------------+
                                           |
                                           v
+-----------------------------------------------------------------------------------+
|                         INGESTION & CONFLICT RESOLUTION                           |
| 1. Payload Normalization (Extract: SKU, Delta, Source, Timestamp, Event Hash)    |
| 2. Idempotency Gate (Redis / n8n Workflow Data deduplication cache)               |
| 3. Echo Suppression: Discard if Source == Internal Sync Worker                   |
+------------------------------------------+----------------------------------------+
                                           |
                                           v
+-----------------------------------------------------------------------------------+
|                            CORE ENGINE (AIRTABLE SSOT)                            |
| 1. Fetch current SSOT State by SKU (with optimistic locking check)                |
| 2. Compute absolute inventory: SSOT_Stock = SSOT_Stock + Delta                   |
| 3. Update Airtable Record (Store: Stock, Last_Source, Last_Synced_Timestamp)      |
+------------------------------------------+----------------------------------------+
                                           |
                                           v
+-----------------------------------------------------------------------------------+
|                         EGRESS LAYER (TARGET FAN-OUT)                             |
| 1. Filter out event origin (Avoid calling back to triggering platform)            |
| 2. Shopify Worker: GraphQL `inventorySetQuantities` (Location scoped)             |
| 3. WooCommerce Worker: REST API Batch Update (`/wp-json/wc/v3/products/batch`)   |
+------------------------------------------+----------------------------------------+
                                           |
                                           v
+-----------------------------------------------------------------------------------+
|                         AUDIT LOGGING & OBSERVABILITY                             |
| - Success -> Append metadata to `Sync_Logs` table                                 |
| - Anomaly (Negative Stock) -> Fire critical incident webhook to Slack / Discord   |
| - Failures -> Dead-Letter Queue (DLQ) + Pager Alert                               |
+-----------------------------------------------------------------------------------+
```

---

## 2. Prerequisites & Environment Setup

### 2.1 n8n Runtime Environment
* **n8n Version:** `>= 1.25.0`
* **Execution Mode:** Queue Mode (recommended for high-volume order spikes; requires Redis) or Regular Mode.
* **Network:** Must possess a public-facing domain secured via TLS (HTTPS) to receive inbound webhooks from Shopify, WooCommerce, and Airtable.

### 2.2 Platform Access Scopes & Credentials

#### Shopify (Custom App)
Create an app in **Shopify Admin > Settings > Apps and sales channels > Develop apps**:
* **Admin API Access Scopes:**
  * `read_inventory`, `write_inventory`
  * `read_products`, `write_products`
  * `read_orders`, `read_fulfillments`
* Record:
  * Admin API Access Token
  * Store Domain (`your-store.myshopify.com`)
  * Target `Location ID` (retrieve via `GET /admin/api/2024-01/locations.json`)

#### WooCommerce
Generate keys in **WooCommerce > Settings > Advanced > REST API**:
* **Permissions:** Read/Write
* Record:
  * Consumer Key (`ck_...`)
  * Consumer Secret (`cs_...`)
  * Base URL (`https://your-woo-domain.com`)

#### Airtable Base Setup
Create a dedicated base with the following exact table structure:

##### Table 1: `Inventory` (Primary SSOT)
| Field Name | Type | Notes |
| :--- | :--- | :--- |
| `SKU` | Single line text | **Primary Field**, unique identifier across all catalogs. |
| `Stock_Level` | Number | Integer, allows negative values (for deficit tracking). |
| `Shopify_Inventory_Item_ID` | Single line text | The inventory item ID within Shopify. |
| `Woo_Product_ID` | Number | Integer, WooCommerce Product or Variation ID. |
| `Last_Sync_Source` | Single line text | Stores: `SHOPIFY`, `WOOCOMMERCE`, `AIRTABLE_MANUAL`. |
| `Last_Sync_Timestamp` | Date | ISO 8601 with time tracking (millisecond precision). |
| `Lock_Version` | Number | Incremental integer for optimistic locking. |

##### Table 2: `Sync_Logs`
| Field Name | Type | Notes |
| :--- | :--- | :--- |
| `Log_ID` | Autonumber | Primary Field |
| `SKU` | Single line text | Target SKU |
| `Delta` | Number | Stock adjustment value (+/-) |
| `Source` | Single line text | Ingress source |
| `Status` | Single select | `SUCCESS`, `FAILED`, `RATE_LIMITED`, `ANOMALY` |
| `Error_Payload` | Long text | Raw JSON of caught exceptions |
| `Timestamp` | Created time | System timestamp |

---

## 3. Installation & Quickstart

### Step 1: Import Workflow File into n8n
1. Blueprint this repository or copy the deliverable:
   ```bash
   git Blueprint https://github.com/your-org/cross-platform-inventory-sync.git
   cd cross-platform-inventory-sync
   ```
2. Open your n8n Dashboard.
3. Go to **Workflows > Import from File** and select `workflow_n8n.json`.

### Step 2: Configure Credentials in n8n
In the n8n UI, navigate to **Credentials** and create:
1. **Airtable Personal Access Token (PAT):**
   * Required Scopes: `data.records:read`, `data.records:write`, `schema.bases:read`
2. **WooCommerce API:**
   * URL, Consumer Key, Consumer Secret.
3. **Shopify API:**
   * Access Token, Domain.
4. **Slack/Discord Webhook (Alerting):**
   * Incoming Webhook URL.

Link each credential to the corresponding nodes within the imported workflow canvas.

### Step 3: Configure Ingress Webhooks

#### 1. Shopify
Execute a `POST` request or configure via **Shopify Admin > Settings > Notifications > Webhooks**:
* **Event:** `Inventory level update` (`inventory_levels/update`)
* **Format:** `JSON`
* **URL:** `https://your-n8n-instance.com/webhook/sync/shopify/inventory`

#### 2. WooCommerce
Navigate to **WooCommerce > Settings > Advanced > Webhooks**:
* **Topic:** `Order created` (`woocommerce_rest_insert_shop_order`) and `Product updated` (`product.updated`)
* **Delivery URL:** `https://your-n8n-instance.com/webhook/sync/woocommerce/inventory`
* **Secret:** Your configured shared secret

#### 3. Airtable
Set up an Airtable Automation on the `Inventory` table:
* **Trigger:** When record updated (watch `Stock_Level` field only).
* **Action:** Run Script / Send Webhook targeting:
  `https://your-n8n-instance.com/webhook/sync/airtable/inventory`

### Step 4: Verify Deployment
1. Set the n8n workflow status to **Active**.
2. Manually adjust the `Stock_Level` of a test SKU in Airtable.
3. Inspect the n8n Execution History to verify:
   * Ingress normalization
   * Echo filter pass
   * Egress execution on Shopify and WooCommerce endpoints
   * Audit log insertion into Airtable `Sync_Logs`

---

## 4. Configuration Options

Store these variables in your n8n `.env` file or export them inside your Docker deployment environment.

| Variable Name | Required | Default | Description |
| :--- | :--- | :--- | :--- |
| `AIRTABLE_BASE_ID` | Yes | - | Base ID (found in Airtable API documentation). |
| `SHOPIFY_LOCATION_ID` | Yes | - | Target Inventory Location GID for Shopify fulfillment. |
| `ALERT_WEBHOOK_URL` | Yes | - | Slack/Discord incoming webhook endpoint. |
| `BATCH_INTERVAL_MS` | No | `5000` | Egress buffer window to bundle delta updates (reduces API load). |
| `MAX_RETRY_ATTEMPTS` | No | `3` | Max HTTP retry count before shifting payload to the DLQ. |
| `RETRY_BACKOFF_BASE_MS`| No | `1000` | Exponential backoff scalar for handling downstream 429/5xx errors. |
| `NEGATIVE_STOCK_ALERT` | No | `true` | When `true`, fires high-priority alerts on negative stock states. |
| `DRY_RUN_MODE` | No | `false` | If `true`, runs workflow logic without mutating external APIs. |

---

## 5. Production Tips & Maintenance

### 5.1 Concurrency & Race Condition Control
* **Optimistic Locking:** The core engine evaluates the `Lock_Version` on the Airtable record prior to writing mutations. If the version increments between read and write operations, the transaction aborts and retries with exponential jitter.
* **Echo Loops Suppression:** The engine uses an internal tracking header (`Last_Sync_Source`). When an update is deployed to Shopify, the subsequent Shopify webhook checks whether the update's origin was the sync engine itself. If `payload.source == ENGINE_HASH`, the event is dropped immediately.

### 5.2 API Throttling & Rate Limits
* **Shopify:** Utilizes the GraphQL `inventorySetQuantities` mutation, running on a 40-cost-per-second leaky bucket algorithm. Do not exceed 20 concurrent workers against a standard Shopify tier.
* **WooCommerce:** Uses the batch updating endpoint (`PUT /wp-json/wc/v3/products/batch`), which caps requests at 100 items per payload. The workflow aggregates incoming mutations via an n8n `Wait` / `Aggregate` node pattern under high-traffic intervals.

### 5.3 Error Handling & Dead Letter Queues (DLQ)
* All uncaught execution errors branch directly into the **Error Handling Sub-Workflow**.
* Failed payloads append directly to `Sync_Logs` with a `FAILED` status, capturing full JSON context (`$json.error.stack`).
* When `Stock_Level < 0`, the anomaly detection node fires a rich Markdown alert to your DevOps Slack/Discord channel:
  ```json
  {
    "level": "CRITICAL",
    "event": "INVENTORY_DEFICIT",
    "sku": "PROD-A12",
    "calculated_level": -2,
    "action_required": "Manual stock reconciliation needed."
  }
  ```

### 5.4 Scale & Queue Optimization
For transaction volumes exceeding 5,000 orders/day:
* Switch n8n from default SQLite execution storage to **PostgreSQL**.
* Run n8n in **Scaling / Queue Mode** backed by Redis:
  ```bash
  EXECUTIONS_MODE=queue
  QUEUE_BULL_REDIS_HOST=redis
  QUEUE_BULL_REDIS_PORT=6379
  ```
* Implement automated pruning of execution logs in n8n configuration:
  ```bash
  EXECUTIONS_DATA_PRUNE=true
  EXECUTIONS_DATA_MAX_AGE=168 # 7 days
  ```

---

## 6. License & Support

### License
Distributed under the **MIT License**. See `LICENSE` for details.

### Support & Issue Tracking
* **Issues:** Report bugs or edge cases via GitHub Issues. Include the execution JSON output (with API keys redacted).
* **Enterprise Customization:** For bespoke ERP integrations (NetSuite, SAP, Microsoft Dynamics 365) or custom middleware rules, submit an inquiry through your assigned engineering support contact.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Cross-Platform Inventory Sync Engine: Shopify, WooCommerce & Airtable (n8n Workflow) on Gumroad](https://mannyverse767.gumroad.com/l/sxgmfj)** *(Promo code: `EARLYBIRD` for -20%)*
