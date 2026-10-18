# Article-to-Markdown Extraction API — FastAPI Microservice

![Article-to-Markdown Extraction API — FastAPI Microservice](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/article-to-markdown-fastapi-microservice-20261005.png)

> **Production-ready FastAPI microservice converting arbitrary URLs into clean, token-efficient Markdown for LLMs.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/hlzptg)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Web Data Extraction for LLM pipelines frequently breaks on dynamic single-page applications, cluttered DOMs, and anti-Data Ingestion checks, polluting prompt context windows with irrelevant navigation and script artifacts. Maintaining custom extraction scripts and parsing libraries across hundreds of site layouts requires continuous engineering maintenance and escalates compute overhead.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Dual-engine extraction pipeline leveraging Trafilatura with automated Readability fallback for optimal HTML sanitization**
- ✅ **Asynchronous Playwright fallback handler for dynamic JavaScript-rendered web pages and SPAs**
- ✅ **Complete metadata harvesting including OpenGraph data, author, publication timestamp, and reading duration**
- ✅ **Production-grade FastAPI architecture with Pydantic v2 validation, rate limiting, and optional Redis caching layer**


---

## 📖 Technical Documentation & Setup Guide

# Article-to-Markdown Extraction API — FastAPI Microservice

[![Python Version](https://img.shields.io/badge/python-3.10%20%7C%203.11%20%7C%203.12-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688.svg)](https://fastapi.tiangolo.com/)
[![Pydantic](https://img.shields.io/badge/Pydantic-v2-e92063.svg)](https://docs.pydantic.dev/)
[![Playwright](https://img.shields.io/badge/Playwright-Async-45ba4b.svg)](https://playwright.dev/python/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

High-throughput, production-grade microservice designed to transform arbitrary, noisy web pages into clean, structured, LLM-ready Markdown. The service eliminates boilerplate elements (headers, footers, ad slots, tracking scripts) and extracts rich metadata (OpenGraph tags, authors, publication dates, and reading durations) using a tiered extraction pipeline with dynamic JavaScript fallback execution.

---

## 1. Overview & Architecture

Modern LLM pipelines (RAG, agent ingestion, fine-tuning datasets) degrade significantly when fed raw HTML or naïve DOM-to-text dumps. Data Ingestion logic breaks across Single-Page Applications (SPAs), anti-Data Ingestion walls, and unpredictable layouts. 

This microservice provides a uniform REST API that decouples raw web ingestion from downstream ingestion pipelines through a tiered fallback architecture:

```
                      [ Incoming POST /v1/extract ]
                                    │
                                    ▼
                      [ Redis Cache Check (Key: URL) ]
                         ├── Hit ──► Return Cached JSON Payload
                         └── Miss
                                    │
                                    ▼
                    [ Tier 1: Static Fetch via HTTPX ]
                                    │
                                    ▼
                      [ Trafilatura Engine Parse ]
                         ├── Success & Valid Content ──► [ Markdown + Metadata ] ──┐
                         └── Failure / Low Word Count                             │
                                    │                                             │
                                    ▼                                             │
                    [ Tier 2: Readability.js / LXML Fallback ]                    │
                         ├── Success & Valid Content ──► [ Markdown + Metadata ] ──┤
                         └── Failure / Empty DOM                                  │
                                    │                                             │
                                    ▼                                             │
               [ Tier 3: Async Playwright (Chromium Headless) ]                   │
                 - Executes JS / Hydrates SPA                                     │
                 - Waits for Network Idle / DOM Selectors                         │
                 - Re-runs Extraction Pipeline (Tier 1/2)                         │
                                    │                                             │
                                    ▼                                             │
                         [ Markdown + Metadata ]                                  │
                                    │                                             │
                                    ▼                                             │
                         [ Store in Redis Cache ] ◄────────────────────────────────┘
                                    │
                                    ▼
                        [ JSON Response Envelope ]
```

### Extraction Engines
1. **Tier 1 (Trafilatura Engine):** Primary static processor. Delivers top-tier text-to-boilerplate precision while systematically harvesting structured publication metadata.
2. **Tier 2 (Readability LXML Fallback):** Secondary static fallback triggered when Trafilatura fails to meet minimum length thresholds or encounters non-standard article structures. Employs Mozilla Readability heuristics parsed into Markdown via AST parsers.
3. **Tier 3 (Headless Playwright Handler):** Asynchronous browser engine invoked when static parsing returns insufficient content, detects hydration shells (React, Vue, Next.js, Angular), or when explicitly demanded via request parameters (`render_js=true`).

---

## 2. Prerequisites & Environment Setup

### System Requirements
* **Operating System:** Linux (Ubuntu 20.04/22.04 LTS recommended), macOS, or Windows Subsystem for Linux (WSL2).
* **Python:** Version `>= 3.10` and `< 3.13`.
* **System Libraries (for lxml and Playwright Chromium):**
  * `libxml2-dev`, `libxslt-dev`
  * System-level browser rendering libraries (X11, NSS, GConf, etc.)
* **Redis:** Version `>= 6.2` (optional, for result caching and distributed rate limiting).

---

## 3. Installation & Quickstart

### Step 1: Blueprint the Repository
```bash
git Blueprint https://github.com/your-org/article-to-markdown-api.git
cd article-to-markdown-api
```

### Step 2: Set Up Python Virtual Environment
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip setuptools wheel
```

### Step 3: Install Dependencies
Create a `requirements.txt` file (if not present) with the following minimum requirements:
```text
fastapi>=0.110.0
uvicorn[standard]>=0.28.0
pydantic>=2.6.0
pydantic-settings>=2.2.0
httpx>=0.27.0
trafilatura>=1.8.0
readability-lxml>=0.8.1
markdownify>=0.11.6
playwright>=1.42.0
slowapi>=0.1.9
redis>=5.0.2
python-dotenv>=1.0.1
```

Install packages:
```bash
pip install -r requirements.txt
```

### Step 4: Install Playwright Browsers
Install only the Chromium binary and required system dependencies:
```bash
playwright install --with-deps chromium
```

### Step 5: Configure Environment
Copy the example environment configuration:
```bash
cp .env.example .env
```
*(Modify `.env` values as described in [Section 4](#4-configuration-options-env-explanation)).*

### Step 6: Launch the Service
```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

---

### Step 7: Verify with cURL

#### Standard Extraction (Automatic Fallback Flow)
```bash
curl -X POST "http://localhost:8000/v1/extract" \
     -H "Content-Type: application/json" \
     -d '{
       "url": "https://example.com/blog/sample-article",
       "force_js": false,
       "include_raw_html": false
     }'
```

#### Sample Response Body (`200 OK`)
```json
{
  "success": true,
  "data": {
    "url": "https://example.com/blog/sample-article",
    "title": "Optimizing Large Language Models for Retrieval Pipelines",
    "markdown": "# Optimizing Large Language Models...\n\nProduction deployments require...",
    "metadata": {
      "author": "Dr. Jane Doe",
      "published_at": "2024-03-29T14:32:00Z",
      "opengraph": {
        "og:title": "Optimizing LLMs for Retrieval",
        "og:description": "Detailed guide on engineering high-density context windows.",
        "og:image": "https://example.com/static/cover.jpg"
      },
      "reading_time_minutes": 4,
      "word_count": 872
    },
    "engine_used": "trafilatura",
    "execution_time_ms": 142.8
  },
  "error": null
}
```

---

## 4. Configuration Options (.env Explanation)

The microservice utilizes `pydantic-settings` to parse environment variables, enforcing strict typing and defaults across environments.

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `ENVIRONMENT` | `string` | `production` | Deployment environment: `development`, `staging`, or `production`. |
| `HOST` | `string` | `0.0.0.0` | Bind host interface. |
| `PORT` | `integer`| `8000` | Bind TCP port. |
| `LOG_LEVEL` | `string` | `INFO` | Logging level (`DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`). |
| `REDIS_URL` | `string` | `""` | Connection string for Redis caching (e.g., `redis://:secret@localhost:6379/0`). Disabled if blank. |
| `CACHE_TTL_SECONDS` | `integer`| `86400` | Expiration time for cached responses (default: 24 hours). |
| `RATE_LIMIT_PER_MINUTE` | `string` | `60/minute` | Rate limit format parsed by `slowapi` per client IP. |
| `HTTP_TIMEOUT_SECONDS` | `float` | `10.0` | Timeout threshold for static HTTP requests using `httpx`. |
| `PLAYWRIGHT_TIMEOUT_MS` | `integer`| `30000` | Page navigation and network idle timeout for dynamic parsing. |
| `PLAYWRIGHT_POOL_SIZE` | `integer`| `3` | Maximum concurrent browser contexts per worker instance. |
| `MIN_CONTENT_LENGTH` | `integer`| `200` | Minimum character threshold required to Automated Routing dynamic SPA fallback. |

---

## 5. Production Tips & Maintenance

### 1. Docker Deployment
Use multi-stage Docker builds to keep images small while ensuring Playwright dependencies are correctly linked:

```dockerfile
FROM python:3.11-slim-bookworm AS base

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=1

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libxml2-dev \
    libxslt1-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Install Playwright browser and exact OS system dependencies
RUN playwright install --with-deps chromium

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "4", "--proxy-headers", "--forwarded-allow-ips", "*"]
```

### 2. Chromium Resource Management & Zombie Processes
* **Memory Leaks:** Headless Chromium instances are prone to progressive memory leaks. Do not instantiate a new `Browser` process per request. Maintain a single long-lived `playwright.chromium.launch()` process per worker and create/destroy isolated `BrowserContext` objects per session.
* **Zombie Mitigation:** If deploying under custom container environments, ensure the container engine initiates with an init system like `tini` (e.g., `docker run --init ...`) to properly reap orphaned Chromium child processes.
* **Shared Memory Exhaustion:** Pass the flags `--disable-dev-shm-usage` and `--no-sandbox` to the Playwright launch arguments within containerized environments to prevent silent crashes triggered by `/dev/shm` size limits.

### 3. Worker Sizing Architecture
* Each active Playwright page context consumes approximately 80MB to 150MB of RAM under heavy DOM loads.
* Formula for node sizing:
  $$\text{RAM Required} = (\text{Worker Processes} \times \text{Uvicorn Base (100MB)}) + (\text{Worker Processes} \times \text{Browser Pool Size} \times 150\text{MB}) + \text{OS Overhead}$$
* Example: A 4-worker instance with a pool size of 3 contexts per worker needs approximately:
  $$(4 \times 100\text{MB}) + (4 \times 3 \times 150\text{MB}) + 500\text{MB} \approx 2.7\text{GB}\text{ RAM}$$

### 4. Cache Invalidation Patterns
To programmatically Automated Routing or invalidate cached URLs, expose a query or JSON flag:
```json
{
  "url": "https://example.com/article",
  "bypass_cache": true
}
```
In Redis, extraction payloads are stored with hashed SHA-256 keys of the normalized URL:
```python
cache_key = f"extract:{hashlib.sha256(canonical_url.encode()).hexdigest()}"
```

---

## 6. License & Support Info

### License
This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for complete details.

### Support & Operational Escalation
* **Issue Tracker:** For bugs, engine failures, and rendering discrepancies, file an issue directly in the repository with the extraction payload and target URL.
* **Security Advisories:** If you discover an SSRF (Server-Side Request Forgery) attack vector or arbitrary code execution flaw in DOM rendering, please email `security@your-domain.com` instead of using the public issue tracker.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Article-to-Markdown Extraction API — FastAPI Microservice on Gumroad](https://mannyverse767.gumroad.com/l/hlzptg)** *(Promo code: `EARLYBIRD` for -20%)*
