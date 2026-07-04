# IBM Bob Build for Planning Analytics

> A hands-on demonstration guide showing how **IBM Bob** (AI assistant) accelerates build teams working on **IBM Planning Analytics (TM1)** — from architecture and TurboIntegrator development to migration, testing, documentation and deployment.

---

## Purpose

This repository is designed to demonstrate Bob's capabilities for **BUILD teams** responsible for implementing and maintaining IBM Planning Analytics environments.

Through **20 practical exercises** across **8 domains**, it shows concretely how Bob — connected to a live Planning Analytics server via MCP — reduces development time, improves quality, and handles tasks that traditionally require deep TM1 expertise.

---

## Main Documents

| Document | Language | Description |
|---|---|---|
| [`guide-bob-pa-build.md`](guide-bob-pa-build.md) | 🇫🇷 French | Complete guide — 20 exercises with real Bob outputs captured on the 24Retail demo server |
| [`guide-bob-pa-build-en.md`](guide-bob-pa-build-en.md) | 🇬🇧 English | English version of the same guide |
| [`guide-bob-pa-build-plan.md`](guide-bob-pa-build-plan.md) | 🇫🇷 French | Construction plan — breakdown of all 20 exercises by domain and subtask |
| [`guide-bob-pa-mcp-config.md`](guide-bob-pa-mcp-config.md) | 🇫🇷 French | Step-by-step MCP configuration guide to connect Bob to Planning Analytics |

---

## The 8 Domains Covered

| # | Domain | Exercises | What Bob demonstrates |
|---|---|---|---|
| 1 | **Architecture & Modelling** | 1.1, 1.2 | Reverse-engineer an existing model, propose a target architecture |
| 2 | **TurboIntegrator Development** | 2.1, 2.2, 2.3, 2.4 | Write, deploy and optimise TI processes (ETL, spread, multi-cube) |
| 3 | **TM1 Rules** | 3.1, 3.2, 3.3, 3.4 | Generate KPI rules, currency conversion, variance calculation, feeders |
| 4 | **Integration** | 4.1, 4.2 | SAP → PA mapping, robust ETL error handling |
| 5 | **Migration** | 5.1, 5.2 | Legacy model audit, structured migration plan |
| 6 | **Testing & Validation** | 6.1, 6.2 | Anomaly detection on live data, test case generation |
| 7 | **Documentation** | 7.1, 7.2 | Auto-generate architecture docs and TI process specifications |
| 8 | **Deployment** | 8.1, 8.2 | Deployment checklist, real-time server monitoring |

---

## Prerequisites

- **IBM Bob** v2.x ([Trial](https://www.ibm.com/products/bob) or Enterprise)
- Access to an **IBM Planning Analytics** environment (TechZone or SaaS)
- Bob connected to PA via MCP — see [`guide-bob-pa-mcp-config.md`](guide-bob-pa-mcp-config.md)

> The exercises use the **24Retail** demo server (available on IBM TechZone) as the reference environment. All example outputs in the guide were captured live on this server.

### MCP Configuration

Copy the template and fill in your credentials:

```bash
cp .bob/mcp.json.example .bob/mcp.json
# Edit .bob/mcp.json with your PA host URL, tenant ID and Base64 credentials
```

See [`guide-bob-pa-mcp-config.md`](guide-bob-pa-mcp-config.md) for the full setup walkthrough.

---

## Repository Structure

```
.
├── guide-bob-pa-build.md           # Main guide (FR) — 20 exercises
├── guide-bob-pa-build-en.md        # Main guide (EN) — 20 exercises
├── guide-bob-pa-build-plan.md      # Construction plan
├── guide-bob-pa-mcp-config.md      # MCP setup guide
├── sample-data/
│   ├── revenue_csv_sample.csv      # Sample CSV for exercise 2.1
│   └── sap_export_sample.csv       # Sample SAP export for exercise 4.1
├── .bob/
│   └── mcp.json.example            # MCP config template (no secrets)
└── BOB PA exo *.pdf                # PDF outputs of each exercise
```

---

## PDF Exercise Outputs

The repository also includes the **PDF output of each exercise**, showing the exact result produced by Bob during a live demo session:

| File | Exercise |
|---|---|
| `BOB PA exo 1.1 ...pdf` | Architecture reverse-engineering |
| `BOB PA exo 1.2 ...pdf` | Target architecture proposal |
| `BOB PA exo 2.1 ...pdf` | TI process — CSV load |
| `BOB PA exo 2.2 ...pdf` | TI process — seasonal spread |
| `BOB PA exo 2.3 ...pdf` | Supply Chain → Income Statement |
| `BOB PA exo 2.4 ...pdf` | Process optimisation |
| `BOB PA exo 3.2 ...pdf` | Currency conversion rules |
| `BOB PA exo 3.4 ...pdf` | TM1 Feeders |
| `BOB PA exo 4.1 ...pdf` | SAP → Revenue mapping |
| `BOB PA exo 4.2 ...pdf` | Robust ETL error handling |
| `BOB PA exo 5.1 ...pdf` | Migration audit |
| `BOB PA exo 5.2 ...pdf` | Migration plan |
| `BOB PA exo 6.1 ...pdf` | Revenue anomaly detection |
| `BOB PA exo 6.2 ...pdf` | Test plan — Copy_Revenue_Units |
| `BOB PA exo 7.1 ...pdf` | Revenue cube documentation |
| `BOB PA exo 7.2 ...pdf` | TI process specification |
| `BOB PA exo 8.1 ...pdf` | Deployment checklist |
| `BOB PA exo 8.2 ...pdf` | Post-deployment monitoring |

---

## About IBM Bob

[IBM Bob](https://www.ibm.com/products/bob) is IBM's AI assistant built on top of large language models, with native integration to IBM products via MCP (Model Context Protocol). When connected to Planning Analytics, Bob can read cube structures, explore data, write and deploy TI processes, and query the TM1 server — all from a conversational interface.

---

*Built with IBM Bob — Planning Analytics TechZone demo environment (24Retail)*
