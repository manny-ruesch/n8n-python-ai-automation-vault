# Local Toolchain Analytics & Productivity Engine — Python Pipeline

![Local Toolchain Analytics & Productivity Engine — Python Pipeline](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/local-toolchain-analytics-python-20261117.png)

> **A self-hosted Python engine to privately track local developer tool usage and AI agent metrics without external telemetry.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€19 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/ojvcj)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Operating systems and cloud platforms increasingly harvest developer tool usage data for their own telemetry. Engineering teams need a private, self-hosted way to track IDE adoption, AI assistant usage, and local tool execution without leaking intellectual property or paying exorbitant per-seat SaaS analytics fees.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€19 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Local background daemon for tracking IDE and CLI execution metrics**
- ✅ **Privacy-first architecture running 100% offline with zero external telemetry**
- ✅ **Automated SQLite aggregation and customizable JSON payload generation**
- ✅ **Pre-configured export schema for seamless internal BI tool integration**


---

## 📖 Technical Documentation & Setup Guide

# Local Toolchain Analytics & Productivity Engine

**Python Pipeline for Privacy-First Developer Telemetry**

---

## 1. Overview & Architecture

Modern operating systems, IDEs, and AI coding assistants increasingly harvest telemetry and tool usage data, raising severe intellectual property (IP) and privacy concerns. Furthermore, third-party developer productivity SaaS platforms charge exorbitant per-seat fees while requiring off-premise data exfiltration.

The **Local Toolchain Analytics & Productivity Engine** is a self-hosted, air-gapped Python pipeline designed to reclaim engineering metrics. It tracks IDE adoption, AI assistant usage, and local CLI tool execution directly on the developer's machine, aggregating data securely without leaking IP to external vendors.

### Core Features
*   **Local Background Daemon**: Silently tracks IDE and CLI execution metrics via low-overhead system process polling.
*   **100% Offline Privacy-First Architecture**: Zero external telemetry. All data is processed and stored locally on the host machine.
*   **Automated SQLite Aggregation**: Efficient local data storage with customizable JSON payload generation for downstream ETL.
*   **Pre-Configured BI Export Schema**: Generates standardized payloads optimized for seamless integration with internal Business Intelligence (BI) tools (e.g., Metabase, Superset, Grafana).

### Architecture Overview
1.  **Collector (`main.py --daemon`)**: Polls OS-level process APIs (via `psutil`) at configurable intervals to detect active toolchain processes (e.g., `vscode`, `cursor`, `docker`, `kubectl`).
2.  **Storage Engine**: Writes raw telemetry to a local SQLite database (`toolchain.db`), utilizing Write-Ahead Logging (WAL) for concurrent read/write performance.
3.  **Aggregator & Exporter (`main.py --export`)**: Queries the SQLite database, aggregates usage durations, and outputs a normalized JSON schema to a designated sink (local directory or internal webhook).

---

## 2. Prerequisites & Environment Setup

This pipeline is designed to run in macOS, Linux, and Windows environments.

*   **Python**: `3.9` or higher.
*   **System Libraries**: `sqlite3` (typically bundled with standard Python distributions).
*   **OS Permissions**: Execution requires adequate user-level permissions to read process lists. (Root/Administrator is *not* required unless tracking cross-user processes).
*   **Disk Space**: ~50MB for the application, plus ~10-20MB/month for SQLite local retention depending on polling frequency.

---

## 3. Installation & Quickstart

### Step 1: Blueprint and Isolate Environment
Blueprint the repository and instantiate a virtual environment to prevent dependency contamination.

```bash
git Blueprint ssh://git@your-internal-repo/toolchain-analytics.git
cd toolchain-analytics
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### Step 2: Install Dependencies
Install required packages (primarily `psutil` for cross-platform process management and standard utility libraries).

```bash
pip install -r requirements.txt
```

### Step 3: Initialize the Local Database
Run the idempotent initialization command to generate the SQLite schema (`toolchain.db`).

```bash
python main.py --init
```

### Step 4: Start the Daemon
Launch the background collector. In a local testing environment, you can run this in the foreground. For production, see section 5.

```bash
python main.py --daemon
```

### Step 5: Export Telemetry
Manually trigger an aggregation and JSON export.

```bash
python main.py --export --output ./exports/telemetry_$(date +%s).json
```

---

## 4. Configuration Options

The pipeline is strictly configured via environment variables. Create a `.env` file in the root directory. The application utilizes `python-dotenv` to load these values.

### Example `.env`

```ini
# Core Execution
POLLING_INTERVAL_SECONDS=15
LOG_LEVEL=INFO
DATA_DIR=~/.local/share/toolchain-analytics

# Process Tracking
TRACKED_PROCESSES=code,cursor,idea,pycharm,vim,nvim,docker,kubectl,terraform
IGNORE_PROCESSES=grep,awk,sed,bash,zsh

# Export & BI Integration
EXPORT_FORMAT=json
EXPORT_RETENTION_DAYS=30
BI_SINK_PATH=/mnt/shared_network_drive/telemetry/
```

### Parameter Reference

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `POLLING_INTERVAL_SECONDS` | Integer | `15` | Frequency of OS process polling. Lower values increase accuracy but marginally increase CPU overhead. |
| `LOG_LEVEL` | String | `INFO` | Standard Python logging levels (`DEBUG`, `INFO`, `WARNING`, `ERROR`). |
| `DATA_DIR` | String | `./data` | Absolute or relative path where the `toolchain.db` SQLite file resides. |
| `TRACKED_PROCESSES` | CSV | *None* | Comma-separated list of executable names to explicitly track. If blank, tracks all dev-categorized tools. |
| `IGNORE_PROCESSES` | CSV | *None* | Comma-separated list of executables to aggressively filter out of the tracking loop. |
| `EXPORT_FORMAT` | String | `json` | Desired output schema (`json`, `csv`, `parquet`). |
| `EXPORT_RETENTION_DAYS` | Integer | `30` | Number of days to retain raw local SQLite data before automatic pruning. |
| `BI_SINK_PATH` | String | `./exports` | Target directory or local network mount for JSON payload generation. |

---

## 5. Production Tips & Maintenance

### Daemonization (Linux via Systemd)
For reliable, background tracking across reboots, manage the pipeline via a `systemd` user service rather than manual shell execution.

1. Create a service file: `~/.config/systemd/user/toolchain-analytics.service`
```ini
[Unit]
Description=Local Toolchain Analytics Daemon
After=network.target

[Service]
Type=simple
WorkingDirectory=%h/toolchain-analytics
ExecStart=%h/toolchain-analytics/.venv/bin/python main.py --daemon
Restart=always
RestartSec=10
EnvironmentFile=%h/toolchain-analytics/.env

[Install]
WantedBy=default.target
```
2. Enable and start the service:
```bash
systemctl --user enable toolchain-analytics
systemctl --user start toolchain-analytics
```

### Data Pruning & SQLite Maintenance
The pipeline is designed to be low-maintenance, but SQLite databases can fragment over time.
*   **Auto-Pruning**: The daemon automatically drops records older than `EXPORT_RETENTION_DAYS` during the `--export` routine.
*   **Vacuuming**: It is recommended to run a monthly cron job to execute `VACUUM;` on the SQLite database to reclaim disk space.

### Internal BI Integration
The generated JSON payloads utilize a flattened, timestamped schema optimized for ingestion into SIEMs (like Splunk/Elastic) or BI visualization tools (like Metabase).
To automate ingestion, set up a cron job to execute `python main.py --export --push-to-webhook https://internal-bi.local/api/ingest` (assuming internal networking allows).

---

## 6. License & Support Info

**License**: Proprietary / Internal Use Only. (If open-sourcing internally, default to **Apache License 2.0**).
*Do not distribute outside the organizational boundary.*

**Support & Issue Tracking**:
*   **Maintainer**: Internal Developer Experience (DevEx) Team.
*   **Bug Reports**: Please open an issue on the internal Jira board under the `DEVEX` project, tagged with `toolchain-analytics`.
*   **Contributions**: Pull requests are welcome. Please ensure `pytest` coverage remains above 85% for all OS abstraction layers before requesting a review.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Local Toolchain Analytics & Productivity Engine — Python Pipeline on Gumroad](https://mannyverse767.gumroad.com/l/ojvcj)** *(Promo code: `EARLYBIRD` for -20%)*
