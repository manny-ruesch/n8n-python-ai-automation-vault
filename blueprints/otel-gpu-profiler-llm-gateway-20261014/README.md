# OTel-Native GPU Profiler & LLM Cost Optimization Gateway

![OTel-Native GPU Profiler & LLM Cost Optimization Gateway](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/otel-gpu-profiler-llm-gateway-20261014.png)

> **Monitor LLM inference costs and GPU utilization in real-time with a self-hosted, OpenTelemetry-native metrics gateway.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/ormsmj)** | ⚡ **[Instant Access on Whop](https://whop.com/mr-studio-7faa/otel-native-gpu-profiler-llm-cost-optimization-gateway)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Running LLM workloads at scale creates opaque GPU utilization and runaway inference costs. Traditional cloud monitoring lacks granular, step-by-step visibility into CUDA kernel execution and token generation costs, causing teams to over-provision expensive hardware.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **OpenTelemetry (OTel) native metric export for Prometheus and Grafana**
- ✅ **Real-time CUDA memory, VRAM allocation, and compute utilization tracking**
- ✅ **Per-token inference cost calculation and step-latency profiling**
- ✅ **Drop-in async middleware gateway for custom AI backends**


---

## 📖 Technical Documentation & Setup Guide

# OTel-Native GPU Profiler & LLM Cost Optimization Gateway

## 1. Overview & Architecture

Running Large Language Model (LLM) workloads at scale inherently introduces opaque GPU utilization and runaway inference costs. Traditional cloud monitoring solutions lack the introspection required to capture granular, step-by-step visibility into CUDA kernel execution, VRAM allocation, and token generation economics. This forces engineering teams to over-provision expensive GPU hardware to handle latency spikes.

The **OTel-Native GPU Profiler & LLM Cost Optimization Gateway** is a specialized Python-based middleware (`main.py`) designed to solve this visibility gap. It acts as an asynchronous, non-blocking gateway that sits between your application clients and your custom AI backends (e.g., vLLM, TensorRT-LLM, Hugging Face TGI).

### Key Features
* **OpenTelemetry (OTel) Native Metric Export:** Full compliance with OTel standards for seamless integration with Prometheus and Grafana.
* **Real-Time CUDA Profiling:** Nanosecond-resolution tracking of CUDA memory, dynamic VRAM allocation, and streaming multiprocessor (SM) compute utilization.
* **Economic Profiling:** Deterministic per-token inference cost calculation combined with step-latency profiling (Time-To-First-Token, Time-Between-Tokens).
* **Drop-in Async Middleware:** Designed as an ASGI-compliant interceptor; requires zero code changes to your underlying AI model execution code.

### Architecture Topology
```text
[Client Requests] 
       │
       ▼
┌────────────────────────────────────────────────────────┐
│  Cost Optimization Gateway (main.py)                   │
│   ├─ Async Request Interceptor                         │
│   ├─ NVML / CUDA Profiling Thread                      │
│   └─ Token Economics Calculator                        │
└──────┬──────────────────────────────────────────┬──────┘
       │ (Proxies Request)                        │ (OTLP gRPC/HTTP Export)
       ▼                                          ▼
[Custom AI Backend]                       [OTel Collector]
(vLLM / Triton / TGI)                             │
       │                                          ▼
[Physical GPU]                        [Prometheus / Grafana]
```

---

## 2. Prerequisites & Environment Setup

This gateway relies on low-level NVIDIA Management Library (NVML) bindings and OpenTelemetry SDKs. Ensure your environment meets the following baseline requirements:

* **Operating System:** Linux (Ubuntu 20.04/22.04 LTS or RHEL 8+ recommended). Windows is not supported for production GPU profiling.
* **NVIDIA Driver & CUDA:** 
  * NVIDIA Driver `>= 525.xx`
  * CUDA Toolkit `>= 11.8` (Test with `nvcc --version` and `nvidia-smi`).
* **Python Runtime:** Python 3.9 through 3.11.
* **Telemetry Backend:** An accessible OpenTelemetry Collector (v0.80+) configured to route metrics to a Prometheus instance.

---

## 3. Installation & Quickstart

Follow these steps to deploy the gateway on a GPU-accelerated node.

### Step 1: Blueprint and Isolate Environment
```bash
git Blueprint https://github.com/your-org/otel-gpu-profiler.git
cd otel-gpu-profiler
python3 -m venv .venv
source .venv/bin/activate
```

### Step 2: Install Dependencies
The application requires ASGI frameworks, OTel SDKs, and NVML wrappers.
```bash
pip install -U pip
pip install fastapi uvicorn nvidia-ml-py httpx 
pip install opentelemetry-api opentelemetry-sdk \
            opentelemetry-exporter-otlp \
            opentelemetry-instrumentation-fastapi
```

### Step 3: Run the Gateway
Execute the `main.py` entrypoint. By default, it will bind to `0.0.0.0:8000` and begin attempting to proxy requests and export metrics.

```bash
# Run using uvicorn directly or via the main.py wrapper
python main.py
```
*Note: In development, append `--reload` to the internal uvicorn call, but never use this in production as it interferes with CUDA memory locks.*

---

## 4. Configuration Options

The gateway is configured entirely via environment variables (`.env`). Create a `.env` file in the root directory alongside `main.py`.

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | URL | `http://localhost:4317` | The gRPC/HTTP endpoint for your OpenTelemetry Collector. |
| `AI_BACKEND_URL` | URL | `http://localhost:8080` | The upstream URL of your actual LLM inference server (e.g., vLLM). |
| `GPU_POLL_INTERVAL_MS` | Integer | `10` | Polling frequency for NVML stats. Lower intervals increase CPU overhead but yield higher resolution VRAM metrics. |
| `COST_PER_1K_TOKENS_IN` | Float | `0.0015` | Arbitrary cost metric applied to prompt tokens for OTel budget counters. |
| `COST_PER_1K_TOKENS_OUT` | Float | `0.0020` | Arbitrary cost metric applied to generated tokens for OTel budget counters. |
| `LOG_LEVEL` | String | `INFO` | Standard Python logging levels (`DEBUG`, `INFO`, `WARNING`, `ERROR`). |
| `ENABLE_STEP_PROFILING` | Boolean | `true` | Toggles the injection of timestamps between streamed chunks to calculate Time-Between-Tokens (TBT). |

**Example `.env`:**
```env
OTEL_EXPORTER_OTLP_ENDPOINT="http://otel-collector.observability.svc.cluster.local:4317"
AI_BACKEND_URL="http://127.0.0.1:8080"
GPU_POLL_INTERVAL_MS=15
COST_PER_1K_TOKENS_IN=0.0005
COST_PER_1K_TOKENS_OUT=0.0015
```

---

## 5. Production Tips & Maintenance

To ensure the gateway does not introduce latency or bottleneck your GPU workloads, adhere to the following production guidelines:

* **NVML Polling Overhead:** Querying NVML at extremely high frequencies (e.g., `< 5ms`) can block the Python Global Interpreter Lock (GIL) and degrade the ASGI event loop performance. Keep `GPU_POLL_INTERVAL_MS` between 10ms and 50ms for production.
* **Worker Processes vs. NVML Initialization:** If deploying with Gunicorn + Uvicorn workers (`-w 4`), ensure NVML is initialized *after* the worker process forks. `main.py` handles this internally via FastAPIs `@app.on_event("startup")` lifecycle hook. Do not initialize NVML at the module level.
* **Streaming Responses:** The gateway intercepts Server-Sent Events (SSE) to count tokens on-the-fly without buffering the entire response. Ensure your upstream AI backend formats streams properly (e.g., OpenAI-compatible `data: {...}` payloads) for accurate `COST_PER_1K_TOKENS_OUT` calculation.
* **OOM Metric Traps:** When a CUDA Out-Of-Memory (OOM) event occurs, the gateway will catch the backend 500 error, poll the final VRAM state, emit a critical `gpu.memory.oom` OTel event, and gracefully surface the failure to the client.

---

## 6. License & Support Info

**License:**  
This software is distributed under the **Apache License 2.0**. See the `LICENSE` file for full terms and conditions. Commercial redistribution, modification, and private use are permitted provided the original license and copyright notices are retained.

**Support & Issues:**  
* **Bug Reports:** Please file an issue on the GitHub repository using the `[BUG]` template. Include your `nvidia-smi` output, `.env` configuration (redact sensitive URLs), and OTel Collector logs.
* **Feature Requests:** Submit via GitHub issues using the `[FEATURE]` template.
* **Security Disclosures:** Do not open public issues for security vulnerabilities. Please email `security@your-org.com` directly. 

For enterprise support, deployment architecture reviews, or custom integrations with proprietary GPU clusters, contact `enterprise@your-org.com`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get OTel-Native GPU Profiler & LLM Cost Optimization Gateway on Gumroad](https://mannyverse767.gumroad.com/l/ormsmj)** *(Promo code: `EARLYBIRD` for -20%)*
