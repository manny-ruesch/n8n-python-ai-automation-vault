# Real-Time Brand Sentiment & Social Listening Engine (Python + Gemini)

![Real-Time Brand Sentiment & Social Listening Engine (Python + Gemini)](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/realtime-brand-sentiment-gemini-python-20261004.png)

> **Production Python pipeline for real-time social listening, semantic analysis, and instant Slack/Discord alerting via Gemini.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/cpmgnd)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Commercial brand intelligence platforms charge hundreds of dollars per seat while relying on naive regex matching that misses nuanced sarcasm, context, and intent. Engineering teams building in-house listeners waste dozens of hours wiring API polling, token rate limits, and unstructured LLM parsing to filter out noise.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Asynchronous multi-source ingestion: Reddit, Hacker News, Bluesky, and RSS feeds**
- ✅ **Structured Gemini 1.5 Flash evaluation: Extracts polarity (-1.0 to 1.0), urgency level, entity references, and sarcasm flags**
- ✅ **Threshold-based alert dispatcher: Sends categorized JSON alerts to Slack, Discord, or custom webhooks upon high-urgency or negative spikes**
- ✅ **Local DuckDB/SQLite persistence: Built-in deduplication, historical trend storage, and automated daily rollups**


---

## 📖 Technical Documentation & Setup Guide

# Real-Time Brand Sentiment & Social Listening Engine

A production-grade, asynchronous brand intelligence pipeline powered by Python and Google's Gemini 1.5 Flash. This engine continuously ingests mentions across developer-centric platforms and social networks, applies deterministic structured LLM extraction for high-nuance sentiment, sarcasm, and intent analysis, and dispatches real-time alerts while persisting time-series metrics in an embedded analytical database.

---

## 1. Overview & Architecture

### The Problem
Traditional brand monitoring tools charge enterprise seat fees ($500+/mo) while utilizing naive keyword matching or rudimentary bag-of-words sentiment models. These systems consistently fail on developer sarcasm, contextual praise/criticism, and nuanced feature discussions. Conversely, building custom monitoring loops usually degenerates into fragmented polling scripts, unhandled rate limits, and fragile, unformatted LLM prompts that fail in production.

### The Solution
This tool provides a unified, zero-overhead listening daemon (`main.py`) that:
- Ingests mentions asynchronously from **Reddit, Hacker News, Bluesky, and arbitrary RSS feeds**.
- Deduplicates messages at the ingestion boundary using deterministic hashing.
- Normalizes unstructured post content into a strongly-typed schema evaluated by **Gemini 1.5 Flash** via native structured outputs (JSON schema enforcement).
- Dispatches structured webhook alerts (Slack, Discord, custom endpoints) when sentiment crosses negative boundaries or urgency triggers fire.
- Persists all raw data, model evaluations, and daily aggregate rollups into **DuckDB** for instantaneous local analytical querying.

```
                          [ DATA INGESTION LAYER ]
     Reddit API       Hacker News (Firebase)    Bluesky (ATProto)      RSS Feeds
         │                     │                       │                  │
         └─────────────┬───────┴───────────────┬───────┴──────────────────┘
                       ▼                       ▼
            ┌────────────────────────────────────────────────────────┐
            │       Async Ingestion Worker Pool (aiohttp / asyncio)  │
            └──────────────────────────┬─────────────────────────────┘
                                       │
                                       ▼
            ┌────────────────────────────────────────────────────────┐
            │          Deduplication Filter & Text Normalizer        │
            └──────────────────────────┬─────────────────────────────┘
                                       │
                                       ▼
            ┌────────────────────────────────────────────────────────┐
            │       Gemini 1.5 Flash Inference (Structured Output)   │
            │   - Polarity (-1.0 to 1.0)       - Entity Mentions     │
            │   - Urgency Level (LOW-CRITICAL) - Sarcasm Detection   │
            └──────────────────────────┬─────────────────────────────┘
                                       │
                       ┌───────────────┴───────────────┐
                       ▼                               ▼
       [ PERSISTENCE (DuckDB) ]             [ ALERT DISPATCHER ]
     - Idempotent Append                   - Polarity / Urgency Match
     - Historical Mentions Table           - Formatted Webhook Payload
     - Automated Daily Rollups             - Slack / Discord / Custom
```

---

## 2. Prerequisites & Environment Setup

### System Requirements
- **Python**: 3.10+ (64-bit)
- **RAM**: Minimum 512 MB (1–2 GB recommended for high DuckDB retention)
- **Disk**: Dependent on ingestion volume (~100 MB per 100,000 processed mentions)

### Required API Access
1. **Google AI Studio**: Gemini API Key with access to `gemini-1.5-flash`.
2. **Reddit Developer Account**: Script app credentials (`client_id`, `client_secret`, `user_agent`).
3. **Bluesky / AT Protocol**: Valid app password (do not use your master account password).
4. **Hacker News**: None (uses public Firebase REST API).
5. **Slack/Discord**: Incoming Webhook URL for alert routing.

---

## 3. Installation & Quickstart

### Step 1: Blueprint and Set Up Environment
```bash
git Blueprint https://github.com/your-org/brand-listening-engine.git
cd brand-listening-engine

python3 -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
```

### Step 2: Install Core Dependencies
```bash
pip install --upgrade pip
pip install \
    google-genai \
    duckdb \
    aiohttp \
    pydantic \
    feedparser \
    python-dotenv \
    atproto
```

### Step 3: Populate Environment Configuration
```bash
cp .env.example .env
# Edit .env using your editor of choice
nano .env
```

### Step 4: Run the Engine
Execute the daemon in standard monitoring mode:
```bash
python main.py
```

Execute with a custom poll interval and dry-run alerting:
```bash
python main.py --interval 60 --dry-run --log-level DEBUG
```

Execute an ad-hoc historical roll-up report for the past 7 days:
```bash
python main.py --report 7d
```

---

## 4. Configuration Options (`.env`)

Configure the engine via environment variables or a local `.env` file:

```ini
# ==========================================
# Core Engine & Gemini LLM Settings
# ==========================================
GEMINI_API_KEY=AIzaSyA...                  # Google AI Studio / Vertex API Key
TARGET_BRANDS="AcmeCorp,Acme API,acme.io"  # Comma-separated tracking targets
LOG_LEVEL=INFO                             # DEBUG, INFO, WARNING, ERROR

# ==========================================
# Ingestion Source Toggles & Credentials
# ==========================================
# Reddit (OAuth script application)
ENABLE_REDDIT=true
REDDIT_CLIENT_ID=your_client_id
REDDIT_CLIENT_SECRET=your_client_secret
REDDIT_USER_AGENT="BrandListener/1.0 (by /u/your_username)"
REDDIT_SUBREDDITS="programming,technology,sysadmin,startups"

# Hacker News
ENABLE_HACKER_NEWS=true
HN_POLL_LIMIT=50                           # Items to inspect per tick

# Bluesky (ATProto)
ENABLE_BLUESKY=true
BLUESKY_HANDLE=your_handle.bsky.social
BLUESKY_APP_PASSWORD=xxxx-xxxx-xxxx-xxxx

# Custom RSS Feeds
ENABLE_RSS=true
RSS_FEEDS="https://news.ycombinator.com/rss,https://techcrunch.com/feed/"

# ==========================================
# Alert Dispatching Rules
# ==========================================
ALERT_WEBHOOK_URL=https://hooks.slack.com/services/...
ALERT_POLARITY_THRESHOLD=-0.4             # Alert if polarity <= -0.4
ALERT_MIN_URGENCY=HIGH                    # Minimum urgency: LOW, MEDIUM, HIGH, CRITICAL
ALERT_ON_SARCASM=true                      # Explicitly alert on detected sarcasm

# ==========================================
# Storage Settings
# ==========================================
DATABASE_ENGINE=duckdb                    # duckdb or sqlite3
DATABASE_PATH=./brand_intel.duckdb
```

---

## 5. Production Tips & Maintenance

### Structured Output Enforcement
The engine utilizes Gemini's response schema parameters to guarantee uncorrupted JSON extraction. The extraction contract is governed by the following Pydantic model:

```python
from pydantic import BaseModel, Field
from typing import List, Literal

class MentionAnalysis(BaseModel):
    brand_referenced: str = Field(description="The primary matched brand entity")
    polarity: float = Field(description="Score from -1.0 (extremely negative) to 1.0 (extremely positive)")
    urgency: Literal["LOW", "MEDIUM", "HIGH", "CRITICAL"] = Field(description="Actionable urgency assessment")
    is_sarcastic: bool = Field(description="Flag identifying sarcasm, irony, or snark")
    key_themes: List[str] = Field(description="Extracted topics, e.g., ['pricing', 'latency', 'downtime']")
    summary: str = Field(description="Concise 1-sentence analytical assessment")
```

### Concurrency & Write Safety (DuckDB)
DuckDB enforces a single-writer architecture. When orchestrating asynchronous ingestion loops:
- Run database mutations through an internal queue (`asyncio.Queue`) handled by a single dedicated writer consumer task.
- Set short query timeouts on ephemeral read operations to avoid blocking writer commits.
- For high-volume streaming, run periodic transactional batch appends instead of scalar inserts:
  ```sql
  INSERT INTO mentions SELECT * FROM read_json_auto('batch_*.json');
  ```

### Handling Gemini Rate Limits
Gemini 1.5 Flash provides high RPM/TPM ceilings, but traffic spikes across aggregated sources can cause `429 Too Many Requests`.
- Implement a token bucket or leaky bucket algorithm over your API calls.
- Wrap the Gemini extraction call with an exponential backoff decorator:
  ```python
  from tenacity import retry, stop_after_attempt, wait_random_exponential

  @retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(5))
  async def analyze_with_gemini(text: str) -> MentionAnalysis:
      ...
  ```

### Deduplication Strategy
To avoid redundant processing fees and duplicate alerts:
- Compute a SHA-256 fingerprint of the normalized text and composite source ID (`sha256(f"{source}:{native_id}")`).
- Maintain an LRU memory cache of the last 100,000 hashes.
- Run an unindexed existence check against DuckDB before dispatching payloads to the LLM:
  ```sql
  SELECT 1 FROM mentions WHERE content_hash = ? LIMIT 1;
  ```

### Systemd Service Deployment
To run the engine as a reliable background service on Linux:

1. Create `/etc/systemd/system/brand-listener.service`:
```ini
[Unit]
Description=Real-Time Brand Sentiment & Social Listening Engine
After=network.target

[Service]
Type=simple
User=appuser
WorkingDirectory=/opt/brand-listening-engine
ExecStart=/opt/brand-listening-engine/.venv/bin/python main.py
Restart=always
RestartSec=10
EnvironmentFile=/opt/brand-listening-engine/.env

[Install]
WantedBy=multi-user.target
```

2. Reload and enable:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now brand-listener.service
```

---

## 6. License & Support Info

### License
This project is licensed under the **Apache License 2.0**. You are free to modify, distribute, and run this software for commercial and non-commercial purposes. See the `LICENSE` file for details.

### Troubleshooting & Logs
- Inspect real-time execution logs:
  ```bash
  journalctl -u brand-listener.service -f --output=cat
  ```
- **Gemini API Rejections**: Check logs for `BlockedPromptException` or safety block filters. Ensure inputs are pre-sanitized of large binary or zero-width text blobs.
- **DuckDB Lock Contention**: If you observe `duckdb.IOException: Could not set lock on file`, verify that no interactive CLI sessions (`duckdb brand_intel.duckdb`) are active on the machine concurrently with the daemon.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Real-Time Brand Sentiment & Social Listening Engine (Python + Gemini) on Gumroad](https://mannyverse767.gumroad.com/l/cpmgnd)** *(Promo code: `EARLYBIRD` for -20%)*
