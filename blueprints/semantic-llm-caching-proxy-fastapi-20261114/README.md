# Semantic LLM Caching Proxy & Token Tracker — Python FastAPI

![Semantic LLM Caching Proxy & Token Tracker — Python FastAPI](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/semantic-llm-caching-proxy-fastapi-20261114.png)

> **Self-hosted Python FastAPI engine to semantic-cache LLM requests, reduce API latency, and track token costs automatically**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€17 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/xdygl)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Redundant LLM API requests drain your budget, trigger rate limits, and introduce unnecessary user latency. Relying on enterprise AI observability platforms costs thousands of dollars annually just to track tokens and route prompts efficiently.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€17 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Sub-millisecond semantic caching using lightweight local embeddings**
- ✅ **Drop-in OpenAI-compatible proxy interface for instant integration**
- ✅ **Granular token tracking and cost calculation logged via SQLite**
- ✅ **Production-ready FastAPI architecture engineered for high concurrency**


---

## 📖 Technical Documentation & Setup Guide

# Semantic LLM Caching Proxy & Token Tracker

A high-performance, production-ready FastAPI proxy designed to intercept, cache, and log LLM API requests. 

**The Problem:** Redundant LLM API requests drain your budget, trigger upstream rate limits, and introduce unnecessary latency. Relying on enterprise AI observability platforms costs thousands of dollars annually just to track tokens and route prompts efficiently.

**The Solution:** This tool provides a self-hosted, lightweight proxy layer. By utilizing local embedding models for semantic cache resolution, it serves identical or highly similar prompts in sub-milliseconds without hitting upstream providers. All requests are meticulously tracked in a local SQLite database for granular cost and token observability.

## Features

- **Sub-Millisecond Semantic Caching:** Utilizes lightweight local embeddings (e.g., `sentence-transformers`) and in-memory vector indexing to identify and serve semantically similar prompts near-instantly.
- **Drop-in OpenAI-Compatible Proxy:** Fully compatible with the OpenAI API specification. Integrate with your existing applications simply by altering the `base_url`.
- **Granular Token Tracking & Cost Calculation:** Automatically intercepts usage metadata, calculates costs based on model pricing, and logs payloads securely to a local SQLite database.
- **Production-Ready FastAPI Architecture:** Engineered for high concurrency, utilizing asynchronous Python (`asyncio`), efficient dependency injection, and non-blocking I/O.

---

## 1. Overview & Architecture

The proxy operates as a middleware layer between your application and the upstream LLM provider (e.g., OpenAI). 

**Request Flow:**
1. **Interception:** The application sends a standard OpenAI ChatCompletion request to the proxy.
2. **Embedding & Search:** The proxy embeds the incoming prompt using a lightweight, locally hosted model and queries the vector index for semantic matches based on a configurable threshold.
3. **Cache Hit:** If a match is found, the cached response is immediately returned (Latency: < 50ms total, < 1ms vector search).
4. **Cache Miss:** The request is forwarded to the upstream LLM.
5. **Logging & Storage:** The upstream response is intercepted, tokens/costs are calculated, the interaction is logged asynchronously to SQLite, and the new prompt/response pair is added to the semantic cache.
6. **Return:** The response is streamed or returned to the client application.

---

## 2. Prerequisites & Environment Setup

Ensure your environment meets the following requirements before deployment:

* **OS:** Linux (Ubuntu/Debian recommended), macOS, or Windows (WSL2)
* **Python:** 3.9+ (3.11+ recommended for optimal `asyncio` performance)
* **Build Tools:** GCC/G++ (required for compiling certain vector library dependencies like FAISS)
* **Hardware:** Minimum 1GB RAM. No GPU required (local embeddings use highly optimized, quantized CPU models).

---

## 3. Installation & Quickstart

### Step 1: Blueprint the Repository
```bash
git Blueprint https://github.com/your-org/semantic-llm-proxy.git
cd semantic-llm-proxy
```

### Step 2: Create a Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```
*(Dependencies include `fastapi`, `uvicorn`, `sentence-transformers`, `openai`, `faiss-cpu`, `sqlite3`, etc.)*

### Step 4: Configure the Environment
Copy the example environment file and add your upstream credentials.
```bash
cp .env.example .env
```

### Step 5: Start the Server
Run the FastAPI application (`main.py`) via Uvicorn.
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

### Step 6: Test the Drop-in Replacement
Modify your existing OpenAI client initialization to point to the proxy:

```python
from openai import OpenAI

client = OpenAI(
    api_key="your-openai-api-key", # Passed through to upstream
    base_url="http://localhost:8000/v1" # Proxy address
)

response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Explain quantum computing in one sentence."}]
)
print(response.choices[0].message.content)
```
*The first request will hit OpenAI. Subsequent semantically identical or similar requests will be returned directly from the proxy cache.*

---

## 4. Configuration Options

Behavior is governed via environment variables. Define these in your `.env` file or export them to your system environment.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `PROXY_PORT` | `8000` | The port on which the FastAPI server listens. |
| `OPENAI_API_KEY` | `""` | Optional fallback key. If omitted, the proxy uses the Bearer token passed by the client. |
| `EMBEDDING_MODEL` | `all-MiniLM-L6-v2` | The local HuggingFace model used for semantic caching. Highly optimized for CPU. |
| `SIMILARITY_THRESHOLD` | `0.95` | Cosine similarity threshold (0.0 to 1.0) required to trigger a semantic cache hit. |
| `SQLITE_DB_PATH` | `./logs/llm_usage.db` | Absolute or relative path to the SQLite database file. |
| `CACHE_TTL_SECONDS` | `86400` | Time-to-live for cached responses. Set to `0` for indefinite retention. |
| `LOG_LEVEL` | `INFO` | Python logging level (`DEBUG`, `INFO`, `WARNING`, `ERROR`). |

---

## 5. Production Tips & Maintenance

To ensure high availability and data integrity in a production environment, adhere to the following operational guidelines:

### Worker Scaling
Do not run a single Uvicorn instance in production. Use Gunicorn with Uvicorn workers to utilize multiple CPU cores effectively:
```bash
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

### SQLite Concurrency (WAL Mode)
By default, the proxy automatically attempts to enable Write-Ahead Logging (WAL) on the SQLite database to support high-concurrency asynchronous writes. Ensure your underlying filesystem supports WAL. If you experience `database is locked` errors under heavy load, verify disk I/O limits.

### Cache Eviction & Memory Management
The in-memory vector index (e.g., FAISS) will grow alongside your cache. 
- Monitor memory usage. 
- Implement a cron job or background task to clear embeddings older than `CACHE_TTL_SECONDS`.
- For enterprise-scale deployments exceeding millions of cached prompts, consider swapping the local index for a dedicated vector database (e.g., Milvus, Qdrant).

### Security & Routing
- **Do not expose this proxy directly to the public internet.** Run it securely within your VPC, VPC subnet, or behind a reverse proxy (Nginx/Traefik) with strict IP allowlisting.
- The proxy passes through the `Authorization` header by default. Ensure client applications are properly securing their API keys.

---

## 6. License & Support Info

**License:**  
This project is licensed under the MIT License. See the `LICENSE` file for details. You are free to use, modify, and distribute this software in both commercial and non-commercial environments.

**Support & Contributions:**  
- **Bugs & Feature Requests:** Please open an issue on the [GitHub Issue Tracker](#). Provide detailed reproduction steps and tracebacks where applicable.
- **Contributions:** Pull requests are welcome. Please ensure all code conforms to `black` formatting and passes existing `pytest` suites before submitting. 
- **Security Vulnerabilities:** Do not open public issues for security vulnerabilities. Contact the maintainers directly at `security@your-org.com`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Semantic LLM Caching Proxy & Token Tracker — Python FastAPI on Gumroad](https://mannyverse767.gumroad.com/l/xdygl)** *(Promo code: `EARLYBIRD` for -20%)*
