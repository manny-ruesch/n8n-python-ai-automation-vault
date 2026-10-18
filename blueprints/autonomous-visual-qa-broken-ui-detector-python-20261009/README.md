# Autonomous Visual QA & Broken UI Bug Detector — Python & Playwright CLI

![Autonomous Visual QA & Broken UI Bug Detector — Python & Playwright CLI](https://raw.githubusercontent.com/manny-ruesch/manny-ruesch.github.io/main/assets/gumroad/autonomous-visual-qa-broken-ui-detector-python-20261009.png)

> **AI-powered visual regression crawler that flags broken layouts and UI bugs across screen sizes without pixel-diffing.**

---

## ⚡ Instant Download & Turnkey Blueprint

- 📦 **Complete Production Package (€14 (Use code `EARLYBIRD` for -20%)):** 👉 **[Download on Gumroad](https://mannyverse767.gumroad.com/l/wuncpo)**
- 🎁 **Free Starter Blueprints (0€):**
  - [Autonomous AI Memory & Notion/Airtable RAG Sync (n8n Engine)](https://mannyverse767.gumroad.com/l/ai-agent-memory-rag-sync-notion-airtable-n8n-20260929)
  - [Programmatic SEO Markdown Engine with Quality Gates (Python)](https://mannyverse767.gumroad.com/l/programmatic-seo-markdown-generator-python-20260929)
- 💎 **MR Studio All-Access Lifetime Vault (€39 One-Time):** **[Unlock All Current + Future Workflows & Engines](https://mannyverse767.gumroad.com/l/depoud)**

---

## 🎯 The Problem & Financial ROI

Manual regression testing fails to catch subtle visual misalignments, overlapping typography, broken responsive layouts, and z-index collisions across diverse viewports before deployment. Existing cloud-based visual testing services charge exorbitant recurring fees per screenshot comparison while relying on brittle pixel-diff thresholds that trigger false positives on minor font-rendering differences.

| Metric | ❌ Traditional Cloud SaaS | ✅ MR Studio Self-Hosted Blueprint |
| :--- | :--- | :--- |
| **Annual Cost** | $600 – $2,400 / year | **€14 One-Time** |
| **Data Privacy & IP** | Third-party cloud lock-in | **100% Self-Hosted on your own infra** |
| **Execution Limits** | Strict per-task / per-seat quotas | **Unlimited runs & full source ownership** |
| **Stack** | Closed black-box | **Type-Safe Python CLI + Pytest Suite** |

---

## 🛠️ Key Capabilities Included in the Package

- ✅ **Headless Playwright multi-viewport automated crawler capturing full-page and component DOM snapshots**
- ✅ **Gemini Vision API multimodal prompt engine identifying layout shifts, clipping, and CSS overlap with zero pixel-diff baseline required**
- ✅ **Automated structured markdown and JSON defect reports with precise bounding box coordinates and severity ratings**
- ✅ **Turnkey CI/CD CLI integration compatible with GitHub Actions, GitLab CI, and local pre-push git hooks**


---

## 📖 Technical Documentation & Setup Guide

# Autonomous Visual QA & Broken UI Bug Detector

A headless, multimodal command-line tool that autonomously identifies visual regressions, responsive layout collapse, typography clipping, and z-index collisions across web applications. 

By leveraging Playwright for deterministic browser automation and the Gemini Vision API for zero-baseline visual inspection, this tool eliminates brittle pixel-diff thresholds, anti-aliasing false positives, and the expensive subscription tiers typical of legacy cloud visual testing platforms.

---

## 1. Overview & Architecture

Traditional visual testing relies on pixel-to-pixel baseline comparisons (e.g., ImageMagick, PerceptualDiff). These implementations fail frequently in modern CI/CD pipelines due to minor cross-platform font rendering, smooth scrolling offsets, dynamic content, and sub-pixel anti-aliasing variations.

This tool captures rendered DOM layouts across standardized viewport matrices and applies multimodal reasoning via Google Gemini Vision to evaluate interface integrity the same way a human QA engineer would, but with mathematical consistency and bounding-box precision.

```
┌────────────────────────────────────────────────────────┐
│                   CLI Entrypoint                       │
│                     (main.py)                          │
└───────────┬────────────────────────────────┬───────────┘
            │                                │
            ▼                                ▼
┌───────────────────────┐        ┌───────────────────────┐
│ Playwright Controller │        │ Viewport Matrix Sync  │
│  - Headless Chromium  │        │  - Mobile (390x844)   │
│  - DOM Stabilization  │        │  - Tablet (768x1024)  │
│  - Lazy-load Flushing │        │  - Desktop (1920x1080)│
└───────────┬───────────┘        └───────────────────────┘
            │
            ▼
┌────────────────────────────────────────────────────────┐
│ Full-Page & Component Buffer Pipeline                  │
│  - In-memory PNG byte streaming (Zero local bloat)     │
└───────────┬────────────────────────────────────────────┘
            │
            ▼
┌────────────────────────────────────────────────────────┐
│ Gemini Vision Multimodal Inference Engine              │
│  - Zero-baseline semantic layout analysis              │
│  - CSS boundary clipping & z-index collision check     │
│  - Relative bounding-box localization [ymin,xmin,...]  │
└───────────┬────────────────────────────────────────────┘
            │
            ▼
┌────────────────────────────────────────────────────────┐
│ Artifact Exporter                                      │
│  - defect-report.json (Machine-readable / CI gate)     │
│  - defect-report.md   (Human-readable / PR comment)   │
└────────────────────────────────────────────────────────┘
```

### Key Functional Capabilities

* **Multimodal Layout Heuristics:** Zero-baseline visual inference flags actual defects—such as text cut off by `overflow: hidden`, floating elements occluded by misconfigured `z-index`, and responsive flex-wrap wrapping breaks—without needing a "golden master" image.
* **Deterministic DOM Hydration Engine:** Pre-capture routines automatically scroll to trigger lazy-loaded resources, monitor network state (`networkidle`), and wait for custom CSS/Webfont stabilization.
* **Normalized Defect Coordinates:** Visual defects are annotated with normalized spatial coordinates (`[ymin, xmin, ymax, xmax]`), enabling continuous integration tools to crop and highlight regressions directly in build artifacts.
* **Strict Return Code Gating:** Exits with code `1` or `0` based on user-defined severity thresholds (`CRITICAL`, `HIGH`, `MEDIUM`, `LOW`), making it a drop-in quality gate for deployment scripts.

---

## 2. Prerequisites & Environment Setup

### System Dependencies

* **Python:** Version 3.10 or higher.
* **Operating System:** Linux (Ubuntu 20.04+ recommended for CI), macOS 12+, or Windows Subsystem for Linux 2 (WSL2).
* **System Libraries (for Headless Chromium):**
  * `libnss3`
  * `libatk-bridge2.0-0`
  * `libxcomposite1`
  * `libxdamage1`
  * `libgbm1`
  * `libasound2`

### API Access

An active Google Cloud Project with the **Gemini API** enabled and an operational API key (`GEMINI_API_KEY`). Ensure your account quota allows for sufficient requests per minute (RPM) for parallel multi-viewport executions.

---

## 3. Installation & Quickstart

### Step 1: Blueprint Repository and Create Virtual Environment

```bash
git Blueprint https://github.com/your-org/visual-qa-detector.git
cd visual-qa-detector

python3 -m venv .venv
source .venv/bin/activate
```

### Step 2: Install Python Dependencies & Playwright Binaries

```bash
pip install --upgrade pip
pip install -r requirements.txt

# Install Playwright browser engines and system-level OS dependencies
playwright install --with-deps chromium
```

*(Sample `requirements.txt` includes: `playwright`, `google-genai`, `pydantic`, `python-dotenv`, `click`, `rich`)*

### Step 3: Run the CLI

Run a basic visual audit on a single URL:

```bash
python main.py --url https://example.com/pricing --threshold HIGH
```

Inspect multiple viewports with custom output directory:

```bash
python main.py \
  --url https://staging.example.com/checkout \
  --viewports desktop,mobile \
  --wait-until networkidle \
  --output-dir ./qa-artifacts \
  --threshold CRITICAL
```

---

## 4. Configuration Options

Runtime behavior is controlled via environment variables (loaded through `.env`) or overridden using CLI arguments.

### Environment Variables (`.env`)

Create a `.env` file in the root directory:

```ini
# Core API Credentials
GEMINI_API_KEY="AIzaSyYourGeminiAPIKeyGoesHere"

# Gemini Model Variant
# Recommended: gemini-2.5-flash for production speed, gemini-2.5-pro for deep structural audits
GEMINI_MODEL="gemini-2.5-pro"

# Network & Rendering Timeouts (milliseconds)
VISUAL_QA_NAV_TIMEOUT=45000
VISUAL_QA_STABILIZE_DELAY=2500

# Default Viewport Configurations (Width x Height)
VIEWPORT_DESKTOP="1920x1080"
VIEWPORT_TABLET="768x1024"
VIEWPORT_MOBILE="390x844"

# Quality Gate Defaults
DEFAULT_FAIL_SEVERITY="HIGH"
```

### CLI Command Options Reference

| Flag | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `--url` | `STRING` | *Required* | Absolute target URL to evaluate (must include protocol). |
| `--viewports` | `STRING` | `desktop,tablet,mobile` | Comma-separated list of target screen profiles. |
| `--threshold` | `CHOICE` | `HIGH` | Minimum severity triggering non-zero exit code: `LOW`, `MEDIUM`, `HIGH`, `CRITICAL`. |
| `--output-dir` | `PATH` | `./artifacts` | Target directory for generated `.json` and `.md` reports. |
| `--full-page` | `BOOLEAN` | `true` | When true, Playwright captures the entire scrollable DOM. |
| `--wait-until` | `CHOICE` | `load` | Playwright navigation lifecycle wait condition (`commit`, `domcontentloaded`, `load`, `networkidle`). |
| `--emulate-media` | `CHOICE` | `screen` | Emulate CSS media types: `screen` or `print`. |

---

## 5. Production Tips & Maintenance

### Deterministic Visual Captures (Flakiness Mitigation)

1. **Font Smoothing & CSS Animations:** Disable smooth scrolling, transitions, and dynamic SVGs before running captures. You can inject this script via Playwright's `page.add_init_script()`:
   ```css
   *, *::before, *::after {
     transition: none !important;
     animation: none !important;
     caret-color: transparent !important;
   }
   ```
2. **Network Idle Edge Cases:** Modern applications running continuous polling (WebSockets, long-polling metrics, or continuous telemetry) can keep the `networkidle` lifecycle event from resolving. In these scenarios, use `--wait-until load` alongside an explicit stabilization delay (`VISUAL_QA_STABILIZE_DELAY=2000`).
3. **Cookie Banners and Paywalls:** Handle authentication and consent overlays globally using a Playwright `storage_state.json` file to prevent modal layers from obscuring viewports and triggering false layout defect flags.

### CI/CD Implementation (GitHub Actions)

Add this quality gate to your `.github/workflows/visual-qa.yml`:

```yaml
name: Autonomous Visual QA Gate

on:
  pull_request:
    branches: [ main, develop ]

jobs:
  visual-regression:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
          cache: 'pip'

      - name: Install Dependencies
        run: |
          pip install -r requirements.txt
          playwright install --with-deps chromium

      - name: Execute Visual QA Inspection
        env:
          GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
        run: |
          python main.py \
            --url https://preview-${{ github.event.pull_request.number }}.internal.app \
            --viewports desktop,mobile \
            --threshold HIGH \
            --output-dir ./reports

      - name: Publish Defect Report Artifacts
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: visual-qa-report
          path: ./reports/
```

### Structured Output Schema

The tool writes raw structured validation results to `artifacts/defect-report.json`. Below is the data model used for programmatic ingestion:

```json
{
  "url": "https://staging.example.com/checkout",
  "timestamp": "2026-03-30T10:45:12Z",
  "status": "FAILED",
  "summary": {
    "critical": 1,
    "high": 0,
    "medium": 2,
    "low": 0
  },
  "defects": [
    {
      "id": "ERR_VIEWPORT_OVERFLOW_01",
      "viewport": "mobile (390x844)",
      "severity": "CRITICAL",
      "category": "LAYOUT_SHIFT_OVERFLOW",
      "description": "Primary checkout CTA button is pushed outside visible viewport boundaries due to non-wrapping flex child container.",
      "bounding_box": {
        "ymin": 810,
        "xmin": 45,
        "ymax": 890,
        "xmax": 345
      },
      "suggested_fix": "Apply 'flex-wrap: wrap' or review media-query container max-width settings at max-width: 480px."
    }
  ]
}
```

---

## 6. License & Support Info

### License

This project is open-source software licensed under the **Apache License, Version 2.0**. You may freely use, modify, and distribute this software in commercial or personal projects according to the terms and conditions outlined in the `LICENSE` file.

### Maintenance & Support

* **Issues & Feature Requests:** File dynamic rendering bugs, browser-engine panics, and feature requests directly in the GitHub Issue Tracker.
* **Multimodal Engine Prompts:** To adjust the underlying Gemini reasoning engine prompt matrices (e.g., custom enterprise design system compliance), edit the prompt templates found in `engine/prompts.py`.

---

## 🚀 Ready to Deploy in Under 5 Minutes?

Skip days of trial-and-error debugging. Grab the verified, production-tested package (including full source/JSON, `.env.example`, and automated test suite):

👉 **[Get Autonomous Visual QA & Broken UI Bug Detector — Python & Playwright CLI on Gumroad](https://mannyverse767.gumroad.com/l/wuncpo)** *(Promo code: `EARLYBIRD` for -20%)*
