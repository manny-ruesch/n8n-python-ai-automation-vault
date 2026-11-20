# Offline-First Edge AI Processing Engine — Python API

![Offline-First Edge AI Processing Engine — Python API](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/offline-first-edge-ai-processing-python-20261118.png)

> **A self-hosted Python ONNX inference API for zero-cost, offline data enrichment and routing.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/esrqj)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Cloud-based AI APIs expose sensitive enterprise data and incur infinite, unpredictable usage costs per token. Relying on centralized cloud endpoints creates single points of failure and compromises data sovereignty.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **100% offline ONNX-powered text classification**
- ✅ **Zero-token-cost local embeddings generation**
- ✅ **Hardware-agnostic CPU optimization**
- ✅ **Plug-and-play REST API for enterprise automation platforms**


---

## 📖 Technical Documentation & Setup Guide

# Offline-First Edge AI Processing Engine — Python API

## 1. Overview & Architecture

Cloud-based AI APIs present critical vulnerabilities for enterprise deployments: they expose sensitive proprietary data, incur infinite and unpredictable token-based billing, and introduce external single points of failure that compromise zero-trust architectures and data sovereignty. 

The **Offline-First Edge AI Processing Engine** mitigates these risks by shifting inference to the edge. This engine delivers a fully air-gapped, highly optimized Python REST API (`main.py`) designed for seamless integration into enterprise automation platforms.

### Core Capabilities
*   **100% Offline ONNX-Powered Text Classification:** Execute high-throughput classification tasks without network dependencies.
*   **Zero-Token-Cost Local Embeddings:** Generate dense vector embeddings locally for RAG (Retrieval-Augmented Generation) pipelines, eliminating continuous API usage costs.
*   **Hardware-Agnostic CPU Optimization:** Leverages ONNX Runtime with AVX2/AVX-512 optimizations to deliver near-GPU latency on standard commodity x86/ARM CPUs.
*   **Plug-and-Play REST API:** Built on FastAPI, providing stateless, documented endpoints ready for immediate consumption by internal microservices, Make/Zapier, or custom orchestration tools.

### Architecture
The request lifecycle remains entirely within your secure network perimeter:
`Client -> REST API (main.py) -> Tokenizer -> ONNX Runtime (C++ backend) -> Output Payload`

---

## 2. Prerequisites & Environment Setup

This tool is designed to run efficiently on standard CPU hardware. No dedicated GPUs or CUDA drivers are required.

**System Requirements:**
*   **OS:** Linux (Ubuntu 20.04+, RHEL 8+), macOS (Apple Silicon or Intel), or Windows Server.
*   **CPU:** Modern x86_64 (Intel/AMD) with AVX2 support, or ARM64.
*   **RAM:** 4GB minimum (8GB recommended for concurrent embedding/classification workloads).

**Software Requirements:**
*   Python 3.9 - 3.11
*   `pip` and `venv` (Standard library)
*   *Optional:* Docker (for containerized deployment)

---

## 3. Installation & Quickstart

To maintain strict data sovereignty, all model weights must be downloaded once and cached locally before air-gapping the system.

### Step 1: Blueprint and Isolate Environment
```bash
git Blueprint https://github.com/your-org/edge-ai-engine.git
cd edge-ai-engine
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### Step 2: Install Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```
*(Note: `requirements.txt` should include `fastapi`, `uvicorn`, `onnxruntime`, `tokenizers`, and `python-dotenv`.)*

### Step 3: Fetch Models (Pre-Air-Gap)
Run the initialization script to download the ONNX-quantized models and tokenizers to your local `./models` directory.
```bash
python scripts/fetch_models.py
```

### Step 4: Boot the Engine
Execute the main application.
```bash
python main.py
```
The server will initialize the ONNX execution sessions in memory and bind to `http://0.0.0.0:8000`.

### Step 5: Test the Endpoints

**Test Text Classification:**
```bash
curl -X POST "http://localhost:8000/v1/classify" \
     -H "Content-Type: application/json" \
     -d '{"text": "The latest Q3 financial results show a 20% increase in revenue."}'
```

**Test Embeddings Generation:**
```bash
curl -X POST "http://localhost:8000/v1/embeddings" \
     -H "Content-Type: application/json" \
     -d '{"text": "Enterprise data sovereignty."}'
```

---

## 4. Configuration Options

The engine's behavior is deterministic and controlled entirely via environment variables. Create a `.env` file in the project root to override default settings.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `API_HOST` | `0.0.0.0` | Bind address for the REST API. |
| `API_PORT` | `8000` | Port for the REST API. |
| `MODELS_DIR` | `./models` | Absolute or relative path to the local ONNX model cache. |
| `ENABLE_EMBEDDINGS` | `true` | Load the embedding execution session into memory on boot. |
| `ENABLE_CLASSIFIER` | `true` | Load the classification execution session into memory on boot. |
| `ONNX_INTRA_OP_THREADS` | `0` | Threads used to parallelize a single model execution. `0` = auto-detect physical cores. |
| `ONNX_INTER_OP_THREADS` | `1` | Threads used to parallelize across multiple independent nodes. Keep at `1` for CPU. |
| `LOG_LEVEL` | `info` | API logging verbosity (`debug`, `info`, `warning`, `error`). |
| `AUTH_TOKEN` | `null` | If set, requires `Bearer <token>` on all requests. |

**Example `.env` configuration for a multi-core CPU production server:**
```env
API_PORT=8080
MODELS_DIR=/var/lib/edge-ai/models
ONNX_INTRA_OP_THREADS=4
LOG_LEVEL=warning
AUTH_TOKEN=sk-local-edge-xyz123
```

---

## 5. Production Tips & Maintenance

To ensure high availability and optimal throughput in an enterprise environment, adhere to the following operational guidelines:

*   **Concurrency & Workers:** Do not rely on the development server (`main.py` running natively). Use a production ASGI server like Uvicorn managed by Gunicorn. Example:
    ```bash
    gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
    ```
    *Rule of thumb:* Set worker count (`-w`) to `(Number of CPU Cores / 2)`.

*   **ONNX Thread Tuning:** Over-provisioning ONNX threads (`ONNX_INTRA_OP_THREADS`) leads to context-switching overhead. Explicitly map this value to the number of physical (not logical/hyperthreaded) cores allocated per worker.

*   **Immutability via Docker:** Package the application, requirements, and the `./models` directory into a single Docker image. This guarantees that deployments are immutable, completely self-contained, and immune to upstream model repository deprecations.

*   **Air-Gapped Updates:** When updating model architectures or weights, do not open firewalls. Generate a new Docker image containing the updated `.onnx` files in a DMZ or staging environment, and promote the artifact across the air-gap via your secure container registry.

*   **Health Checks:** Configure your load balancer or Kubernetes readiness probes to poll `GET /health`. This endpoint verifies that both the REST API is responsive and the ONNX execution sessions are successfully mapped in memory.

---

## 6. License & Support Info

**License:** 
Distributed under the internal Enterprise License. By using this software, you comply with the organization's Zero-Trust Data Sovereignty protocols. Source code modification is permitted for internal use only.

**Support:**
*   **Documentation & OpenAPI:** Once the server is running, navigate to `http://localhost:8000/docs` for the interactive Swagger UI and OpenAPI schemas.
*   **Issue Tracking:** Report bugs or request hardware-specific optimizations via the internal Jira portal: `PROJ-EDGE-AI`.
*   **Maintainers:** For emergency integration support, ping `#edge-ai-engine` on the corporate Slack or contact the ML Infrastructure Team.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Offline-First Edge AI Processing Engine — Python API on Gumroad](https://mannyverse767.gumroad.com/l/esrqj)** *(Promo code: `EARLYBIRD` for -20%)*
