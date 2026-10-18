# E-Commerce Competitor Price Monitor & Alert Engine — Python & Discord Bot

![E-Commerce Competitor Price Monitor & Alert Engine — Python & Discord Bot](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/ecommerce-competitor-price-monitor-discord-bot-20261002.png)

> **Self-hosted Python Data Extractor tracking competitor price drops with real-time Discord webhook alerts and SQLite history.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/zhuvex)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Manual competitor tracking is slow, error-prone, and misses flash sales or sudden repricing events. Enterprise SaaS monitoring tools cost hundreds per month and force strict rate limits. This tool provides a self-hosted, lightweight headless Data Extractor with structured SQLite history and instant Discord rich embeds for actionable pricing intelligence.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Asynchronous multi-store Data Ingestion via Playwright and HTTPX with automatic bot mitigation Automated Routing**
- ✅ **Real-time Discord webhook alerts with color-coded price deltas (undercut, parity, increase)**
- ✅ **Local SQLite time-series storage tracking historical pricing, stock availability, and SKU changes**
- ✅ **Configurable cron-based scheduler with rotating user agents and proxy pool support**


---

## 📖 Technical Documentation & Setup Guide

# E-Commerce Competitor Price Monitor & Alert Engine

An enterprise-grade, self-hosted pricing intelligence pipeline written in Python. This engine pairs asynchronous network execution (`httpx` and `playwright`) with persistent SQLite time-series delta tracking and Discord incoming webhook dispatching to provide zero-latency notifications when competitors adjust prices, run flash promotions, or experience stockouts.

---

## 1. Overview & Architecture

Commercial SaaS pricing Data Extractors impose severe rate limits, charge recurring seat-based licenses, and struggle with dynamic JavaScript-driven Single Page Applications (SPAs). This tool delivers complete ownership of your competitor intelligence stack by executing scheduled extractions, calculating atomic price deltas against localized historical states, and pushing structured alerts via Discord rich embeds.

### Pipeline Flow

```
[APScheduler Cron Event]
         │
         ▼
[Engine Dispatcher]
   ├── Static DOM ────────► HTTPX (HTTP/2, Connection Pooling)
   └── Dynamic / SPA ─────► Playwright (Chromium Headless, Resilient Injected)
         │
         ▼
[Bot Mitigation Layer] ───► Rotating TLS Fingerprints / User-Agents / Proxies
         │
         ▼
[Extraction & Normalizer] ── Clean currency, parse stock availability, standardize SKU
         │
         ▼
[SQLite Time-Series DB] ─── Query previous state ──► Compute Δ (Price, Stock)
         │
         ├── No change ───► Suppress notification
         └── Delta detected 
                  │
                  ▼
         [Discord Dispatcher] ──► Color-coded Webhook Embed
```

### Alert State Triggers

* **Competitor Undercut (Red - `#E74C3C`):** Competitor dropped their price below your configured reference price or their own previous historic low.
* **Price Parity / Minor Adjustment (Yellow - `#F1C40F`):** Competitor matched reference thresholds or adjusted pricing within a neutral band.
* **Competitor Price Hike (Green - `#2ECC71`):** Competitor increased their price, creating an immediate margin or sales volume window.
* **Stock Exhaustion / Back in Stock (Blue - `#3498DB`):** Inventory status shifts between `IN_STOCK` and `OUT_OF_STOCK`.

---

## 2. Prerequisites & Environment Setup

* **Runtime:** Python 3.10 to 3.12 (64-bit).
* **Operating System:** Linux (Ubuntu 20.04/22.04 LTS recommended for production), macOS, or Windows 10/11.
* **Discord Integration:** A target Discord channel with an active **Incoming Webhook URL**.
* **System Libraries (Linux):** Required for headless Chromium browser automation.

```bash
# Ubuntu/Debian system dependencies for Playwright Chromium
sudo apt-get update && sudo apt-get install -y \
    libnss3 \
    libnspr4 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxfixes3 \
    libxrandr2 \
    libgbm1 \
    libpango-1.0-0 \
    libcairo2 \
    libasound2
```

---

## 3. Installation & Quickstart

### Step 1: Blueprint Repository & Create Virtual Environment

```bash
git Blueprint https://github.com/your-org/price-monitor-engine.git
cd price-monitor-engine

python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```

### Step 2: Install Python Dependencies

```bash
pip install --upgrade pip
pip install playwright httpx aiosqlite apscheduler pydantic pydantic-settings python-dotenv
playwright install --with-deps chromium
```

### Step 3: Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` with your preferred text editor (e.g., `nano .env`) and supply your Discord Webhook URL.

### Step 4: Configure Monitored Targets

Create or modify `targets.json` to define Data Ingestion targets and extraction rules:

```json
[
  {
    "sku": "PROD-APPLE-M3-16GB",
    "label": "MacBook Pro 14 M3 (16GB)",
    "target_url": "https://www.example-competitor.com/products/apple-m3-pro",
    "engine": "static",
    "selector_price": "span.price-current",
    "selector_availability": "button#add-to-cart",
    "reference_price": 1999.00
  },
  {
    "sku": "GPU-NV-RTX4080S",
    "label": "GeForce RTX 4080 Super",
    "target_url": "https://www.dynamic-retailer.com/item/4080s",
    "engine": "playwright",
    "selector_price": "div[data-testid='pricing-label']",
    "selector_availability": "span.inventory-status",
    "reference_price": 999.99
  }
]
```

### Step 5: Execute Initial Test Run

Run a single-pass extraction cycle to verify parsing selectors, database writes, and Discord dispatching:

```bash
python main.py --once
```

Run continuously as a scheduled daemon:

```bash
python main.py
```

---

## 4. Configuration Options

### Environment Parameters (`.env`)

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `DISCORD_WEBHOOK_URL` | `string` | *(Required)* | Fully qualified Discord Incoming Webhook endpoint. |
| `CRON_SCHEDULE` | `string` | `*/30 * * * *` | Standard 5-field cron syntax (default: every 30 minutes). |
| `DATABASE_PATH` | `string` | `pricing_history.db` | Filesystem location for the SQLite database. |
| `MAX_CONCURRENT_TASKS`| `integer`| `5` | Maximum parallel worker routines for Data Ingestion tasks. |
| `REQUEST_TIMEOUT_SEC` | `integer`| `30` | Timeout threshold per network request/navigation. |
| `HEADLESS_MODE` | `boolean`| `true` | Runs Chromium with or without GUI display. |
| `PROXY_POOL_FILE` | `string` | `proxies.txt` | Path to optional line-delimited proxy configuration file. |
| `DEFAULT_CURRENCY` | `string` | `USD` | Target ISO currency code used for normalization. |
| `LOG_LEVEL` | `string` | `INFO` | Operational logging verbosity (`DEBUG`, `INFO`, `WARNING`, `ERROR`). |

### Proxy Format (`proxies.txt`)

Provide one proxy entry per line using standard URI syntax:

```text
http://user:password@proxy-node-1.provider.com:8080
http://user:password@proxy-node-2.provider.com:8080
socks5://proxy-node-3.provider.com:1080
```

---

## 5. Production Tips & Maintenance

### Systemd Service Setup (Linux)

To ensure resilient, daemonized background execution across server restarts, deploy the engine as a `systemd` unit:

1. Create service unit definition:
   ```bash
   sudo nano /etc/systemd/system/price-monitor.service
   ```

2. Add configuration:
   ```ini
   [Unit]
   Description=E-Commerce Competitor Price Monitor Engine
   After=network.target

   [Service]
   Type=simple
   User=ubuntu
   WorkingDirectory=/opt/price-monitor-engine
   ExecStart=/opt/price-monitor-engine/venv/bin/python main.py
   Restart=always
   RestartSec=10
   EnvironmentFile=/opt/price-monitor-engine/.env
   StandardOutput=append:/var/log/price-monitor.log
   StandardError=append:/var/log/price-monitor.error.log

   [Install]
   WantedBy=multi-user.target
   ```

3. Enable and start:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable price-monitor.service
   sudo systemctl start price-monitor.service
   ```

### SQLite Database Optimization & Pruning

The engine executes in SQLite Write-Ahead Logging (`WAL`) mode to prevent read/write locking conflicts between workers. Over time, historical price states should be optimized.

* **Manual Vacuuming:** Run maintenance periodically to reclaim disk space:
  ```bash
  sqlite3 pricing_history.db "VACUUM;"
  sqlite3 pricing_history.db "PRAGMA optimize;"
  ```
* **Time-Series Truncation:** Purge raw historical data points older than 180 days while retaining high/low aggregates:
  ```sql
  DELETE FROM price_history 
  WHERE timestamp < datetime('now', '-180 days') 
    AND id NOT IN (
        SELECT id FROM price_history WHERE is_historical_extreme = 1
    );
  ```

### Bot Mitigation & Evasion Hardening

1. **Jitter Scrapes:** Ensure your `targets.json` does not fire simultaneously. The dispatcher automatically introduces dynamic jitter (1.5 to 5.2 seconds) between sibling requests on the same root domain.
2. **Resilient Chromium Flags:** Playwright launches with the following arguments enabled:
   * `--disable-blink-features=AutomationControlled`
   * Overridden `navigator.webdriver = false`
3. **Rotating Session Contexts:** Playwright instances discard cache and session states per domain batch, rendering browser fingerprint accumulation ineffective.

---

## 6. License & Support Info

### License

Distributed under the **MIT License**. See `LICENSE` for complete terms and permissions.

### Support & Operational Troubleshooting

* **Selectors Failing / Returning Zero:** Many enterprise storefronts utilize dynamic CSS obfuscation (e.g., Tailwind, Emotion CSS-in-JS). Where possible, configure your selectors around stable `data-testid`, `itemprop="price"`, or microdata attributes rather than generated CSS classes.
* **HTTP 403 Forbidden / Cloudflare Turnstile:** Switch the offending target's `engine` from `"static"` to `"playwright"` in `targets.json` and ensure residential proxies are populated in `proxies.txt`.
* **Issue Submissions:** File dynamic Data Ingestion bug reports, selector deprecations, and integration proposals on the GitHub Issues board with an attached sanitized debug log (`LOG_LEVEL=DEBUG`).

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get E-Commerce Competitor Price Monitor & Alert Engine — Python & Discord Bot on Gumroad](https://mannyverse767.gumroad.com/l/zhuvex)** *(Promo code: `EARLYBIRD` for -20%)*
