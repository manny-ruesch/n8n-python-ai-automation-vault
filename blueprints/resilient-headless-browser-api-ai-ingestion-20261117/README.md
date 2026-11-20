# Resilient Headless Browser API for AI Web Ingestion

![Resilient Headless Browser API for AI Web Ingestion](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/resilient-headless-browser-api-ai-ingestion-20261117.png)

> **Self-hosted headless browser API for reliable AI web ingestion and structured data extraction.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/smeyrf)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Standard HTTP requests fail on modern web applications protected by enterprise firewalls and JavaScript challenges. AI agents require a scalable, resilient headless browser pipeline to ingest real-time data without triggering rate limits or blocks.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Containerized FastAPI infrastructure with Playwright and fingerprint anonymization**
- ✅ **Dynamic proxy rotation and automated browser footprint randomization**
- ✅ **Seamless integration with LLM agents via standard REST endpoints**
- ✅ **Zero-maintenance DOM parsing and structured JSON data extraction**


---

## 📖 Technical Documentation & Setup Guide

# Resilient Headless Browser API for AI Web Ingestion

![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-00a393.svg)
![Playwright](https://img.shields.io/badge/Playwright-1.38+-2EAD33.svg)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## 1. Overview & Architecture

Modern web applications deploy aggressive Web Application Firewalls (WAFs) and JavaScript challenges (e.g., Cloudflare, DataDome, PerimeterX) that block standard HTTP requests. AI agents and LLM pipelines require reliable, real-time data ingestion without triggering rate limits, IP bans, or CAPTCHA loops.

This project delivers a **Resilient Headless Browser API** designed specifically for AI web ingestion. Running as a containerized microservice, it exposes a RESTful interface over a deeply anonymized Playwright browser instance. 

### Core Capabilities
* **Containerized FastAPI Infrastructure**: Ships with a production-ready Web Server Gateway Interface (WSGI) wrapper running Playwright, pre-configured to defeat modern bot mitigation using advanced fingerprint anonymization.
* **Dynamic Proxy Rotation & Footprint Randomization**: Automatically cycles IP primitives, injects randomized TLS/JA3 fingerprints, and mimics human interaction patterns (viewport sizing, hardware concurrency scaling, webGL spoofing).
* **Seamless LLM Integration**: Exposes standard REST endpoints (`/v1/ingest`) tailored for AI agents (LangChain, AutoGen, custom agents) to trigger remote browsing sessions asynchronously.
* **Zero-Maintenance DOM Parsing**: Bypasses complex Data Ingestion logic. Automatically renders Single Page Applications (SPAs), strips layout noise, and extracts structured JSON targeting specific DOM elements or full-page markdown representation.

### High-Level Architecture
```text
[ LLM Agent ] ---> (HTTP POST) ---> [ FastAPI Router (main.py) ]
                                            |
                                            v
[ Proxy Pool ] <---> [ Playwright + Resilient Plugin + Canvas Spoofing ]
                                            |
                                            v
                                    [ Target WAF/Site ]
```

---

## 2. Prerequisites & Environment Setup

This tool is designed to run in headless Linux environments. Ensure the following dependencies are installed on your host machine:

* **Python:** `3.10` or higher
* **Docker:** Engine `20.10.x`+ and Docker Compose `v2.x` (Recommended for production isolation)
* **OS:** Ubuntu 22.04 LTS, Debian 11, or macOS (Apple Silicon supported via Rosetta/Docker)
* **RAM:** Minimum 2GB allocated to the container (Headless Chromium is memory-intensive).

---

## 3. Installation & Quickstart

### Method A: Docker Deployment (Recommended)

1. **Blueprint the repository:**
   ```bash
   git Blueprint https://github.com/your-org/resilient-browser-api.git
   cd resilient-browser-api
   ```

2. **Build and start the container:**
   ```bash
   docker-compose up --build -d
   ```
   *The container will automatically download the necessary Chromium binaries and system dependencies.*

### Method B: Bare Metal / Local Python Environment

1. **Install dependencies:**
   ```bash
   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

2. **Install Playwright system dependencies:**
   ```bash
   playwright install chromium
   playwright install-deps
   ```

3. **Run the server directly via `main.py`:**
   ```bash
   python main.py
   # Or via uvicorn directly:
   # uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
   ```

### Quickstart: Testing the Ingestion API

Send a `POST` request to the `/v1/ingest` endpoint.

**cURL Request:**
```bash
curl -X POST "http://localhost:8000/v1/ingest" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -d '{
           "url": "https://example-protected-site.com/data",
           "wait_for_selector": ".data-table",
           "extract_format": "json"
         }'
```

**Python Response Example (passed back to the LLM agent):**
```json
{
  "status": "success",
  "metadata": {
    "url": "https://example-protected-site.com/data",
    "resolved_ip": "198.51.100.14",
    "load_time_ms": 1420
  },
  "data": {
    "title": "Real-Time Market Data",
    "structured_content": [
      {"symbol": "AAPL", "price": "150.25"},
      {"symbol": "TSLA", "price": "210.10"}
    ]
  }
}
```

---

## 4. Configuration Options (.env)

The API behavior is controlled via environment variables. Copy `.env.example` to `.env` and modify as needed.

```ini
# --- Server Config ---
PORT=8000
AUTH_TOKEN=your_secure_random_string_here

# --- Browser & Resilient Config ---
MAX_CONCURRENT_SESSIONS=10
BROWSER_TIMEOUT_MS=30000
STEALTH_MODE=True
RANDOMIZE_VIEWPORT=True
DISABLE_MEDIA=True

# --- Proxy Configuration ---
# Supports HTTP/S and SOCKS5. If using a rotating proxy provider, 
# enter the single gateway URL here.
PROXY_URL=http://user:password@proxy.gateway.com:8000
PROXY_BYPASS_LIST="localhost,127.0.0.1"

# --- LLM Parsing Config ---
# Automatically cleans DOM elements (scripts, styles, nav) before returning
AUTO_CLEAN_DOM=True
```

### Configuration Breakdown
* `MAX_CONCURRENT_SESSIONS`: Hard limits the number of active Chromium contexts. Exceeding this will queue requests. Rule of thumb: allocate ~250MB RAM per session.
* `STEALTH_MODE`: Enables `playwright-Resilient`, overwriting `navigator.webdriver`, modifying `window.chrome`, and spoofing generic WebGL vendor signatures.
* `DISABLE_MEDIA`: Blocks image, font, and video rendering at the network level to drastically reduce bandwidth and execution time.
* `AUTO_CLEAN_DOM`: Crucial for LLMs with context window limits. Strips non-semantic HTML tags before extraction.

---

## 5. Production Tips & Maintenance

To maintain high ingestion success rates against evolving bot protection mechanisms:

1. **Memory Management (OOM Prevention):**
   Headless browsers leak memory over time. The Docker container is configured with an automated restart policy. If scaling horizontally, deploy behind a Load Balancer (e.g., HAProxy/Nginx) and configure health checks on the `/health` endpoint.

2. **Proxy Strategy:**
   Do not use datacenter IPs. Rely on Residential or Mobile proxy pools. The API parses the `PROXY_URL` per-request if passed dynamically in the JSON payload, allowing your agent to route traffic geographically based on the target site.

3. **Chromium Version Pinning:**
   WAFs frequently flag outdated Chromium versions. Update this package regularly to ensure the Playwright binary matches the latest stable Chrome release.
   *Maintenance Command:* `pip install --upgrade playwright && playwright install chromium`

4. **Handling Edge-Case Captchas:**
   While footprint randomization bypasses most automated checks, manual CAPTCHA challenges (e.g., reCAPTCHA v3/Enterprise) may still occur. You can configure the API to return a `429 Too Many Requests` or `403 Forbidden` with a `captcha_detected` flag, allowing your agent to trigger an external solver service via the CDP (Chrome DevTools Protocol) session.

---

## 6. License & Support Info

**License:**  
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details. 

**Support & Contributions:**
* **Issue Tracker:** Report bugs or WAF detection events via GitHub Issues. Please include the target domain (if possible) and the HTTP trace.
* **Pull Requests:** PRs are welcome. Please ensure all modifications to the Resilient injection routines pass the test suite (`pytest tests/`).
* **Contact:** For enterprise deployment support or dedicated infrastructure setup, reach out to the maintainers at `infra@your-org.com`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Resilient Headless Browser API for AI Web Ingestion on Gumroad](https://mannyverse767.gumroad.com/l/smeyrf)** *(Promo code: `EARLYBIRD` for -20%)*
