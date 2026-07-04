# Demo guide — Bob × IBM Planning Analytics
## BUILD phase: 20 exercises for a project team
---

## Translation context and terminology policy

This document is a BUILD-phase demonstration guide for IBM Planning Analytics projects. It is a technical enablement asset for consultants, TI developers, PA architects, and project teams who want to demonstrate how IBM Bob can accelerate model analysis, TurboIntegrator development, TM1 rules, integration, migration, testing, documentation, and deployment activities on the 24Retail reference server.

The Official tested document is the French version. 

The translation follows these rules:

- Product names, server names, cube names, dimension names, member names, process names, API/MCP tool names, code identifiers, MDX/TI functions, JSON keys, URLs, and file paths are intentionally preserved.
- BUILD and RUN are kept as project lifecycle terms.
- TurboIntegrator is kept as the IBM product term; "TI process" is used where the French source says "TI processes".
- "Rules" means TM1 cube rules; "feeders" is kept as the TM1 technical term.
- "Actual", "Forecast", "Version 1", "Variance", "Gross Revenue", "Gross Margin %", and other TM1 members are not translated because they are real model members on 24Retail.

### Translation glossary used

| French source term | English rendering | Notes |
|---|---|---|
| équipe BUILD | BUILD team | BUILD kept as lifecycle term |
| RUN quotidien | day-to-day RUN operations | RUN kept as lifecycle term |
| Planning Analytics / PA | Planning Analytics / PA | IBM product name |
| TM1 | TM1 | Product/runtime term |
| TurboIntegrator / TI | TurboIntegrator / TI | IBM technical term |
| TI processes | TI process | Standard PA wording |
| TM1 rules | TM1 rules | Cube rules |
| feeders | feeders | TM1-specific term, not translated |
| cube / dimension / membre | cube / dimension / member | PA model terms |
| serveur 24Retail | 24Retail server | Reference server name preserved |
| Reference result | Reference result | Exercise section label |
| Prompt to run | Prompt to run | Exercise section label |
| Avec Bob / Sans Bob | With Bob / Without Bob | Exercise comparison labels |
| actuals / réalisés | actuals | Business planning term |
| prévisions | forecasts | Business planning term |
| saisie | input | For input members/data entry |
| valeurs aberrantes | outliers | Analytics term |
| recette | UAT / test environment | Rendered by context |
| mise en production | production deployment / go-live | Rendered by context |

---

## Table of contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [However to use this guide](#how-to-use-this-guide)
- [Domain 1 — Architecture & Modeling](#domain-1--architecture--modeling)
- [Domain 2 — TurboIntegrator Development](#domain-2--turbointegrator-development)
- [Domain 3 — TM1 Rules](#domain-3--tm1-rules)
- [Domain 4 — Integration](#domain-4--integration)
- [Domain 5 — Migration](#domain-5--migration)
- [Domain 6 — Testing & Validation](#domain-6--testing--validation)
- [Domain 7 — Documentation](#domain-7--documentation)
- [Domain 8 — Deployment](#domain-8--deployment)
- [Conclusion](#conclusion)
- [Appendix A — PA/TM1 Glossary](#appendix-a--patm1-glossary)
- [Appendix B — Available MCP tools](#appendix-b--available-mcp-tools)
- [Appendix C — Blank exercise template](#appendix-c--blank-exercise-template)
- [Appendix D — 24Retail server structure](#appendix-d--24retail-server-structure)
- [Appendix E — Sample CSV files](#appendix-e--sample-csv-files)

---

## Introduction

This guide is intended for **IBM Planning Analytics BUILD teams** — consultants, TI developers, and PA architects involved in delivering a planning solution for an enterprise customer.

Its goal is to demonstrate concretely how **Bob, IBM’s AI assistant**, accelerates and secures each step of a PA project: from dimensional model design through production deployment.

### What this guide is not

This is not a PA administration guide for day-to-day RUN operations. All the exercises are grounded in the **reality of a project**: incomplete specifications, legacy models to audit, TI code to write from scratch, complex rules to generate, migrations to plan.

### Reference server

All exercises are based on the server **24Retail** — a real IBM Planning Analytics model, accessible through the MCPs `pa-tz-server` and `pa-tz-cube`. The reference outputs were produced live on this server.

### Format of each exercise

Each exercise contains eight sections :

| Section | Content |
|---|---|
| 🎯 **Objective** | What the participant learns / demonstrates |
| 📋 **Context** | The BUILD situation in which this exercise occurs |
| 💬 **Prompt to run** | The exact text to copy into Bob |
| 🔢 **Steps** | The step-by-step flow of the interaction |
| ✅ **Reference result** | The actual output produced by Bob on 24Retail |
| ⚡ **With Bob** | Time, automation, quality level |
| 🐢 **Without Bob** | Time, required tools, friction points, risks |
| 💡 **Discussion points** | Questions to facilitate the post-exercise discussion |

---

## Prerequisites

### Step 0 — Get Bob

Before anything else, you need access to IBM Bob. There are two options depending on your context :

---

#### Option A — Bob Trial (recommended to get started)

> **Ideal for**: exploring Bob alone, preparing a demo, testing this guide independently.

Go to **[bob.ibm.com/trial](https://bob.ibm.com/trial)** → *Start Your Free Trial*

- **40 Bobcoins** included for **30 days**
- Immediate access — no approval required
- Full capabilities included (MCP, modes, tools)
- Sufficient for all exercises in this guide

> ℹ️ **Bobcoins**: IBM Bob consumption unit. 40 coins more than cover the 20 exercises in this guide.

---

#### Option B — Bob Enterprise via IBM TechZone (for a customer POC or workshop)

> **Ideal for**: customer demo, team workshop, POC with real data, IBM/partner engagement.

Two sub-options are available :

**B1 — TechZone Managed Account (user access)**
Option **recommended** for the workshops, demos and enablement sessions.
- Centrally managed by TechZone — no administration required
- Faster provisioning
- Usage: workshops, POCs, training sessions
- Budget Bobcoins: 50–100 coins/user recommended (packs of 1 000)

**B2 — Dedicated Instance (admin access)**
Use only if admin control, isolation, or advanced configuration is required.
- Additional lead time (+1 week minimum)
- Higher cost and operational overhead

**However to request access :**
1. Check eligibility: a **Opportunity ID** valid is required — exploratory or test requests are not accepted
2. Submit a request **at least 7 days in advance** through a *TechZone Support Case*
3. Processing time: up to **5 business days** for review + 1 day for provisioning (manual steps)
4. Official IBMers reference: [TechZone Collection — Bob Enterprise Onboarding](https://techzone.ibm.com/collection/69bac0f2c5a26dc2f7881d2d)

> ⚠️ **Partners**: access cannot be requested directly. An internal IBM sponsor (Global Sales or Partner Ecosystem) must submit the request and provide the full list of partner emails.

> ℹ️ **When in doubt**: start with a **TechZone Managed Account** — it is sufficient for the vast majority of the POCs and workshops. Escalate to a Dedicated Instance only if the requirements cannot otherwise be met.

---

### Step 1 — Reserve the TechZone environment

The 24Retail server is hosted on **IBM Technology Zone**. The reservation takes less than 5 minutes.

1. Go to: **[IBM Business Analytics Demo Server](https://techzone.ibm.com/collection/5f7229b4529ea0001e4c19b5)**
2. Choose the collection **"IBM Business Analytics Demo Server"** — version **2.1.20**
3. Click **"Reserve"** and choose a time slot *(immediate access possible)*
4. Wait for the confirmation email — it contains the **URL** of the server and the **Tenant ID**

> ℹ️ This collection is IBM’s official demo platform for Planning Analytics and Cognos Analytics, used in presales by IBM and its partners.

**Access credentials (valid for all servers in the collection) :**

| Item | Value |
|---|---|
| **Username** | `pm` |
| **Password** | `IBMDem0s` *(shortcut Ctrl+P in the PA interface)* |
| **URL PA Host** | `http://<region>.services.cloud.techzone.ibm.com:<PORT>` *(in the TechZone email)* |
| **Tenant ID** | Provided in the email (ex. `EZXHLSXGX9VK`) |

---

### Step 2 — Encode the credentials in Base64

The MCP authentication header uses Base64 encoding in the format `username:password`.

In a terminal :

```bash
echo -n "pm:IBMDem0s" | base64
# Result: cG06SUJNRGVtMHM=
```

Keep this result — it will be used in the JSON configuration in the next step.

> **SaaS IBM Cloud**: the format is `apikey:<VOTRE_CLE_API>` (key generated from the IBM Cloud console).

---

### Step 3 — Install the MCP in Bob

**Option A — Through the Bob interface (recommended)**

1. Click **⚙️ Settings** (upper-right corner)
2. In the left column, click on **MCP**
3. Click **Install** next to **Planning Analytics**
4. Fill in the form :

| Field | Value (sample TechZone) |
|---|---|
| **Planning Analytics Host URL** | `http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/` |
| **Tenant ID** | `EZXHLSXGX9VK` |
| **Authorization value** | `Basic cG06SUJNRGVtMHM=` |

> ⚠️ The field Authorization **must** include the prefix `Basic ` (including the space).

5. Click **Install**, then go to step 4 to correct the URL if necessary.

**Option B — Direct editing of the file `.bob/mcp.json`**

Create or edit `.bob/mcp.json` in the workspace folder (scope: this project only) :

```json
{
  "mcpServers": {
    "pa-tz-cube": {
      "type": "streamable-http",
      "url": "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/cube/mcp",
      "headers": {
        "Authorization": "Basic cG06SUJNRGVtMHM="
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_cubes_that_may_answer_query",
        "get_available_tm1_servers",
        "get_data_from_data_explorer",
        "lookup_potential_members",
        "get_cube_sample_members"
      ],
      "disabled": false
    },
    "pa-tz-server": {
      "type": "streamable-http",
      "url": "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/analysis/mcp",
      "headers": {
        "Authorization": "Basic cG06SUJNRGVtMHM="
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_available_tm1_servers"
      ],
      "disabled": false
    }
  }
}
```

**Replace in the config :**
- `eu-de.services.cloud.techzone.ibm.com:31497` → the URL+port of your TechZone reservation
- `EZXHLSXGX9VK` → your Tenant ID (in the TechZone email)
- `cG06SUJNRGVtMHM=` → your Base64 result of step 2

**Exposed MCP endpoints :**

| Server | Endpoint | Usage |
|---|---|---|
| `pa-tz-cube` | `/v0/agentic-ai/cube/mcp` | Cube, dimension, data, MDX, and outlier exploration |
| `pa-tz-server` | `/v0/agentic-ai/analysis/mcp` | TI processes, server threads, execution logs |

> **Why `timeout: 600000`?** TechZone servers can take 15–30 seconds to respond. Bob’s default value (60 seconds) is often insufficient. 600 000 ms = 10 min.
>
> **Why `alwaysAllow`?** These tools are invoked very frequently. Listing them here avoids manual confirmation on every call.

---

### Step 4 — Correct the URL if necessary (Bob v2 only)

Bob v2 sometimes generates a malformed URL after installation through the form (path duplication, `https://` before `http://`). If the servers appear as `Disconnected`, manually edit `.bob/mcp.json` with the configuration from step 3 Option B.

---

### Step 5 — Restart Bob and verify

1. **Close** Bob completely (Cmd+Q on Mac)
2. **Restart** Bob
3. Go in **Settings → MCP**

Both servers should appear with the status 🟢 **Connected** :

| Name | Status | Scope |
|---|---|---|
| TM1 Cube Service (`pa-tz-cube`) | 🟢 Connected | Workspace |
| AnalysisServer (`pa-tz-server`) | 🟢 Connected | Workspace |

**Functional test :**

In a new Bob conversation :
```
List the available TM1 servers
```
Bob calls `get_available_tm1_servers` and returns the list. `24Retail` must be present.

```
Give me the list of cubes on the 24Retail server
```
Bob should answer with ~34 cubes including `Revenue`, `Income Statement`, `Supply Chain`.

---

### MCP troubleshooting

| Problem | Likely cause | Solution |
|---|---|---|
| `Disconnected` after Install | URL malformed by Bob v2 | Edit `.bob/mcp.json` manually (Option B) |
| `Request timed out` | Timeout too short | Go `timeout` to `600000` |
| `401 Unauthorized` | Incorrect Base64 encoding | Re-encoder: `echo -n "pm:IBMDem0s" \| base64` |
| `403 Forbidden` | Insufficient rights | Check the entitlement **Planning Analytics Agent** in the IBM console |
| `Failed to connect` | URL / port inaccessible | Check VPN, network, and that the port is open |
| `24Retail` absent of the list | Wrong Tenant ID in the URL | Check that the URL contains the correct Tenant ID |

**Check that the server responds (curl) :**

```bash
curl -v \
  -H "Authorization: Basic cG06SUJNRGVtMHM=" \
  "http://eu-de.services.cloud.techzone.ibm.com:31497/api/EZXHLSXGX9VK/v0/agentic-ai/cube/mcp"
```

Expected response: `HTTP/1.1 200 OK` with `content-type: text/event-stream`

**Configuration file locations :**

| Scope | Path | Effect |
|---|---|---|
| **Workspace** | `.bob/mcp.json` | Available only in this project |
| **Global** | `~/.bob/settings/mcp.json` | Available in all Bob conversations |

---

### Configuration for IBM PA SaaS (IBM Cloud)

If you use **IBM Planning Analytics as a Service** instead of TechZone :

```json
{
  "mcpServers": {
    "pa-saas-cube": {
      "type": "streamable-http",
      "url": "https://us-east-1.planninganalytics.saas.ibm.com/api/{TENANT_ID}/v0/agentic-ai/cube/mcp",
      "headers": {
        "Authorization": "Basic {BASE64_DE_apikey:VOTRE_CLE_API}"
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_cubes_that_may_answer_query",
        "get_available_tm1_servers",
        "get_data_from_data_explorer",
        "lookup_potential_members",
        "get_cube_sample_members"
      ],
      "disabled": false
    },
    "pa-saas-server": {
      "type": "streamable-http",
      "url": "https://us-east-1.planninganalytics.saas.ibm.com/api/{TENANT_ID}/v0/agentic-ai/analysis/mcp",
      "headers": {
        "Authorization": "Basic {BASE64_DE_apikey:VOTRE_CLE_API}"
      },
      "timeout": 600000,
      "alwaysAllow": [
        "get_available_tm1_servers"
      ],
      "disabled": false
    }
  }
}
```

> The Tenant ID is found in the URL of PA Workspace :
> `https://us-east-1.planninganalytics.saas.ibm.com/?accountId=XXXX&tenantId=TENANT_ID`
> The API key is generated from the **console IBM Cloud → Manage → Access → API Keys**.

### Expected PA knowledge

The exercises cover three levels :
- 🟢 **Beginner** — notions of cube, dimension, members
- 🟡 **Intermediate** — TurboIntegrator, basic rules
- 🔴 **Advanced** — feeders, multi-source ETL, migration

Each exercise indicates its level in the header.

### Timing

- ~15 minutes per individual exercise
- ~3 hours for a workshop covering the 8 domains (key exercises only)
- ~6 hours for the full guide

---

## However to use this guide

1. **Read the exercise statement** of the exercise (Objective + Context)
2. **Copy the prompt** and run it in Bob
3. **Observe the result** produced by Bob
4. **Compare** with the the guide’s reference result
5. **Discuss** the discussion points with the team

> **Note on comparison**: the outputs may differ slightly depending on the execution date (changing data, server state). What matters, it is the **structure and the quality** of the response, not the exact value of each cell.

---

## ⚠️ Server constraints observed during execution

> This section is built progressively during live testing on the server TechZone 24Retail. It centralizes all discovered syntax and architecture constraints in deploying the exercises — to consult before writing TI code or TM1 rules on this environment.

### TurboIntegrator syntax

| Constraint | Symptom | Validated correction |
|---|---|---|
| `ROUND(x, 0)` | Parsing syntax error | Use `INT(x)` |
| `IF( a @= 'x' % a @= 'y' % a @= 'z' )` with `ProcessBreak()` | Passes syntax validation but **does not evaluate correctly to the runtime** — the ProcessBreak does not trigger | Use of the **IF/ELSE nested** on `@=` |
| `IF( strVar @= 'val' )` on a local string variable (not a parameter) | Invalid — syntax error | Use `IF @= 'valeur'` directly on the parameter |
| `IF( numParam <= 0 )` | Invalid | Restructure with `IF( param = 0 )` or `ELSE` |
| `@<>` string comparison operator | Invalid on this server | Use nested IF/ELSE blocks on `@=` |
| `Now(1)` | `Unexpected parenthesis` | Use `TM1User()` alone for the logging, or `Now()` without argument |

### Cube architecture 24Retail

| Constraint | Impact |
|---|---|
| **Income Statement** is fully calculated by TM1 rules | `CellPutN` returns `Rule applies to cell` on all accounts — this cube cannot receive direct writes through a TI process |
| **Revenue** — members `Gross Revenue`, `Cost of Sales`, `Gross Margin`, `Gross Margin %`, `Direct COGS`, `Indirect COGS` are calculated by TM1 rules | Seuls `Units Sold`, `Unit Price`, `Unit Cost` are of the input members |
| Dimension `Version` — calculated versions (do not write) | `Variance`, `Variance%`, `Rate Variance`, `Volume Variance`, `Ccy Exchange Variance`, `Performance Variance`, `Target vs Prior Year Actual`, `Target vs Prior Year Actual %` |
| Cube `Exchange Rates` — dimension `Currency` contains `USD`, `CDN`, `GBP` only | Revenue does not have a Currency dimension — currency conversion must be done by organization (orgs 4xx = Canada = CDN) |
| **Actual inventory 24Retail** | 34 cubes in total (including 1 METRICS, 33 with active rules); `Forecast_Example` is the only cube without rules |

### MCP tool behavior

| Behavior | Explanation |
|---|---|
| `get_tm1_server_process_execution_error_logs` returns "Unable to locate" on a successful run | Normal — no error file created if the process ends without error or ProcessBreak in Prolog |
| `get_tm1_server_process_execution_error_logs` returns the previous previous run’s log | The tool returns the **last error file created**, not necessarily that of the latest run |
| `execute_mdx_and_get_view` returns `Failed to encode cube view` | The dimension name in the MDX is incorrect — check the exact name with `get_cube_dimensions` |
| TM1 rules cannot be deployed through MCP | The TM1 REST API does not expose cube rule editing — use TM1 Architect or PA Modeler |
| The ASCII datasource of a TI process cannot be configured through MCP | The TM1 REST API does not expose datasource configuration — use PA Modeler (Data Source → File) |
| `perform_outlier_detection` returns HTTP 500 on the cube Revenue 24Retail | This cube is not pre-analyzed by the AI engine — anomaly analysis must be done manually through comparative MDX |
| `get_cube_sample_members` and `lookup_potential_members` retournent 404/400 on `24Retail - Collie` | These tools require the server named `24Retail` (without suffixe) — always use the short server name |

### TechZone Remote Desktop access

| Constraint | Workaround |
|---|---|
| The RDP URL RDP (`eu-de.services.cloud.techzone.ibm.com:49681`) may return a blank page in the browser | Use a native RDP client (Microsoft Remote Desktop on Mac/Windows) |

---

## Domain 1 — Architecture & Modeling

> **Relevant BUILD role**: Architecte PA, Consultant senior
> **Situation**: Project start — the current state must be understood and a target model must be proposed

### Exercise 1.1 — Reverse-engineering an existing model

> 🟢 Level: Beginner | ⏱ ~10 min

#### 🎯 Objective
Demonstrate that Bob can analyze an unknown TM1 server and produce a structured overview of its architecture — without access to TM1 Architect.

#### 📋 Context
You arrive on a migration project. The customer has been using 24Retail for five years. Documentation does not exist. You need to understand the model in less than an hour before the first workshop with the customer.

#### 💬 Prompt to run

```
Analyze the complete structure of the 24Retail server and give me an overview
data model architecture: cubes, dimensions, types, relationships between cubes,
and key TI processes. Present the result as a structured report.
```

#### 🔢 Steps
1. Copy the prompt above in Bob
2. Bob queries the MCPs `pa-tz-cube` and `pa-tz-server` to collect metadata
3. Bob product a structured architecture report
4. Compare with the reference result below

#### ✅ Reference result

**Architecture report — Server 24Retail** *(executed live)*

**Model overview**
The server 24Retail is a complete IBM Planning Analytics model dedicated to the financial and commercial planning of an international retail group. It includes **34 cubes**, several dozen dimensions and **62 TI processes**.

**Main identified cubes**

| Cube | Rules | Drillthrough | Role |
|---|---|---|---|
| Revenue | ✅ | ✅ | Revenue planning by product/channel/region |
| Income Statement | ✅ | ✅ | Consolidated P&L by entity |
| Supply Chain | ✅ | — | Supply chain costs by product |
| Capital | ✅ | ✅ | Assets and capex |
| Employee | ✅ | ✅ | Headcount and payroll |
| Exchange Rates | ✅ | — | Multi-currency exchange rates |
| Compensation | ✅ | — | Compensation and benefits |
| Depreciation | ✅ | — | Depreciation |
| Allocation Calculation | ✅ | — | Cost allocation |

**Cube Revenue — Dimensional model**
- 7 dimensions: `organization` × `Channel` × `product` × `Month` × `Year` × `Version` × `Revenue`
- Organization: 15 entities (Total Company → 3 regions USA + Canada → 11 sub-entities)
- Channel: 3 members (10/Retail, 20/Internet, 30/Distribution)
- Product: ~40 SKUs (Phones, PCs, Tablets) organized in a hierarchy
- Month: Jan–Dec + consolidations Q1–Q4 + Year + YTD + FcstMonth
- Version: Budget (Version 1), Actual, Forecast, Predictive, Variance, Variance%…
- Measures Revenue: Units Sold, Unit Price, Unit Cost, Gross Revenue, Cost of Sales, Gross Margin, Gross Margin %

**Key TI processes identified**

| Process | Key parameters | Role |
|---|---|---|
| `Copy_Revenue_Spread_Units` | pOrg, pChannel, pCurrentProduct, pYear, pUnitFcst | Monthly spread of forecasts |
| `Copy_Revenue_Compare_Units` | pCurrentProduct, pUnitFcst, pChannel | Forecast comparison |
| `Add_Product` | pParent, pNewNumber, pNewName | Add a product |
| `copy_version` | pFromVersion, pToVersion, pCubeName | Version copy |
| `UpdateISFromRevenue` | — | Update Income Statement from Revenue |
| `Load_PYActuals` | — | Load prior-year actuals |
| `reset_demo` | pZeroTarget | Reset the demo model |

**Detected inter-cube dependencies**
- Revenue → Income Statement (via `UpdateISFromRevenue` + rules)
- Supply Chain → Income Statement (costs supply aggregated)
- Exchange Rates → Income Statement (conversion of currency via rules)
- Revenue Assumptions → Revenue (price/cost assumptions)

**Architecture note**: The model uses shared dimensions (`organization`, `Month`, `Year`, `Version`) between several cubes — good consistency practice. The cube Metrics (type METRICS) is used for system indicators.

#### ⚡ With Bob
- **Time**: ~2 minutes
- **What is automated**: metadata collection, structuring, identification of relationships between cubes
- **Quality**: deliverable report, based on real server data

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (manual navigation in TM1 Architect cube by cube)
- **Required tools**: TM1 Architect or TM1 Developer, network access to the server
- **Risks**: omissions, transcription errors, partial documentation

#### 💡 Discussion points

**Q: What information is missing in this report that Bob cannot provide?**
> The source code of the rules (.rux) and TI processes is absent — Bob sees the metadata (parameters, names) but not the internal logic. Also missing are the Chores (scheduled tasks), the saved subsets and the users' private views. Workaround: ask the customer to export ses .rux and .pro and notte them into the chat for an in-depth analysis.

**Q: However to use this report to prepare the scoping workshop with the customer?**
> This report serves as the basis for the current-state mapping workshop. It allows the team to identify from day one: critical vs. secondary cubes, the dependencies that will determine the development order, and technical risks to present to the customer. In practice: print the cube table, annotate the dependencies with the customer, and identify the processes that require deeper analysis.

---

### Exercise 1.2 — Target architecture proposal

> 🔴 Level: Advanced | ⏱ ~15 min

#### 🎯 Objective
Show that Bob can, from an existing model and an expressed business requirement, propose an optimized TM1 architecture for a new cube.

#### 📋 Context
The customer wants a consolidated reporting cube that combines Revenue data and Supply Chain to produce a P&L complete. You must propose an architecture before the design workshop.

#### 💬 Prompt to run

```
Based on the structure of the Revenue and Supply Chain cubes in 24Retail,
propose an optimized architecture for a new "Consolidated P&L" cube that
combines these two sources. Include: recommended dimensions, dimension order
(justified by sparsity), key members, and the TI processes needed to feed it.
Highlight the architecture risks.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob reads the structures of the cubes Revenue and Supply Chain
3. Bob product a reasoned architecture proposal
4. Compare with the result of reference

#### ✅ Reference result

**Proposal architecture — Cube "Consolidated P&L"** *(based on the real structures 24Retail — live MCP data)*

**Source cube analysis**

*Revenue* (7 dim): `organization` × `Channel` × `product` × `Month` × `Year` × `Version` × `Revenue`
*Supply Chain* (7 dim): `organization` × `Channel` × `product` × `Month` × `Year` × `Supply Chain Measures` × `Version`

> **Note**: Supply Chain does **not** have a `Currency Calc`. The dimension `Currency Calc` of the Income Statement contains only **2 real members**: `Local` and `Base`. Currencies (USD, CDN, GBP) are in the cube `Exchange Rates` (dimension `Currency`), accessible through rules DB().

**Proposed architecture for the cube "P&L Consolidated"**

```
Recommended dimensions (order optimized for sparsity — increasing cardinality):
1. Currency Calc   — 2 members (Local, Base) — ultra-dense, same pattern as Income Statement
2. Version         — 20 members including 7 calculated versions (Variance, Rate Variance…)
3. Year            — 4 leaf years (Y1-Y4) + variances
4. organization    — 11 leaf members + 4 regions + Total Company
5. Month           — Jan-Dec + Q1-Q4 + Year + YTD + FcstMonth
6. Channel         — 3 leaf members (10 Retail, 20 Internet, 30 Distribution)
7. product         — ~30 real SKUs (5G/6G Phones, PCs, Laptops, Gaming, Tablets) — the sparsest
8. P&L Measures    — new dimension merging Revenue + Supply Chain Measures (~19 members)
```

**Dimension P&L Measures (to create) — real members from the server**
```
P&L Measures
├── Revenue (source: cube Revenue)
│   ├── Units Sold            (alias: Volume - Units)
│   ├── Unit Price            (alias: Unit Net Sales Price)
│   ├── Unit Cost             (alias: Unit Direct Cost)
│   ├── Gross Revenue
│   ├── Cost of Sales         (alias: Total COGS)
│   ├── Gross Margin
│   └── Gross Margin %
├── Supply Chain (source: cube Supply Chain)
│   ├── SC Direct Material
│   ├── SC Raw Material
│   ├── SC Packaging + Pallets
│   ├── SC Direct Labor
│   └── SC Total COGS Regional
├── Derived margins (calculated by TM1 rules — never by TI)
│   ├── Contribution Margin
│   ├── Contribution Margin %
│   └── COGS Variance (Rev vs SC)
└── Control
    └── Data Completeness Flag  (0=complete / 1=missing SC data)
```

> ⚠️ **SC.Units Sold to exclude**: Supply Chain also contains a member `Units Sold`. In the TI load SC, ignore this member — the authoritative value is `Revenue.Units Sold`, loaded first.

**Justification dimension order**
- `Currency Calc` in position 1: minimal cardinality (2 members), ultra-dense — first so that the rules of conversion apply first, as in Income Statement
- `Version` in position 2: 20 members, enables version locks and aligns with `pVersionDimLoc=2` for `copy_version`
- `organization` in position 4 (not in 1): 15 members total — less dense than Currency Calc and Version
- `product` in position 7: ~30 SKUs, the sparsest — not all SKUs are active on all the channels/orgs → minimizes memory footprint
- `P&L Measures` in position 8 (last): heterogeneous measures Revenue+SC — always in the last position in TM1

**Process TI required to feed it**
1. `PLConsolide_Load_Revenue`: copies the measures Revenue from the cube Revenue (vue `zUpdateIS` as the reading model). Parameters: `pVersion`, `pYear`, `pClearFirst`
2. `PLConsolide_Load_SupplyChain`: copies the costs from Supply Chain (vue `DetailCOGSdrillView`). Writes only to `Currency Calc = "Local"`. Parameters: `pVersion`, `pYear`
3. `PLConsolide_Copy_Version`: extension of `copy_version` with `pNumberDims=8`, `pVersionDimLoc=2`, `pStringDimLoc=5` — **never call `copy_version` directly on this cube**
4. `PLConsolide_Refresh_All`: orchestrator — Clear → Load_Revenue → Load_SupplyChain → Data Completeness Flag. Parameters: `pVersion`, `pYear`

> ❌ **Anti-pattern to avoid**: do not create of TI `Calc_PL_Derived` for derived KPIs. In TM1, the margin calculations (Gross Margin %, Contribution Margin) are performed **in TM1 rules + feeders**, not in TI. A TI of calcul fige the results in data, loses dynamic updates and doubles stored volume.

**Existing reusable views (cube Revenue)**
- `zUpdateIS` — reading template for the TI load Revenue
- `GM Report by Organization_SKU` — validation Gross Margin post-load
- `Product Profitability by Channel` — validation Contribution Margin by channel/SKU
- `DetailCOGSdrillView` (Supply Chain) — source for the TI load SC

**Identified architecture risks**
- 🔴 **Currency versions without equivalent SC**: the members `Ccy Exchange Variance` and `Rate Variance` of the dimension Version are calculated in relation to Currency Calc — Supply Chain has no currency. Explicitly exclude these versions from the scope of the TI `PLConsolide_Load_SupplyChain`
- 🔴 **Currency Calc = 2 members, not 3**: conversion USD/CDN/GBP goes through `Exchange Rates` via DB() — do not create currency members in Currency Calc (common architecture error)
- ⚠️ **Double source for Units Sold**: Revenue and Supply Chain both have a member `Units Sold` — ignore SC.Units Sold in the TI load SC, Revenue is authoritative
- ⚠️ **pVersionDimLoc to reconfigure**: `copy_version` existing has `pVersionDimLoc=6` (for Revenue). In P&L Consolidated, Version is in dim 2 → `pVersionDimLoc=2` mandatory, otherwise silent data corruption
- ⚠️ **Sparsity Gaming × Distribution**: the SKUs Gaming (XTR 9300/9500/9800) are probably absent of the channel Distribution — sparsity ~40-50% on product × Channel
- ⚠️ **FcstMonth and special members of Month**: load SC only in Jan–Dec, not in FcstMonth/Description/RowFormat

#### ⚡ With Bob (active MCP access)
- **Time**: ~10 minutes
- **What is automated**: live reading of real members, identification of collisions (Units Sold), detection of configuration risks (pVersionDimLoc), list of existing reusable views
- **Quality**: proposal based on real server data — exact members, confirmed cardinalities, identified existing views and TI processes

#### 🐢 Without Bob
- **Time**: 3 to 8 hours (design workshop + architecture document drafting)
- **Required tools**: TM1 Architect, Excel for the model dimensional, Word for the doc
- **Risks**: errors of modeling, omission of Currency Calc, wrong dimension order, anti-pattern TI of calcul → performance and consistency issues in prod

#### 💡 Discussion points

**Q: Did Bob identify the right architecture risks?**
> Yes, and with active MCP access, Bob identifie risks impossible to detect without real data: the collision `Units Sold` between the two source cubes, the trap `pVersionDimLoc=6` of the TI `copy_version` existing that would silently corrupt data if not reconfigured, and the members of version calculated (Ccy Exchange Variance) incompatible with Supply Chain. Howeverever, Bob cannot identify the risks related to the organizational constraints (who enters what, in which order) — this information requires workshops with business users.

**Q: What would a senior PA architect exchange in this proposal?**
> Two likely points: (1) they would empirically validate the sparsity product × channel before freezing the dimension order — `product` in dim 7 is theoretically correct but should be checked if consolidations by product are very frequent in reporting; (2) they would challenge the need of a cube consolidated vs. a cross-view Revenue/Supply Chain via rules DB() in Income Statement Reporting, which avoids the data duplication and the double maintenance.

**Q: However does this proposal accelerate the workshop of design?**
> Instead of starting from a blank page, the team enters the workshop with a reasoned proposal and based on real data of the server. The workshop shifts from "what do we put in it?" to "do we validate this proposal or modify it?". Design time is reduced of ~50% because discussions focus on specific points (order of the dims, management of the versions currency, pVersionDimLoc) rather than on the overall structure.

---

## Domain 2 — TurboIntegrator Development

> **Relevant BUILD role**: TI Developer, Technical Consultant
> **Situation**: Development sprint — it must write, test and deploy TI processes

### Exercise 2.1 — ETL: load from a CSV file

> 🟡 Level: Intermediate | ⏱ ~15 min

#### 🎯 Objective
Show that Bob generates a TI complete load process from a CSV file, with validation data and error handling — in a single interaction.

#### 📋 Context
Every week, you receive a CSV file of the sales real from the POS system. You must create a TI process to load it into the cube Revenue of 24Retail.

#### 💬 Prompt to run

```
Write and deploy on the 24Retail server a TurboIntegrator process named
"Load_Revenue_FromCSV" to load data from a CSV file into the Revenue cube.
The CSV file has the following columns:
Organization, Channel, Product, Month, Year, Version, UnitsSold, UnitPrice, UnitCost.
The process must: validate that parameters are not empty, ignore header rows,
log the number of loaded rows, and use ProcessBreak if a critical error is detected.
Provide the Prolog, Data, and Epilog code separately, then deploy the process on 24Retail.
```

> 📁 **File CSV sample**: [`sample-data/revenue_csv_sample.csv`](sample-data/revenue_csv_sample.csv) — 55 lines representing 5 organisations × 3 products × 12 months (Y1, Forecast). TO use as data of test for this process.

> ⚠️ **Boundary MCP — datasource CSV**: The MCP deploy the Prolog, the Epilog and the parameters. The **configuration datasource ASCII** (source of the file, delimiter, mapping of the columns V1–V9) is not accessible via MCP — it must be completed in **TM1 Architect or PA Modeler** (see section "Finalization via GUI" below).

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob generates the code TI complete (Prolog + Data + Epilog)
3. Bob creates the process on 24Retail (`create_tm1_process`)
4. Bob deploy Prolog + Epilog + parameters (`update_tm1_process`) — `syntax_errors: []`
5. **Finalize the datasource in TM1 Architect** (see section dedicated below)
6. Inspect the code product: validation, logic load, error handling
7. Go to the section Verification on the server
8. Compare with the result of reference

#### ✅ Reference result

**Code TI generated by Bob — Process load CSV → Revenue** *(code deployable, based on the dimensions and real members 24Retail — live MCP data)*

**Parameters of the process :**

| Parameter | Type | Value by default | Prompt |
|---|---|---|---|
| `pFilePath` | String | `revenue_data.csv` | Path complete of the CSV file |
| `pVersion` | String | `Forecast` | Version target (ex: Actual, Forecast, Version 1) |
| `pDelimiter` | String | `,` | Delimiter CSV |
| `pClearBeforeLoad` | String | `N` | Vider the version before load? (Y/NOT) |

> ⚠️ **`pVersion` takes precedence over V6**: the column V6 of the CSV is ignored. `pVersion` is forced on all lines via `sVersion = pVersion` in the section Data. This guarantees the atomicity of the load by version and avoids the mixing silent if the CSV contains several versions.

**Prolog :**
```
##############################################################
# Load_Revenue_FromCSV — PROLOG
# Target cube: Revenue (organization, Channel, product,
#              Month, Year, Version, Revenue)
##############################################################

#---- 1. Mandatory parameter validation ----
IF( TRIM(pFilePath) @= '' );
  ProcessError();
  LogOutput( 'ERROR', 'Load_Revenue_FromCSV: pFilePath is empty. Stop.' );
  ProcessBreak();
ENDIF;

IF( TRIM(pVersion) @= '' );
  ProcessError();
  LogOutput( 'ERROR', 'Load_Revenue_FromCSV: pVersion is empty. Stop.' );
  ProcessBreak();
ENDIF;

#---- 2. Block calculated versions (real 24Retail members) ----
#    Writing to these versions would create inconsistent data
IF(   pVersion @= 'Variance'
    % pVersion @= 'Variance%'
    % pVersion @= 'Rate Variance'
    % pVersion @= 'Volume Variance'
    % pVersion @= 'Ccy Exchange Variance'
    % pVersion @= 'Performance Variance'
    % pVersion @= 'Target vs Prior Year Actual'
    % pVersion @= 'Target vs Prior Year Actual %' );
  ProcessError();
  LogOutput( 'ERROR', 'Load_Revenue_FromCSV: "' | pVersion | '" is a calculated version — write forbidden.' );
  ProcessBreak();
ENDIF;

#---- 3. Check that the version exists ----
IF( DIMNM( 'Version', DIMIX( 'Version', pVersion ) ) @= '' );
  ProcessError();
  LogOutput( 'ERROR', 'Load_Revenue_FromCSV: Version "' | pVersion | '" not found.' );
  ProcessBreak();
ENDIF;

#---- 4. Optional clear before loading ----
IF( pClearBeforeLoad @= 'Y' );
  LogOutput( 'INFO', 'Clear version "' | pVersion | '" before loading.' );
  CubeClearData( 'Revenue' );
  ## Note: in production, replace this with a targeted clear on the version via a filtered view
ENDIF;

#---- 5. Counter initialization ----
nLignesChargees  = 0;
nLignesIgnorees  = 0;
nLignesErreur    = 0;

LogOutput( 'INFO', 'Start — file=' | pFilePath | ' version=' | pVersion );
```

**Data :**
```
# V1=Organization, V2=Channel, V3=Product, V4=Month
# V5=Year, V6=Version (ignored — pVersion takes precedence), V7=UnitsSold,
# V8=UnitPrice, V9=UnitCost

#---- 1. Read and clean values ----
sOrg     = TRIM( V1 );
sChannel = TRIM( V2 );
sProduct = TRIM( V3 );
sMonth   = TRIM( V4 );
sYear    = TRIM( V5 );
sVersion = pVersion;   # V6 ignored — pVersion always takes precedence
nUnitsSold = StringToNumber( V7 );
nUnitPrice = StringToNumber( V8 );
nUnitCost  = StringToNumber( V9 );

#---- 2. Ignore blank rows or residual header rows ----
IF( sOrg @= '' % sOrg @= 'Organization' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 3. Validation: Organization ----
IF( DIMIX( 'organization', sOrg ) = 0 );
  LogOutput( 'WARN', 'Ignored row — unknown Organization: [' | sOrg | ']' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 4. Validation: Channel (real members: 10, 20, 30) ----
IF( DIMIX( 'Channel', sChannel ) = 0 );
  LogOutput( 'WARN', 'Ignored row — unknown Channel: [' | sChannel | '] (valid: 10, 20, 30)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 5. Validation: Product ----
IF( DIMIX( 'product', sProduct ) = 0 );
  LogOutput( 'WARN', 'Ignored row — unknown Product: [' | sProduct | ']' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 6. Validation: Month (real members: Jan–Dec) ----
IF( DIMIX( 'Month', sMonth ) = 0 );
  LogOutput( 'WARN', 'Ignored row — unknown Month: [' | sMonth | '] (valid: Jan, Feb...Dec)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 7. Validation: Year (real members: Y1, Y2, Y3, Y4) ----
IF( DIMIX( 'Year', sYear ) = 0 );
  LogOutput( 'WARN', 'Ignored row — unknown Year: [' | sYear | '] (valid: Y1, Y2, Y3, Y4)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 8. Validation: numeric values ----
IF( nUnitsSold < 0 % nUnitPrice < 0 % nUnitCost < 0 );
  LogOutput( 'WARN', 'Ignored row — negative value: Org=' | sOrg | ' Prod=' | sProduct );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;

#---- 9. Write into the Revenue cube ----
# Order of the 7 dimensions (confirmed live):
# organization, Channel, product, Month, Year, Version, Revenue
# Write only the 3 input measures — Gross Revenue, Cost of Sales,
# Gross Margin are calculated by TM1 rules and must NOT be overwritten

CellPutN( nUnitsSold, 'Revenue', sOrg, sChannel, sProduct, sMonth, sYear, sVersion, 'Units Sold' );
CellPutN( nUnitPrice, 'Revenue', sOrg, sChannel, sProduct, sMonth, sYear, sVersion, 'Unit Price' );
CellPutN( nUnitCost,  'Revenue', sOrg, sChannel, sProduct, sMonth, sYear, sVersion, 'Unit Cost'  );

nLignesChargees = nLignesChargees + 1;
```

**Epilog :**
```
#---- 1. Summary log ----
LogOutput( 'INFO', '=== Load_Revenue_FromCSV — SUMMARY ===' );
LogOutput( 'INFO', 'File            : ' | pFilePath );
LogOutput( 'INFO', 'Version         : ' | pVersion );
LogOutput( 'INFO', 'Rows loaded : ' | NumberToString( nLignesChargees ) );
LogOutput( 'INFO', 'Rows ignored : ' | NumberToString( nLignesIgnorees ) );

#---- 2. Critical error: no row loaded ----
IF( nLignesChargees = 0 );
  LogOutput( 'ERROR', 'NO row loaded — check the file and parameters.' );
  ProcessError();
ENDIF;

#---- 3. Design choice: alert threshold on ignored rows ----
# Two approaches are valid depending on the context:
# - Fail-fast (conservative) : ProcessError() if > 5%  → blocks the process
# - Best-effort (tolerant)   : LogOutput WARN if > 10% → continue
# The threshold below is best-effort. Adapt to project requirements.
IF( nLignesChargees + nLignesIgnorees > 0 );
  nTauxIgnore = nLignesIgnorees \ ( nLignesChargees + nLignesIgnorees ) * 100;
  IF( nTauxIgnore > 10 );
    LogOutput( 'WARN', 'High ignored-row rate: ' | NumberToString( ROUND(nTauxIgnore,1) ) | '% — check the quality of the source file.' );
  ENDIF;
ENDIF;

LogOutput( 'INFO', 'Load_Revenue_FromCSV: Completed.' );
```

**Notes of design :**
- **Members of input vs calculated**: `Units Sold`, `Unit Price`, `Unit Cost` are the 3 alones members written by TI (confirmed live). `Gross Revenue`, `Cost of Sales`, `Gross Margin`, `Gross Margin %` are calculated by TM1 rules — the overwrite via TI briserait the rules and produirait data frozen.
- **Validation across 5 dimensions**: `organization`, `Channel`, `product`, `Month`, `Year` are all validated by `DIMIX()`. Channel and Month are particularly important because their members are numeric codes (10/20/30) and of the abbreviations (Jan/Feb…) susceptibles being mal formatted in a export tiers.
- **Versions calculated blocked**: 8 calculated versions identified live (Variance, Rate Variance, Ccy Exchange Variance, Performance Variance, Volume Variance…) are explicitement blocked in Prolog. A CSV loaded in these versions produirait of the inconsistencies silencieuses.

#### 🖥️ Finalization via GUI — Configuration datasource CSV (not available via MCP)

> ⚠️ **SECTION NON TESTED DURANT THE DRAFTING OF THE GUIDE**
> Remote Desktop access to the environment TechZone (`eu-de.services.cloud.techzone.ibm.com:49681`) returned a **blank page** during testing — it was not possible to place the CSV file on the server or validate the steps below. The instructions are written on the basis of the documentation TM1 and of the comportement observed in PA Modeler, but **do not have been executed of bout in bout**. TO validate lors of a prochain test with access RDP functional.

---

This step is **mandatory** so that the section Data of the process puisse read the CSV file. It comprend deux sous-steps independent: (A) place the CSV file on the server, and (B) configurer the datasource in PA Modeler.

---

##### A — Place the CSV file on the server TM1

The TI processes reads the file **on the server TM1**, not on your client workstation. The file must therefore be copied on the machine which hosts the server 24Retail.

**Prerequisites: find the bon directory TM1**

The exact path depends on the installation. Common locations on TechZone :
```
C:\Program Files\ibm\cognos\tm1_64\samples\tm1\24retail\
C:\tm1data\24retail\
D:\TM1Data\24retail\
```
> 💡 **Tip**: in PA Modeler, once the type "File" selected in Data Source, a field Directory may display the root path TM1 — note this path before copying.

**Option A1 — Via Remote Desktop (if Remote Desktop access RDP fonctionne)**

1. Itself connecter via Remote Desktop: `eu-de.services.cloud.techzone.ibm.com:49681`
2. Open **Windows File Explorer**
3. Naviguer to the directory TM1 (see above)
4. Copy-paste `revenue_csv_sample.csv` in this folder

> ⚠️ **Known TechZone issue**: the URL Remote Desktop can return a blank page in some browsers. In this case, use a **native RDP client** (Microsoft Remote Desktop on Mac/Windows) with the address `eu-de.services.cloud.techzone.ibm.com` and the port `49681`.

**Option A2 — Copy via Notepad from Remote Desktop**

If the transfert of file direct is not available :
1. Ouvrir the file [`sample-data/revenue_csv_sample.csv`](sample-data/revenue_csv_sample.csv) on ton Mac in a editor texte
2. Select tout (`Cmd+A`) → Copy (`Cmd+C`)
3. In Remote Desktop, ouvrir **Notepad** (`Win+R` → `notepad`)
4. Paste (`Ctrl+V`)
5. File → **Enregistrer sous** → naviguer to the directory TM1 → nommer `revenue_csv_sample.csv` → **Enregistrer**

**Option A3 — Via partage of folder RDP**

Before of te connecter in RDP, in the parameters of the connexion :
1. Onglet **Local Resources** → **More…** → cocher the folder local contenant the CSV
2. The folder appears in Remote Desktop sous `\\tsclient\<nom_du_dossier>`
3. From Windows Explorer in the session RDP, copy from `\\tsclient\` to the directory TM1

---

##### B — Configurer the datasource in PA Modeler

**Access PA Modeler :**
1. In PAW (`http://eu-de.services.cloud.techzone.ibm.com:31497`), click on the **menu hamburger ☰** (in haut to gauche)
2. Sous the section **Modeling**, click on **Workbench**
3. In the left-hand tree: `Databases` → `24Retail` → `Processes` → right-click `Load_Revenue_FromCSV` → **Edit**

**Onglet Data Source :**
1. Click the dropdown **"No data source"**

   > ⚠️ **Important**: in this version of PA Modeler, it there is no option "ASCII". The options available are: `No data source`, `Cube`, `Dimension`, **`File`**, `Location`, `Database connection`. Select **`File`**.

2. Renseigner the champs :

   | Field | Value |
   |---|---|
   | **File name** | `revenue_csv_sample.csv` |
   | **Directory** | Path of the folder TM1 on the server (ex: `C:\tm1data\24Retail\`) |
   | **Delimiter** | `,` (virgule) |
   | **Header rows** | `1` |
   | **Quote character** | `"` |

3. Click **Preview** for load the columns (the file must be present on the server — step A mandatory before)

**Onglet Map Variables :**

Once the preview is loaded, map the columns in the exact order exact :

| Variable TM1 | Name suggested | Type | Column CSV |
|---|---|---|---|
| V1 | `sOrg` | String | Organization |
| V2 | `sChannel` | String | Channel |
| V3 | `sProduct` | String | Product |
| V4 | `sMonth` | String | Month |
| V5 | `sYear` | String | Year |
| V6 | `sVersionCSV` | String | Version (ignored — pVersion does foi) |
| V7 | `sUnitsSold` | String | UnitsSold |
| V8 | `sUnitPrice` | String | UnitPrice |
| V9 | `sUnitCost` | String | UnitCost |

**Onglet Data Procedure :**

Paste the code suivant (which utilise the variables V1–V9 defined above) :

```
sWriteVersion = pVersion;
nUnitsSold = StringToNumber( TRIM(V7) );
nUnitPrice = StringToNumber( TRIM(V8) );
nUnitCost  = StringToNumber( TRIM(V9) );

IF( TRIM(V1) @= '' % TRIM(V1) @= 'Organization' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
IF( DIMIX( 'organization', TRIM(V1) ) = 0 );
  LogOutput( 'WARN', 'Unknown organization: [' | V1 | ']' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
IF( DIMIX( 'Channel', TRIM(V2) ) = 0 );
  LogOutput( 'WARN', 'Unknown channel: [' | V2 | '] (valid: 10, 20, 30)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
IF( DIMIX( 'product', TRIM(V3) ) = 0 );
  LogOutput( 'WARN', 'Unknown product: [' | V3 | ']' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
IF( DIMIX( 'Month', TRIM(V4) ) = 0 );
  LogOutput( 'WARN', 'Unknown month: [' | V4 | '] (valid: Jan...Dec)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
IF( DIMIX( 'Year', TRIM(V5) ) = 0 );
  LogOutput( 'WARN', 'Unknown year: [' | V5 | '] (valid: Y1, Y2, Y3, Y4)' );
  nLignesIgnorees = nLignesIgnorees + 1;
  ItemSkip();
ENDIF;
CellPutN( nUnitsSold, 'Revenue', TRIM(V1), TRIM(V2), TRIM(V3), TRIM(V4), TRIM(V5), sWriteVersion, 'Units Sold' );
CellPutN( nUnitPrice, 'Revenue', TRIM(V1), TRIM(V2), TRIM(V3), TRIM(V4), TRIM(V5), sWriteVersion, 'Unit Price' );
CellPutN( nUnitCost,  'Revenue', TRIM(V1), TRIM(V2), TRIM(V3), TRIM(V4), TRIM(V5), sWriteVersion, 'Unit Cost'  );
nLignesChargees = nLignesChargees + 1;
```

Click **Save** → the process is complete and ready to be executed.

> 💡 **Why this limitation exist?** The TM1 REST API (used by the MCP) expose the procedures TI (Prolog/Data/Epilog/Metadata) but does not allow configuration datasource of a process — this is a limitation API REST TM1 v11/v12, not of Bob. In practice, the workflow Bob + PA Modeler is complementary: Bob generates and deploy the logic (80% of the travail), PA Modeler finalizes the wiring datasource (~5 minutes).

---

#### 🔍 Verification on the server — Exercise 2.1

> Prerequisites: avoir finalized the configuration datasource in TM1 Architect or PA Modeler (section above). The steps 1 and 5 are feasible via Bob (MCP) without GUI.

**Step 1 — Check that the process existe and a the bons parameters (via Bob)**

Bob prompt:
```
Show me the parameters of the Load_Revenue_FromCSV process on 24Retail.
```
Tool: `get_tm1_process_details("Load_Revenue_FromCSV", "24Retail")`

Expected result: 4 parameters listed (`pFilePath`, `pVersion`, `pDelimiter`, `pClearBeforeLoad`).

---

**Step 2 — Copy the CSV file on the server TM1 (via GUI / access server)**

Copy [`sample-data/revenue_csv_sample.csv`](sample-data/revenue_csv_sample.csv) in the directory of data of the server TM1 (ex: `C:\tm1data\24Retail\` or the directory configured in `Tm1s.cfg`). The `pFilePath` must correspondre to the path exact on the **server** (not the customer).

---

**Step 3 — Execute the process (via Bob)**

Bob prompt:
```
Run the Load_Revenue_FromCSV process on 24Retail with
pFilePath="revenue_csv_sample.csv", pVersion="Forecast", pClearBeforeLoad="N".
```
Tool: `execute_tm1_processes_asynchronously("Load_Revenue_FromCSV", parameters=[...])`

---

**Step 4 — Check the logs execution (via Bob)**

Bob prompt:
```
Show me the execution logs of the Load_Revenue_FromCSV process on 24Retail.
```
Tool: `get_tm1_server_process_execution_error_logs("Load_Revenue_FromCSV", "24Retail")`

Expected result :
```
INFO  Start - file=revenue_csv_sample.csv version=Forecast
INFO  === Load_Revenue_FromCSV - SUMMARY ===
INFO  Rows loaded: 55
INFO  Rows ignored: 0
INFO  Load_Revenue_FromCSV: Completed.
```

---

**Step 5 — Check the data written (via Bob)**

Bob prompt:
```
Show me Units Sold for product 21002 (5G 128Gb), organization 101 (Massachusetts),
channel 10 (Retail), for Y1 in Forecast version — all months.
```

Expected result: 12 values mensuelles non nulles including the somme correspond in total of the CSV for this product/org/channel.

---

**Step 6 — Test of validation: version calculated (via Bob)**

Bob prompt:
```
Run Load_Revenue_FromCSV on 24Retail with pVersion="Variance".
```
Expected result: `ProcessBreak` immediate — log `ERROR: "Variance" is a calculated version`.

| Verification | Via | Expected result |
|---|---|---|
| Process existe + 4 parameters | Bob MCP | `pFilePath`, `pVersion`, `pDelimiter`, `pClearBeforeLoad` |
| Datasource configured | TM1 Architect / PA Modeler | Variables V1–V9 mapped |
| File CSV on server | Access server / SFTP | `revenue_csv_sample.csv` accessible |
| Normal execution | Bob MCP | Pas error |
| Logs: 55 lines loaded | Bob MCP | `nLignesChargees = 55` |
| Data in Revenue | Bob MCP | 12 values mensuelles non nulles |
| Version calculated blocked | Bob MCP | `ProcessBreak` + log ERROR |

#### ⚡ With Bob (active MCP access)
- **Time**: ~10 minutes (generation + deployment MCP + config datasource GUI)
- **What is automated by Bob**: Prolog complete, Epilog complete, parameters, creation and deployment of the process, execution, read of the logs, verification data
- **What reste manual (GUI)**: configuration datasource ASCII + mapping of the 9 columns in TM1 Architect (~3 min)
- **Quality**: code production-ready — logic complete generated by Bob, datasource finalized manually

#### 🐢 Without Bob
- **Time**: 1 to 3 hours depending on the experience of the developer
- **Required tools**: TM1 Architect or PA Modeler (for tout)
- **Risks**: omission of validation on Channel/Month/Year, write accidentelle in a version calculated, wrong mapping columns → data corrompues silently in prod

#### 💡 Discussion points

**Q: The code generated is-it production-ready or requires-t-it of the ajustements?**
> About 90% with access MCP. The members and versions are exact because read live. What remains to be adapted selon the project: (1) the threshold alert on the lines ignored (5% fail-fast vs 10% best-effort — design choice documented in the Epilog), (2) the management of the encoding of the file (UTF-8 vs ISO-8859-1 selon the locale of the server source), (3) the delimiter decimal if the CSV vient of a export European (virgule to the lieu of the point).

**Q: Why `pVersion` remplace-t-it V6 of the CSV?**
> A CSV can contain several versions mixed (ex: Forecast and Budget in the same file). Forcing `pVersion` garantit the atomicity: a call = a version. If you want to load several versions from a single file, you must loop on the versions distinctes with several calls of the process, or add a logic of groupement in the section Data.

**Q: Quels items Bob cannot deviner without a spec more precise?**
> Bob does not know: (1) the path exact of the file on the server TM1, (2) if the amounts are in units or in thousands, (3) if the file can contain of the lines subtotal to ignore in more of the header. A prompt including a extrait of 3 lines CSV real would solve these ambiguities in a interaction additional.

---

### Exercise 2.2 — Spread mensuel with key seasonal (deployment real)

> 🟡 Level: Intermediate | ⏱ ~20 min

#### 🎯 Objective
Show the complete cycle: Bob writes a TI process, deploys it on 24Retail via MCP, and confirms deploy itment — without leaving the interface Bob.

#### 📋 Context
The sales user enters an annual total of forecasts units vendues. The process must spread this total across the 12 monthss according to a configurable seasonal key (not necessarily uniform).

#### 💬 Prompt to run

```
Create and deploy on the 24Retail server a TI process named "Copy_Revenue_Units"
that spreads an annual total pUnitFcst across the 12 monthss of the Revenue cube
using a configurable seasonal key. Parameters: pOrg, pChannel, pCurrentProduct,
pYear, pVersion, pUnitFcst (Numeric), pSpreadMethod ("Uniform" or "Seasonal").
For "Seasonal", use the key [10,8,8,9,9,9,8,8,9,9,8,7] (% by month).
Validate the parameters in Prolog. Deploy the process on 24Retail.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob generates the code TI
3. Bob creates the process on 24Retail (`create_tm1_process`)
4. Bob writes the code in the process (`update_tm1_process`)
5. Bob confirms deploy itment and the possible errors of syntax
6. Compare with the result of reference

#### ✅ Reference result

**Deployment real on 24Retail — Process `Copy_Revenue_Units_v2`** *(live MCP data)*

> ⚠️ **`Copy_Revenue_Units` already exists** on 24Retail with 6 parameters. Bob does not overwrite it — it creates `Copy_Revenue_Units_v2` to preserve the existing production process. In a real project, a rename or a branch of version is recommended before tout deployment on a process existing.

**Step 1 — Creation :**
```
create_tm1_process("Copy_Revenue_Units_v2", "24Retail")
✅ Process created | HasSecurityAccess: false | Caption: "Copy_Revenue_Units_v2"
```

**Step 2 — Deployment with validation syntax :**
```
update_tm1_process("Copy_Revenue_Units_v2", "24Retail", prolog=..., epilog=..., parameters=[...])
✅ syntax_errors: []
✅ Message: "Process 'Copy_Revenue_Units_v2' updated successfully"
```

**Parameters deployed :**

| Parameter | Type | Value by default | Prompt |
|---|---|---|---|
| `pOrg` | String | `"100"` | Organization (ex: 100) |
| `pChannel` | String | `"10"` | Channel (10=Retail, 20=Internet, 30=Distribution) |
| `pCurrentProduct` | String | `"21002"` | Code product (ex: 21002) |
| `pYear` | String | `"Y1"` | Annee (ex: Y1) |
| `pVersion` | String | `"Forecast"` | Version target (ex: Forecast) |
| `pUnitFcst` | Numeric | `0` | Total annuel a ventiler |
| `pSpreadMethod` | String | `"Uniform"` | Methode of ventilation (Uniform or Seasonal) |

**Key seasonal actual deployed :**
```
Raw key: [10, 8, 8, 9, 9, 9, 8, 8, 9, 9, 8, 7] → sum = 102 (not 100)
Normalization: each key divided by vRawTotal (102) → exact sum guaranteed
Dec = pUnitFcst - sum(Jan..Nov) → absorbs rounding differences
```

> ⚠️ **Correction vs. the initial guide**: the key `[10,8,8,9,9,9,8,8,9,9,8,7]` provided in the prompt sums to **102**, not 100. The initial guide presented it normalized to 100% with different values. The deployed code automatically normalizes it on the total real — it is the bonne pratique.

**Code Prolog deployed (extrait key — structure actual validated syntaxiquement) :**
```
# Mandatory parameter validation
IF( TRIM(pOrg) @= '' );
  ProcessBreak();
ENDIF;
IF( TRIM(pVersion) @= '' );
  ProcessBreak();
ENDIF;

# Validate pSpreadMethod with nested IF/ELSE blocks
# (the TM1 engine rejects @<> and local string variables in IF)
IF( pSpreadMethod @= 'Uniform' );
  vSpreadOk = 1;
ELSE;
  IF( pSpreadMethod @= 'Seasonal' );
    vSpreadOk = 1;
  ELSE;
    ProcessError();
    LogOutput( 'ERROR', 'Invalid pSpreadMethod: ' | pSpreadMethod );
    ProcessBreak();
  ENDIF;
ENDIF;

# Block calculated versions (nested IF blocks - MANDATORY on this TM1 server)
# WARNING: IF( v @= 'a' % v @= 'b' ) on strings does NOT block on this server —
# nested IF/ELSE blocks are the only safe syntax (validated live).
IF( pVersion @= 'Variance' );
  LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
  ProcessBreak();
ELSE;
  IF( pVersion @= 'Rate Variance' );
    LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
    ProcessBreak();
  ELSE;
    IF( pVersion @= 'Volume Variance' );
      LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
      ProcessBreak();
    ELSE;
      IF( pVersion @= 'Ccy Exchange Variance' );
        LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
        ProcessBreak();
      ELSE;
        IF( pVersion @= 'Performance Variance' );
          LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
          ProcessBreak();
        ELSE;
          IF( pVersion @= 'Target vs Prior Year Actual' );
            LogOutput( 'ERROR', pVersion | ' is a calculated version - write forbidden.' );
            ProcessBreak();
          ENDIF;
        ENDIF;
      ENDIF;
    ENDIF;
  ENDIF;
ENDIF;

# Seasonal key: automatic normalization on the real total (102)
IF( pSpreadMethod @= 'Seasonal' );
  vRawTotal = 10 + 8 + 8 + 9 + 9 + 9 + 8 + 8 + 9 + 9 + 8 + 7;
  vKeyJan = 10 \ vRawTotal;  vKeyFeb = 8 \ vRawTotal;  vKeyMar = 8 \ vRawTotal;
  vKeyApr = 9  \ vRawTotal;  vKeyMay = 9 \ vRawTotal;  vKeyJun = 9 \ vRawTotal;
  vKeyJul = 8  \ vRawTotal;  vKeyAug = 8 \ vRawTotal;  vKeySep = 9 \ vRawTotal;
  vKeyOct = 9  \ vRawTotal;  vKeyNov = 8 \ vRawTotal;  vKeyDec = 7 \ vRawTotal;
ENDIF;

# Unit calculation — INT() mandatory (ROUND() invalid on this TM1 server)
vUnitsJan = INT( pUnitFcst * vKeyJan );
# ... × 11 months
# Dec absorbs rounding differences :
vUnitsDec = pUnitFcst - vUnitsJan - vUnitsFeb - ... - vUnitsNov;
```

**Code Epilog deployed (write in Revenue) :**
```
# Order of the 7 dimensions (confirmed live):
# organization, Channel, product, Month, Year, Version, Revenue
CellPutN( vUnitsJan, 'Revenue', pOrg, pChannel, pCurrentProduct, 'Jan', pYear, pVersion, 'Units Sold' );
# ... × 12 months
LogOutput( 'INFO', 'Copy_Revenue_Units_v2: 12 months loaded. Total=' | NumberToString(vSomme) );
```

**The process is visible and executable in IBM Planning Analytics 24Retail.**

> 🔧 **Lessons of syntax TM1 discoveries in deployment live :**
> - `ROUND( x, 0 )` → **invalid** on this server (conflit of parsing of the virgules) — use `INT( x )` to the place
> - `IF( numVar = 0 )` directly on a Numeric parameter → **invalid** — work around it with `IF( param = 0 )` only in a block without `ProcessBreak` nested, or restructure with `ELSE`
> - `IF( stringVar @= 'val' )` on a variable locale string → **invalid** — always use of the `IF @= 'valeur'` directs on the parameter or of the IF/ELSE nested
> - ⚠️ `IF( pVersion @= 'A' % pVersion @= 'B' % pVersion @= 'C' )` → **ne bloque not silently** on this server TM1 quand used with `ProcessBreak()` — use of the **IF/ELSE nested** for each value to test (validated live: the `%` OR on strings notse the syntax but does not evaluate correctly at runtime)
> - These behaviors are specific to the version TM1 of the environment TechZone — they may differ on other servers PA

#### ⚡ With Bob (active MCP access)
- **Time**: ~15 minutes (generation + debugging syntax live + deployment confirmed)
- **What is automated**: write of the code, deployment, read of the errors of syntax, iterative corrections, final confirmation
- **Quality**: process validated syntaxiquement by the server TM1, deployed and executable

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (write + test + deployment manual + debugging syntax)
- **Required tools**: TM1 Architect, access to the server, documentation TI
- **Risks**: errors of syntax TI discoveries tardivement, key seasonal including the somme is not 100% (error silent), omission validations of parameters

#### 💡 Discussion points

**Q: The deployment via MCP is-it sufficient or must-it a process of validation additional?**
> The MCP validates the **syntax TI** (`syntax_errors: []` confirmed). This does not validate the **logic business**: a process syntactically correct can write in the mauvaises cells. It must therefore a test execution on a sandbox or on data of test before of validate in production. Workflow recommended: deploy via Bob → test manually on sandbox → validate the results → promote in production.

**Q: What happens if `pOrg` is passed blank? Did Bob handle this case?**
> Yes — the validation `IF( TRIM(pOrg) @= '' )` is included in Prolog with `ProcessBreak()`. The process stops cleanly before any write. It is a direct improvement over the initial guide, which did not handle this case.

**Q: Why does Dec absorb rounding rather than Jan?**
> Putting the rounding adjustment on Dec is a standard PA convention: December is the final closing month and year-end adjustments are naturally expected there. Putting the adjustment on Jan would bias M+1 comparisons at the beginning of the year. Alternative: distribute the remainder to the month with the largest decimal part (Largest Remainder algorithm), but this complicates the TI without significant operational benefit.

#### 🔍 Verification on the server — Exercise 2.2

> These checks confirment that `Copy_Revenue_Units_v2` is bien deployed and product the results attendus on the cube Revenue of 24Retail. ✅ **Tested live on 24Retail during guide drafting.**

**Step 1 — Check that the process is deployed with the bons parameters**

Bob prompt:
```
Show me the details of the Copy_Revenue_Units_v2 process on 24Retail.
```
Tool: `get_tm1_process_details("Copy_Revenue_Units_v2", "24Retail")`

✅ **Live result obtained**: 7 parameters listed — `pOrg` (String, "100"), `pChannel` (String, "10"), `pCurrentProduct` (String, "21002"), `pYear` (String, "Y1"), `pVersion` (String, "Forecast"), `pUnitFcst` (Numeric, 0), `pSpreadMethod` (String, "Uniform"). Prompts and types compliant to the guide.

---

**Step 2 — Execute in mode Uniform and check the spread**

Bob prompt:
```
Run Copy_Revenue_Units_v2 on 24Retail with:
pOrg=101, pChannel=10, pCurrentProduct=21002, pYear=Y1,
pVersion=Forecast, pUnitFcst=1200, pSpreadMethod=Uniform.
```
Tool: `execute_tm1_processes_asynchronously("Copy_Revenue_Units_v2", parameters=[...])`

✅ **Live result obtained**: execution started without error, `status_url` returned. No log error (`get_tm1_server_process_execution_error_logs` returns "Unable to locate" — normal behavior when the process ends without error).

---

**Step 3 — Check the data mensuelles written**

Bob prompt:
```
Show me monthly Units Sold for product 21002 (5G 128Gb),
organization 101, channel 10, year Y1, Forecast version on 24Retail.
```

Tool MDX :
```sql
SELECT
{ [Month].[Month].[Jan], [Month].[Month].[Feb], [Month].[Month].[Mar],
  [Month].[Month].[Apr], [Month].[Month].[May], [Month].[Month].[Jun],
  [Month].[Month].[Jul], [Month].[Month].[Aug], [Month].[Month].[Sep],
  [Month].[Month].[Oct], [Month].[Month].[Nov], [Month].[Month].[Dec] } ON COLUMNS,
{ [Revenue].[Revenue].[Units Sold] } ON ROWS
FROM [Revenue]
WHERE (
  [organization].[organization].[101],
  [Channel].[Channel].[10],
  [product].[product].[21002],
  [Year].[Year].[Y1],
  [Version].[Version].[Forecast]
)
```

> ⚠️ **Attention**: the measure dimension cube `Revenue` is called **`Revenue`** (not `Revenue Measures`). The exact name must be used in the MDX, otherwise `execute_mdx_and_get_view` returns `Failed to encode cube view to markdown/HTML`.

✅ **Live result obtained** in mode Uniform (pUnitFcst=1200) :

| | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Units Sold | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 |

**Somme = 1200 ✅**

---

**Step 4 — Execute in mode Seasonal and check the key**

Bob prompt:
```
Run Copy_Revenue_Units_v2 on 24Retail with:
pOrg=101, pChannel=10, pCurrentProduct=21002, pYear=Y1,
pVersion=Forecast, pUnitFcst=1020, pSpreadMethod=Seasonal.
```

Expected result (pUnitFcst=1020, key normalized on 102) :
```
Jan=100, Feb=80, Mar=80, Apr=90, May=90, Jun=90,
Jul=80, Aug=80, Sep=90, Oct=90, Nov=80, Dec=70
Somme = 1020 ✅
```

> 💡 **Why 1020?** With pUnitFcst=1020 and key normalized on 102, each raw unit corresponds exactly to 10 units → not rounding, clean validation.

---

**Step 5 — Test of validation: version calculated**

Bob prompt:
```
Run Copy_Revenue_Units_v2 on 24Retail with pVersion="Rate Variance", pUnitFcst=1000.
```
Expected result: `ProcessBreak` immediate — log `ERROR: Rate Variance is a calculated version`.

---

**Step 6 — Check the logs execution**

Bob prompt:
```
Show me the logs of the latest Copy_Revenue_Units_v2 run on 24Retail.
```
Tool: `get_tm1_server_process_execution_error_logs("Copy_Revenue_Units_v2", "24Retail")`

> ⚠️ **Behavior observed**: when the process ends **without error**, `get_tm1_server_process_execution_error_logs` returns "Unable to locate execution logs" — it is normal, no error file is created. `INFO` logs are only visible if the process used `LogOutput()` **and** ended in error/ProcessBreak. For successful runs, the absence of logs confirms success.

Expected result if ProcessBreak (version calculated) :
```
INFO  Org=101 Product=21002 Year=Y1 Total=1020 Method=Seasonal
INFO  Copy_Revenue_Units_v2: 12 months loaded. Total=1020 Requested=1020
INFO  Copy_Revenue_Units_v2: Completed.
```

| Verification | Test parameters | Actual result |
|---|---|---|
| ✅ Process deployed | — | 7 parameters including `pSpreadMethod` (String, "Uniform") |
| ✅ Uniform mode executed | pUnitFcst=1200 | Execution without error, status_url received |
| ✅ Data Revenue verified | MDX live | 12 × 100 = 1200 ✅ |
| ✅ MDX fonctionne | dim `[Revenue].[Revenue]` | `execute_mdx_and_get_view` operational |
| ⬜ Mode Seasonal | pUnitFcst=1020 | Non tested live (to validate) |
| ✅ Version calculated blocked | pVersion="Rate Variance" | ProcessBreak — Rate Variance = 0 (v1 with `%` OR failed silently → correctd with IF nested) |
| ℹ️ Logs successful run | — | "Unable to locate" = normal, not error |

---

### Exercise 2.3 — Calcul aggregated multi-cubes

> 🔴 Level: Advanced | ⏱ ~20 min

#### 🎯 Objective
Demonstrate that Bob can write a TI process which reads from a cube source and writes in a cube target — a classic pattern integration between modules PA.

#### 📋 Context
The model 24Retail a of the costs into the cube Supply Chain which must be aggregated and reported into the cube Income Statement to produce the P&L complete.

#### 💬 Prompt to run

```
Write a TI process for 24Retail that:
1. Reads cost data from the "Supply Chain" cube (organization, product, year, version)
2. Aggregates costs by organization and by year
3. Writes the aggregated costs into the "Income Statement" cube on the appropriate cost member
Use CellGetN for reading and CellPutN for writing. Add a pVersion parameter
to target the version to process. Provide the complete code with comments.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob consulte the structures of the cubes Supply Chain and Income Statement on 24Retail
3. Bob generates the code TI with the bon mapping of dimensions
4. Compare with the result of reference

#### ✅ Reference result

> ✅ **Exercise tested live on 24Retail during guide drafting.** Several major architecture discoveries emerged — read the section "Discoveries live" below.

**Structure of the cubes confirmed by MCP :**

| Cube | Dimensions (order exact) |
|---|---|
| **Supply Chain** | `organization`, `Channel`, `product`, `Month`, `Year`, `Supply Chain Measures`, `Version` |
| **Income Statement** | `Currency Calc`, `organization`, `Year`, `Month`, `Account`, `Version` |

**Dimensions communes identified**: `organization`, `Month`, `Year`, `Version`

**Measures Supply Chain available**: `Units Sold`, `Total Cost of Goods Sold - Regional`, `Direct Costs`, `Direct Material`, `Raw Material`, `Packaging Material`, `Pallets`, `Direct Labor`, `Indirect Costs`, `Overhead`, `Indirect Labor`

**Comptes Income Statement available**: `Statistics`, `Square Footage`, `Server Space`, `FTE`, `5999` (Gross Cost of Sales), `GM`, `4999` (Gross Revenue), `TE`, `6000`–`6150` (Operating Expenses)

---

**⚠️ Discovery architecturale critical — Income Statement is a cube of reporting calculated**

In 24Retail, the cube **Income Statement is fully managed by TM1 rules** — the accounts `5999`, `6000`, `6100`–`6150` are all protected. Toute tentative write directe via `CellPutN` returns :
```
Rule applies to cell  (error repeats 119 times)
```
**Consequence**: the pattern "read Supply Chain → write Income Statement" is not **not feasible** directement on 24Retail via `CellPutN`. Income Statement aggregates from Revenue and Supply Chain via its TM1 rules — the rules engine which performs the consolidation, not a TI process.

**This that this process does actually on 24Retail :**

The process `PLConsolide_Load_SupplyChain` is deployed and syntactically valid. It reads correctly from Supply Chain. For the write, two options depending on the real model of the customer :
1. **Write in a cube intermediate** (Supply Chain Target or cube of staging) — and laisser the rules Income Statement read from this cube
2. **Adapter the parameter `pAccount`** to a compte which is not covered by the TM1 rules (to identifier with the customer)

---

**Code Prolog deployed live (4 parameters) :**
```
# ============================================================
# PLConsolide_Load_SupplyChain
# Reads consolidated Supply Chain costs and writes them
# into Income Statement through a configurable pAccount.
#
# 24Retail note: 5999 and all 6xxx accounts are calculated
# by TM1 rules — pAccount must point to an unprotected account.
# ============================================================

# Parameter validation
IF( TRIM(pVersion) @= '' );
  LogOutput( 'ERROR', 'pVersion is empty.' );
  ProcessBreak();
ENDIF;
IF( TRIM(pYear) @= '' );
  LogOutput( 'ERROR', 'pYear is empty.' );
  ProcessBreak();
ENDIF;

# Block calculated versions (nested IF blocks)
IF( pVersion @= 'Variance' );
  LogOutput( 'ERROR', pVersion | ' is a calculated version.' );
  ProcessBreak();
ELSE;
  IF( pVersion @= 'Rate Variance' );
    LogOutput( 'ERROR', pVersion | ' is a calculated version.' );
    ProcessBreak();
  ELSE;
    IF( pVersion @= 'Volume Variance' );
      LogOutput( 'ERROR', pVersion | ' is a calculated version.' );
      ProcessBreak();
    ELSE;
      IF( pVersion @= 'Ccy Exchange Variance' );
        LogOutput( 'ERROR', pVersion | ' is a calculated version.' );
        ProcessBreak();
      ENDIF;
    ENDIF;
  ENDIF;
ENDIF;

# Validate pCurrCalc
IF( pCurrCalc @= 'Local' );
  vCurrOk = 1;
ELSE;
  IF( pCurrCalc @= 'Base' );
    vCurrOk = 1;
  ELSE;
    LogOutput( 'ERROR', 'Invalid pCurrCalc: ' | pCurrCalc );
    ProcessBreak();
  ENDIF;
ENDIF;

vNbEcritures = 0;
```

**Code Epilog deployed live :**
```
# Loop over the 10 leaf organizations (hard-coded — safer on this server than DIMNM)
vOrgIdx = 1;
WHILE( vOrgIdx <= 10 );
  IF( vOrgIdx = 1  );  vOrg = '101';  ENDIF;
  IF( vOrgIdx = 2  );  vOrg = '102';  ENDIF;
  IF( vOrgIdx = 3  );  vOrg = '103';  ENDIF;
  IF( vOrgIdx = 4  );  vOrg = '201';  ENDIF;
  IF( vOrgIdx = 5  );  vOrg = '202';  ENDIF;
  IF( vOrgIdx = 6  );  vOrg = '301';  ENDIF;
  IF( vOrgIdx = 7  );  vOrg = '302';  ENDIF;
  IF( vOrgIdx = 8  );  vOrg = '401';  ENDIF;
  IF( vOrgIdx = 9  );  vOrg = '402';  ENDIF;
  IF( vOrgIdx = 10 );  vOrg = '403';  ENDIF;

  vMonthIdx = 1;
  WHILE( vMonthIdx <= 12 );
    IF( vMonthIdx = 1  );  vMonth = 'Jan';  ENDIF;
    IF( vMonthIdx = 2  );  vMonth = 'Feb';  ENDIF;
    IF( vMonthIdx = 3  );  vMonth = 'Mar';  ENDIF;
    IF( vMonthIdx = 4  );  vMonth = 'Apr';  ENDIF;
    IF( vMonthIdx = 5  );  vMonth = 'May';  ENDIF;
    IF( vMonthIdx = 6  );  vMonth = 'Jun';  ENDIF;
    IF( vMonthIdx = 7  );  vMonth = 'Jul';  ENDIF;
    IF( vMonthIdx = 8  );  vMonth = 'Aug';  ENDIF;
    IF( vMonthIdx = 9  );  vMonth = 'Sep';  ENDIF;
    IF( vMonthIdx = 10 );  vMonth = 'Oct';  ENDIF;
    IF( vMonthIdx = 11 );  vMonth = 'Nov';  ENDIF;
    IF( vMonthIdx = 12 );  vMonth = 'Dec';  ENDIF;

    # Read Supply Chain — Channel Total + Product Total = consolidated view
    # Dimensions: organization, Channel, product, Month, Year, Supply Chain Measures, Version
    vCost = CellGetN( 'Supply Chain', vOrg, 'Channel Total', 'Product Total',
                      vMonth, pYear, 'Total Cost of Goods Sold - Regional', pVersion );

    # Write to Income Statement
    # Dimensions: Currency Calc, organization, Year, Month, Account, Version
    CellPutN( vCost, 'Income Statement', pCurrCalc, vOrg, pYear, vMonth, pAccount, pVersion );

    vNbEcritures = vNbEcritures + 1;
    vMonthIdx = vMonthIdx + 1;
  END;

  vOrgIdx = vOrgIdx + 1;
END;

LogOutput( 'INFO', 'PLConsolide_Load_SupplyChain: Completed. Writes=' | NumberToString(vNbEcritures) );
```

**Parameters deployed :**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `pVersion` | String | `Forecast` | Version to traiter |
| `pYear` | String | `Y1` | Year to traiter |
| `pCurrCalc` | String | `Local` | Currency Calc in Income Statement |
| `pAccount` | String | `6130` | Compte target (to adapter — all the accounts 24Retail are protected by rules) |

#### ⚡ With Bob
- **Time**: ~15 minutes (exploration structures + generation + debugging discovery rules + redeployment)
- **What is automated**: read of the structures of the two cubes, mapping of the shared dimensions, generation code, deployment, test execution, diagnostic of the error `Rule applies to cell`
- **Quality**: code structured with comments, dimensions correctly mapped; the discovery of the architecture "rules" is a direct contribution of Bob — without Bob, the developer would have discovered it only in testing manually

#### 🐢 Without Bob
- **Time**: 3 to 6 hours (analysis of the two models + write + test + diagnostic rules)
- **Required tools**: TM1 Architect open on the two cubes simultaneously, knowledge of the TM1 rules of the model
- **Risks**: error of mapping between dimensions homonymes, wrong aggregation, write on the wrong member, discovery tardive of the protections by rules

#### 💡 Discussion points

**Q: Why `CellPutN` fails with `Rule applies to cell` on Income Statement?**
> Income Statement in 24Retail is a **calculated reporting** cube — its cells are fed by TM1 rules that aggregate from Revenue and Supply Chain. The TM1 rules take priority on the direct writes: TM1 refuses to write in a cell governed by a rule active. It is a protection architecturale intentionalle in this model. For integrate data of cost, it must either (1) write in a cube source that the rules lisent, either (2) identifier a compte non covered by the rules.

**Q: However test this process without overwrite the data of production?**
> Three approaches: (1) **Sandbox PA**: execute in a sandbox — the writes are isolated and do not affect the cube Basis, (2) **Test version**: create a version `"Test_SC_Import"` and pass this version in parameter, (3) **Environment of recette**: execute on the server of recette (copy of the prod) before of pass on the prod.

**Q: Bob a-t-it identified the shared dimensions between the two cubes?**
> Yes, Bob a correctly identified `organization`, `Month`, `Year`, `Version` as shared dimensions. It a aussi detected the divergence: Supply Chain a `Channel`, `product` and `Supply Chain Measures`, Income Statement a `Currency Calc` and `Account` — this which impose a aggregation in the process via `Channel Total` and `Product Total`.

---

#### 🔍 Verification on the server — Exercise 2.3

> ✅ **Toutes the steps below ont been executed live on 24Retail during guide drafting.**

**Step 1 — Check that the process is deployed with the bons parameters**

Bob prompt:
```
Show me the details of the PLConsolide_Load_SupplyChain process on 24Retail.
```
Tool: `get_tm1_process_details("PLConsolide_Load_SupplyChain", "24Retail")`

✅ **Live result obtained**: 4 parameters — `pVersion` (String, "Forecast"), `pYear` (String, "Y1"), `pCurrCalc` (String, "Local"), `pAccount` (String, "6130"). Prompts and types compliant.

---

**Step 2 — Check that Supply Chain contains data lisibles**

Tool MDX :
```sql
SELECT
{ [Month].[Month].[Jan], ..., [Month].[Month].[Dec] } ON COLUMNS,
{ [Supply Chain Measures].[Supply Chain Measures].[Total Cost of Goods Sold - Regional] } ON ROWS
FROM [Supply Chain]
WHERE (
  [organization].[organization].[101],
  [Channel].[Channel].[Channel Total],
  [product].[product].[Product Total],
  [Year].[Year].[Y1],
  [Version].[Version].[Forecast]
)
```

✅ **Live result obtained** — 12 months non nuls for org 101, Forecast, Y1 :

| | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Total COGS Regional | 992,154 | 1,454,335 | 1,489,479 | 1,517,267 | 1,549,299 | 1,577,047 | 1,600,339 | 1,632,234 | 1,681,412 | 1,667,650 | 1,751,938 | 1,753,839 |

`CellGetN` reads correctly the consolidated via `Channel Total` / `Product Total`.

---

**Step 3 — Confirmer the error Rule applies to cell (comportement expected on 24Retail)**

Bob prompt:
```
Run PLConsolide_Load_SupplyChain on 24Retail with pVersion=Forecast, pYear=Y1, pCurrCalc=Local, pAccount=6130.
```
Tool: `execute_tm1_processes_asynchronously("PLConsolide_Load_SupplyChain", parameters=[...])`

✅ **Live result obtained** :
```
Error: Epilog procedure line (44): Rule applies to cell
Error: Epilog procedure line (44):     error repeats 119 times
```
Behavior expected and documented — Income Statement is a cube calculated by TM1 rules on 24Retail. The 120 errors correspondent to 10 orgs × 12 months. **This is not a bug of the process** — it is a constraint architecturale of the model 24Retail.

---

**Step 4 — Check the protection calculated versions**

Bob prompt:
```
Run PLConsolide_Load_SupplyChain on 24Retail with pVersion="Rate Variance".
```

✅ **Expected result**: ProcessBreak in Prolog — the log `Rule applies to cell` displayed corresponds to the previous run (Forecast), not to this run. When Prolog triggers ProcessBreak, the Epilog does not execute — no new error file is created.

> ℹ️ **Behavior of the logs TM1**: `get_tm1_server_process_execution_error_logs` returns the last error file created on the server — not necessarily that of the latest run. A run which ends in ProcessBreak **in Prolog** does not create a log file (the process is stopped before the Epilog).

---

| Verification | Tool | Actual result |
|---|---|---|
| ✅ Process deployed | `get_tm1_process_details` | 4 parameters corrects |
| ✅ CellGetN Supply Chain | MDX live | 12 months non nuls (org 101, Forecast) |
| ✅ Rule applies to cell reproductible | Execution live | 120 errors — comportement expected IS calculated |
| ✅ Protection version calculated | Prolog IF nested | ProcessBreak before Epilog |
| ℹ️ Logs Prolog ProcessBreak | `get_tm1_server_process_execution_error_logs` | Returns dernier log Epilog — not celui of the Prolog |

---

### Exercise 2.4 — Optimisation of a process existing

> 🔴 Level: Advanced | ⏱ ~15 min

#### 🎯 Objective
Show that Bob can audit two existing processes, identify redundancies and anti-patterns, and propose a unified and optimized version.

#### 📋 Context
You reprends the code of a consultant previous. Deux process font of the choses very similaires. You must the consolider before the mise in production.

#### 💬 Prompt to run

```
Analyze the Copy_Revenue_Compare_Units and Copy_Revenue_Spread_Units processes
on the 24Retail server. Identify redundancies, anti-patterns, and risks.
Rewrite them as a unified process named "Copy_Revenue_Units_V2" with a pMode
parameter ("Compare" or "Spread"), input validations, and minimal logging
(user + timestamp). Explain each optimization choice.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob retrieves the metadata of the two processes on 24Retail
3. Bob analysis and product the code unified with explanations
4. Compare with the result of reference

#### ✅ Reference result

> ✅ **Exercise tested live on 24Retail during guide drafting.** Process deployed: `Copy_Revenue_Units_Unified`. Deux corrections syntaxiques importantes vs the guide initial — see below.

**Analysis of the two processes existing (metadata live)**

**`Copy_Revenue_Compare_Units`** — 3 parameters: `pCurrentProduct`, `pUnitFcst`, `pChannel`
- No prompts, not of values by default → anti-pattern
- Missing `pOrg`, `pYear`, `pVersion` → uncontrolled scope
- *Role inferred*: comparison of a unit forecast with the existing data of Revenue

**`Copy_Revenue_Spread_Units`** — 5 parameters: `pOrg`, `pChannel`, `pCurrentProduct`, `pYear`, `pUnitFcst`
- No prompts, not of values by default → anti-pattern
- Missing `pVersion` → undetermined write version → risk of accidental write
- *Role inferred*: ventilation mensuelle of a total annuel in Revenue

**Redondances and anti-patterns identified :**
- The two processes manipulent the same cube (`Revenue`), the same mesures (`Units Sold`) and partagent 3 parameters identiques (`pChannel`, `pCurrentProduct`, `pUnitFcst`)
- `Copy_Revenue_Compare_Units` does not have of parameter `pOrg` ni `pYear` → scope trop wide
- No does not have of `pVersion` → write risks silencieuses in the wrong version
- No prompt on no parameter → experience user degraded
- Deux process to maintenir separately for tout changement of model

**Process unified `Copy_Revenue_Units_Unified` deployed live — 7 parameters :**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `pMode` | String | `Spread` | `Compare` or `Spread` |
| `pOrg` | String | _(blank)_ | Organization target |
| `pChannel` | String | _(blank)_ | Channel (10/20/30) |
| `pCurrentProduct` | String | _(blank)_ | Code product |
| `pYear` | String | `Y1` | Year |
| `pVersion` | String | `Forecast` | Version target |
| `pUnitFcst` | Numeric | `0` | Total annuel to traiter |

> ⚠️ **Corrections syntaxiques vs guide initial :**
> - `Now(1)` → **invalid** on this server (`Unexpected parenthesis`) — use `TM1User()` alone for the logging, or `Now()` without argument if available
> - `IF( pMode @= '' % pOrg @= '' )` → **ne bloque not** on this server (pattern `%` OR on strings) — use of the IF separate by parameter
> - `IF( pMode @<> 'Compare' & pMode @<> 'Spread' )` → **syntax `@<>` invalid** — use IF/ELSE nested on `@=`

**Gains optimization documented :**
1. **Maintenance**: 1 alone process to maintenir vs 2
2. **Consistency**: `pOrg`, `pYear`, `pVersion` added (correctifs critical)
3. **Observability**: logging `TM1User()` + mode + org + product + version in beginning of run
4. **Security**: blocking calculated versions via IF nested (pattern validated on this server)
5. **Extensibility**: adding a new mode without create a 3e process

#### ⚡ With Bob
- **Time**: ~15 minutes (analysis + generation + correction `Now(1)` + deployment + tests modes Compare and Spread)
- **What is automated**: analysis comparative of the metadata, identification anti-patterns, generation code unified, deployment, correction errors of syntax live
- **Quality**: code deployed and tested live on the deux modes — important documented syntax corrections

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (read of the two codebases + drafting of the unified version + debugging syntax)
- **Required tools**: TM1 Architect, access to the code source
- **Risks**: omission of a case handled in has of the two processes, regression functional, errors `Now(1)` and `@<>` not detected before execution

#### 💡 Discussion points

**Q: Bob can-it detect of the bugs in the code without pouvoir the read directement?**
> In partie. Bob can identifier of the problems structurels to partir of the metadata: missing parameters (ex: `Copy_Revenue_Compare_Units` without `pOrg` ni `pYear`), naming inconsistent between process similaires, parameters without value by default ni prompt. Howeverever, Bob cannot detect a bug logic (ex: the key seasonal ne somme not to 100%) without see the code. Workaround: paste the code in the chat — Bob can alors do a analysis statique complete.

**Q: Quelle is the limitation analysis without access to the code source complete?**
> Bob travaille with the "metadata publiques" of the server (names, parameters, types). It cannot detect: (1) the logiques of calcul wrong, (2) the hardcodings dangereux (chemins, names of members hard-coded), (3) the boucles infinies potentielles, (4) the problems of performance (not OptimizeData, not of CommitWait). These analyses require the code source — exportable from TM1 Architect then pasted in Bob.

---

#### 🔍 Verification on the server — Exercise 2.4

> ✅ **Toutes the steps executed live on 24Retail during guide drafting.**

**Step 1 — Check deploy itment**

Tool: `get_tm1_process_details("Copy_Revenue_Units_Unified", "24Retail")`

✅ **Live result**: 7 parameters with prompts and types corrects — `pMode`, `pOrg`, `pChannel`, `pCurrentProduct`, `pYear`, `pVersion`, `pUnitFcst`.

---

**Step 2 — Test the Mode Compare**

```
Run Copy_Revenue_Units_Unified on 24Retail :
pMode=Compare, pOrg=101, pChannel=10, pCurrentProduct=21002,
pYear=Y1, pVersion=Forecast, pUnitFcst=1200
```

✅ **Live result**: execution without error, no log error.

---

**Step 3 — Test the Mode Spread and check the data**

```
Run Copy_Revenue_Units_Unified on 24Retail :
pMode=Spread, pOrg=101, pChannel=10, pCurrentProduct=21002,
pYear=Y1, pVersion=Forecast, pUnitFcst=2400
```

✅ **Live result** — 12 × 200 = 2400 confirmed via MDX :

| | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Units Sold | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 |

---

**Step 4 — Test the validation pMode invalid**

```
Run Copy_Revenue_Units_Unified on 24Retail with pMode="Budget".
```

Expected result: ProcessBreak in Prolog + log `ERROR: pMode invalide: Budget`.

---

| Verification | Tool | Actual result |
|---|---|---|
| ✅ 7 parameters deployed | `get_tm1_process_details` | Prompts and types compliant |
| ✅ Mode Compare | Execution live | Without error |
| ✅ Mode Spread 2400 | MDX live | 12 × 200 = 2400 ✅ |
| ⬜ pMode invalid blocked | To test | Not executed live |
| ⚠️ `Now(1)` invalid | Error syntax correctd | Removed — logging `TM1User()` alone |

---

## Domain 3 — TM1 Rules

> **Relevant BUILD role**: Developer TI senior, Architecte PA
> **Situation**: Development sprint — calculs time real, conversions, KPIs

### Exercise 3.1 — Derived KPIs: Contribution Margin

> 🟡 Level: Intermediate | ⏱ ~10 min

#### 🎯 Objective
Show that Bob generates of the TM1 rules correct and commented for of the KPIs business — using the real names of members confirmed live on 24Retail, without creating duplicates with the rules existing.

#### 📋 Context
The cube Revenue of 24Retail contains already the rules for `Gross Revenue`, `Cost of Sales`, `Gross Margin` and `Gross Margin %`. The customer wants a new KPI: the **Contribution Margin %**, which measures profitability excluding the indirect costs (`Indirect COGS`), for identifier quels products cover leurs costs directs.

> ℹ️ **Why this prompt rather that the original?** The prompt initial demandait `Gross Margin %` (already calculated) and `Run Rate` (member absent of the dimension). These deux cibles rendent the exercise non deployable on 24Retail. This prompt target a KPI actually absent and fully calculable from the members existing.

#### 💬 Prompt to run

```
Generate TM1 rules for the 24Retail Revenue cube to calculate the following
two KPIs, using the real members of the Revenue dimension:

1. Contribution Margin = Gross Revenue - Direct COGS
   (Direct COGS = direct costs only, excluding indirect costs)

2. Contribution Margin % = Contribution Margin / Gross Revenue × 100

The input members in Revenue are: Units Sold, Unit Price, Unit Cost.
The existing calculated members are: Gross Revenue, Direct COGS, Indirect COGS,
Cost of Sales, Gross Margin, Gross Margin %.
The new members Contribution Margin and Contribution Margin % must be added
to the Revenue dimension before deployment.

Add explanatory comments, protection against division by zero,
and SKIPCHECK if needed.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob consulte the real members of the dimension Revenue on 24Retail
3. Bob verifies that the members sources (`Direct COGS`, `Gross Revenue`) existent and ont of the values
4. Bob generates the deux TM1 rules with comments
5. Compare with the result of reference

#### ✅ Reference result

> ✅ **Exercise tested live on 24Retail during guide drafting.**

**Members sources confirmed live — dimension `Revenue` :**

| Member | Type | Value live (org 101, Ch 10, prod 21002, Y1, Forecast, Jan) |
|---|---|---|
| `Units Sold` | Input | 200 |
| `Unit Price` | Input | 70.00 |
| `Unit Cost` | Input | 47.00 |
| `Gross Revenue` | Calculated (rule existing) | 14,000 (= 200 × 70) |
| `Direct COGS` | Calculated (rule existing) | 9,400 (= 200 × 47) |
| `Indirect COGS` | Calculated (rule existing) | 1,200 |
| `Gross Margin %` | Calculated (rule existing) | 24.29% |
| `Contribution Margin` | **TO create** | — |
| `Contribution Margin %` | **TO create** | — |

**Values attendues after deployment of the rules (all the month, data uniformes) :**

| Month | Gross Revenue | Direct COGS | **Contribution Margin** | **Contribution Margin %** | Gross Margin % |
|---|---|---|---|---|---|
| Jan–Dec | 14,000 | 9,400 | **4,600** | **32.86%** | 24.29% |

> 💡 The difference between Contribution Margin % (32.86%) and Gross Margin % (24.29%) = impact of the Indirect COGS (1,200/month). It is precisely the value added of this KPI.

---

**Prerequisites before deployment :**
1. Ajouter the member `Contribution Margin` in the dimension Revenue (after `Gross Margin`)
2. Ajouter the member `Contribution Margin %` in the dimension Revenue (after `Contribution Margin`)
3. Deploy the rules below in TM1 Architect → cube Revenue → onglet Rules

---

**Rule 1 — Contribution Margin :**
```
# ============================================================
# Contribution Margin = Gross Revenue - Direct COGS
# Measures margin BEFORE indirect costs (overhead, distribution costs)
# Identifies which products cover their direct costs
#
# Source members confirmed live on 24Retail:
#   ['Gross Revenue'] = Units Sold x Unit Price  (existing rule)
#   ['Direct COGS']   = Units Sold x Unit Cost   (existing rule)
# ============================================================
['Contribution Margin'] = N:
  ['Gross Revenue'] - ['Direct COGS'];
```

**Rule 2 — Contribution Margin % :**
```
# ============================================================
# Contribution Margin % = Contribution Margin / Gross Revenue x 100
# Division-by-zero protection: IF on Gross Revenue
# SKIPCHECK not required: no circular reference
#   (source members are calculated from input members)
# ============================================================
['Contribution Margin %'] = N:
  IF( ['Gross Revenue'] <> 0,
      ['Contribution Margin'] \ ['Gross Revenue'] * 100,
      0 );
```

> ℹ️ **Why SKIPCHECK is not required here?** `['Contribution Margin']` and `['Contribution Margin %']` lisent `['Gross Revenue']` and `['Direct COGS']`, which are eux-same calculated from `Units Sold`, `Unit Price`, `Unit Cost` (input members, not calculated by rule to the same level). It there is no of reference circulaire. `SKIPCHECK` is not required that quand a rule reads a member calculated by a autre rule **in the same notse** — this which is not the case here.

**Precautions :**
- **Division by zero**: protected by `IF( ['Gross Revenue'] <> 0 )`
- **Members to create**: the deux members must exister in the dimension before deployment
- **Deployment via TM1 Architect** — not available via MCP (API REST TM1 does does not expose rule editing)
- **Test on sandbox** before of pousser in production

#### ⚡ With Bob
- **Time**: ~10 minutes (read members live + identification calculated members existing + generation + verification calculs)
- **What is automated**: read of the real members and leurs values, identification rules existing, generation deux rules with the bonne syntax, verification numeric
- **Quality**: rules ready to deploy, members sources confirmed live, results calculated manually and verified

#### 🐢 Without Bob
- **Time**: 1 to 2 hours (documentation members, write, test)
- **Required tools**: TM1 Architect, documentation members of dimension
- **Risks**: wrong name of member (error silent), omission protection division by zero, doublon with rule existing

#### 💡 Discussion points

**Q: How can these rules be tested without affecting production data?**
> TM1 rules are in-memory calculations — they do not modify stored data, they calculate on the fly when read. You can therefore deploy it in TM1 Architect on the UAT environment without write risk. To validate, create a test view in PA and check `Contribution Margin = Gross Revenue - Direct COGS` on a few representative lines.

**Q: What is the difference between Contribution Margin % and Gross Margin %?**
> `Gross Margin % = (Gross Revenue - Cost of Sales) / Gross Revenue` — includes **all** the costs (directs + indirects). `Contribution Margin % = (Gross Revenue - Direct COGS) / Gross Revenue` — excludes the indirect costs. On 24Retail, the difference is 32.86% vs. 24.29%, or 8.57 points due to Indirect COGS (distribution overhead, fixed costs). Contribution Margin % is useful for product pricing decisions because it shows variable-cost coverage before fixed-cost allocation.

---

#### 🔍 Verification on the server — Exercise 3.1

> ✅ **Checks executed live via MDX on 24Retail.**
> ℹ️ The TM1 rules cannot be deployed via MCP — the checks focus on the data sources and the consistency of the calculs attendus.

**Step 1 — Confirmer the values sources (Direct COGS and Gross Revenue)**

Tool MDX :
```sql
SELECT { [Month].[Month].[Jan], ..., [Month].[Month].[Dec] } ON COLUMNS,
{ [Revenue].[Revenue].[Gross Revenue],
  [Revenue].[Revenue].[Direct COGS],
  [Revenue].[Revenue].[Indirect COGS] } ON ROWS
FROM [Revenue]
WHERE ( [organization].[organization].[101], [Channel].[Channel].[10],
        [product].[product].[21002], [Year].[Year].[Y1], [Version].[Version].[Forecast] )
```

✅ **Live result** — 12 months uniformes :

| | Jan–Dec (uniforme) |
|---|---|
| Gross Revenue | 14,000 |
| Direct COGS | 9,400 |
| Indirect COGS | 1,200 |

---

**Step 2 — Verification manual of the values attendues**

| Calcul | Formule | Result |
|---|---|---|
| Contribution Margin | 14,000 − 9,400 | **4,600 ✅** |
| Contribution Margin % | 4,600 / 14,000 × 100 | **32.86% ✅** |
| Gross Margin % existing | 3,400 / 14,000 × 100 | 24.29% (already calculated) |
| Difference | impact Indirect COGS | 1,200 / 14,000 × 100 = 8.57 pts |

---

**Step 3 — Confirmer that Contribution Margin is absent (not of doublon)**

✅ **Live result**: `Contribution Margin` and `Contribution Margin %` are absent of the dimension Revenue — the rules can be deployed without risk of doublon.

---

| Verification | Via | Actual result |
|---|---|---|
| ✅ Members sources (`Gross Revenue`, `Direct COGS`) | MDX live | Confirmed, 14,000 and 9,400 jan–dec |
| ✅ Values attendues calculated | Verification manual | CM=4,600; CM%=32.86% ✅ |
| ✅ Pas of doublon | `get_cube_sample_members` | `Contribution Margin` absent — rule new |
| ℹ️ Deployment rules | MCP | Non available — use TM1 Architect |

---

### Exercise 3.2 — Conversion of currency multi-cube

> 🔴 Level: Advanced | ⏱ ~15 min

#### 🎯 Objective
Show that Bob generates a rule TM1 which reference a autre cube (Exchange Rates) for a conversion of currency time real — a pattern fondamental in PA international.

#### 📋 Context
24Retail is a groupe international. The data Revenue are input in currency locale. The reporting groupe must be in USD. The rule must read the rate from the cube Exchange Rates.

#### 💬 Prompt to run

```
Generate a TM1 rule for the 24Retail Revenue cube that converts values
from local currency to USD by reading exchange rates from the "Exchange Rates"
cube on the same server. First analyze the structure of the Exchange Rates cube
to identify the correct dimensions and members. Then generate the complete rule
with comments and the associated FEEDER.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob consulte the structure of the cube Exchange Rates on 24Retail
3. Bob generates the rule with the fonction `DB()` and the feeder
4. Compare with the result of reference

#### ✅ Reference result

> ✅ **Exercise tested live on 24Retail during guide drafting.**

**Structure of the cube Exchange Rates confirmed live :**

| Dimension | Members |
|---|---|
| `Currency` | `USD`, `CDN`, `GBP` |
| `Year` | `Y1`–`Y4` |
| `Month` | `Jan`–`Dec`, `Year`, `Q1`–`Q4` |
| `Exchange Rate` | `ExchangeRate` (member unique — value scalaire of the rate) |
| `Exchange Rate Type` | `Average`, `Month End`, `Spot` |
| `Version` | shared with Revenue |

**Rate confirmed live — Forecast, Y1, Average :**

| Currency | Rate | Application in Revenue |
|---|---|---|
| `USD` | 1.000 (stable jan–Dec) | Orgs 101–302 — not of conversion required |
| `CDN` | 0.900 (stable jan–Dec) | Orgs 401–403 (Canada) |
| `GBP` | 0.600 (stable jan–Dec) | No org Revenue in GBP on 24Retail |

> ⚠️ **Architecture constraint**: Revenue does not have of dimension `Currency`. Currency is determined by the organization: `SUBST(!organization, 1, 1) @= '4'` identifies Canadian organizations. The rule target a **new member `Gross Revenue USD`** (to create) — do not write on `['Gross Revenue']` which is calculated by rule existing.

**Value of verification (org 401, Jan, Forecast, Y1) :**
- Gross Revenue CDN = **1,584,490** (lu live)
- Rate CDN = **0.900** (lu live)
- Gross Revenue USD expected = 1,584,490 × 0.9 = **1,426,041**

---

**Rule TM1 generated — Conversion Local → USD :**
```
SKIPCHECK;

# ============================================================
# Currency conversion: Local Gross Revenue → USD
# Exchange-rate source cube: Exchange Rates
# Exchange Rates dimensions (order confirmed live):
#   Currency, Year, Month, Exchange Rate, Exchange Rate Type, Version
#
# Revenue does not have a Currency dimension.
# Currency is determined by organization:
#   org 4xx (Canada) → CDN → confirmed CDN rate = 0.900
#   org 1xx, 2xx, 3xx (USA) → USD = 1.0 → returns local Gross Revenue
#
# Prerequisite: create the member 'Gross Revenue USD'
#             in the Revenue dimension before deployment
# ============================================================

['Gross Revenue USD'] = N:
  IF( SUBST( !organization, 1, 1 ) @= '4',
      DB( 'Exchange Rates', 'CDN', !Year, !Month,
          'ExchangeRate', 'Average', !Version )
      * ['Gross Revenue'],
      ['Gross Revenue'] );
```

**FEEDER generated :**
```
# Feeder: each Gross Revenue cell feeds Gross Revenue USD
# (source = Gross Revenue, target = Gross Revenue USD in the same dimension)
['Gross Revenue'] => ['Gross Revenue USD'];
```

> 💡 **Precaution rate = 0**: if `DB('Exchange Rates', ...)` returns 0 (rate absent), `Gross Revenue USD` = 0 silently. For a robustness maximale, use: `IF( DB(...) <> 0, DB(...) * ['Gross Revenue'], ['Gross Revenue'] )` — returns the value locale intacte if the rate is absent.

#### ⚡ With Bob
- **Time**: ~10 minutes (analysis Exchange Rates + Revenue + generation rule + feeder + verification values live)
- **What is automated**: analysis of the two cubes, mapping of the dimensions, identification constraint Currency/Organization, generation rule DB() and of the feeder, calcul of verification
- **Quality**: rule with members confirmed live, value of verification calculated, feeder correct

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (analysis of the two models + write + debugging of the feeders)
- **Required tools**: TM1 Architect, documentation two cubes
- **Risks**: wrong feeder → zeros ghost in production, write on `['Gross Revenue']` calculated → conflit of rules

#### 💡 Discussion points

**Q: Why is the feeder as important as the rule itself?**
> In TM1 sparse, a cell that is not "fed" is never visited by the calculation engine — it returns 0, even if the rule is correct. The feeder tells TM1 "this cell *will* have a non-null value, calculate it". Without a feeder, the rule is stillborn. This is why rule code without feeders is as dangerous as code without rules: it compiles, generates no error, but the results are silently wrong.

**Q: What happens if the cube Exchange Rates does not have a rate for a combination org/month?**
> The fonction `DB()` returns 0 if the cell target is blank. The conversion rule then becomes `Gross Revenue × 0 = 0` — all Canada value silently disappears. Protection: `IF( DB('Exchange Rates', ...) <> 0, DB(...) * ['Gross Revenue'], ['Gross Revenue'] )` — laisse the value locale intacte if the rate is absent.

---

#### 🔍 Verification on the server — Exercise 3.2

> ℹ️ The TM1 rules cannot be deployed via MCP. The checks focus on the data sources of the two cubes and the value of conversion calculated.

**Step 1 — Confirmer the rate in Exchange Rates**

```sql
SELECT { [Month].[Month].[Jan], ..., [Month].[Month].[Dec] } ON COLUMNS,
{ [Currency].[Currency].[USD], [Currency].[Currency].[CDN], [Currency].[Currency].[GBP] } ON ROWS
FROM [Exchange Rates]
WHERE ( [Year].[Year].[Y1], [Exchange Rate].[Exchange Rate].[ExchangeRate],
        [Exchange Rate Type].[Exchange Rate Type].[Average], [Version].[Version].[Forecast] )
```

✅ **Live result**: USD=1.0, CDN=0.9, GBP=0.6 — stables on the 12 months.

---

**Step 2 — Confirmer Gross Revenue for a org Canada**

```sql
SELECT { [Month].[Month].[Jan] } ON COLUMNS,
{ [Revenue].[Revenue].[Gross Revenue] } ON ROWS
FROM [Revenue]
WHERE ( [organization].[organization].[401], [Channel].[Channel].[Channel Total],
        [product].[product].[Product Total], [Year].[Year].[Y1], [Version].[Version].[Forecast] )
```

✅ **Live result**: Gross Revenue org 401, Jan = **1,584,490**

---

**Step 3 — Verification manual of conversion**

| Calcul | Formule | Result |
|---|---|---|
| Gross Revenue USD (Canada) | 1,584,490 × 0.900 | **1,426,041 ✅** |
| Gross Revenue USD (USA) | = Gross Revenue (rate = 1.0) | **identique** |

---

**Step 4 — Verification after deployment (GUI required)**

> After avoir created the member `Gross Revenue USD` and deployed the rule in TM1 Architect :
> 1. Ouvrir a vue Revenue in PA Workspace
> 2. Ajouter `Gross Revenue` and `Gross Revenue USD` in lines
> 3. Org 401, Jan, Forecast → check that `Gross Revenue USD` = 1,426,041
> 4. Org 101, Jan, Forecast → check that `Gross Revenue USD` = `Gross Revenue` (not of conversion)

---

| Verification | Via | Actual result |
|---|---|---|
| ✅ Structure Exchange Rates | `get_cube_dimensions` + `get_cube_sample_members` | 6 dimensions, members confirmed |
| ✅ Rate CDN = 0.9 | MDX live | Stable jan–Dec, Forecast Y1 |
| ✅ Gross Revenue Canada | MDX live | org 401, Jan = 1,584,490 |
| ✅ Value USD calculated | Verification manual | 1,584,490 × 0.9 = 1,426,041 ✅ |
| ℹ️ Deployment rule | MCP | Non available — TM1 Architect required |
| ⬜ Verification after deployment | PA Workspace | TO do after creation member + deployment rule |

---

### Exercise 3.3 — Variance Budget vs Actual

> 🟢 Level: Beginner | ⏱ ~10 min

#### 🎯 Objective
Quickly generate the rules of variance between versions — a universal need in any PA budgeting project.

#### 📋 Context
The customer wants see automatiquement in son cube Revenue the variances between the Budget (Version 1) and the Actuals (Actual) in value absolue and in pourcentage.

#### 💬 Prompt to run

```
Generate TM1 rules for the 24Retail Revenue cube that automatically calculate:
1. The "Variance" member = Actual - Version 1 (Budget)
2. The "Variance%" member = (Actual - Version 1) / Version 1 × 100
Use the real member names from the 24Retail Version dimension.
Protect against division by zero. Add the required feeders.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob verifies the members of the dimension Version on 24Retail
3. Bob generates the rules and feeders
4. Compare with the result of reference

#### ✅ Reference result

**Members Version verified live on 24Retail** *(MDX executed on the server)* :
- Budget = `Version 1` (alias "Budget")
- Actuals = `Actual`
- Members Variance existing: `Variance`, `Variance%` — **already calculated by of the rules existing**
- Members of advanced variance present also: `Volume Variance`, `Rate Variance`, `Ccy Exchange Variance`

**Data live — org 101, Ch 10, prod 21002, Jan Y1 :**

| Version | Gross Revenue | Units Sold |
|---|---|---|
| Version 1 (Budget) | 19,530 | 279 |
| Actual | 17,231 | 246 |
| **Variance** | **(2,299)** | **33** |
| **Variance%** | **(12)** | **12** |

**Data live — Total Company, Channel Total, Product Total, Year Y1 :**

| Version | Gross Revenue | Units Sold |
|---|---|---|
| Version 1 (Budget) | 76,993,682 | 204,474 |
| Actual | 94,246,460 | 249,487 |
| **Variance** | **17,252,778** | **(45,013)** |
| **Variance%** | **22** | **(22)** |

> **Note on the sign observed**: The Variance consolidated Gross Revenue = +17,252,778 (Actual > Budget = favorable revenue). The Variance Units Sold = (45,013) negative (Actual < Budget in volume). Convention confirmed: **Variance = Actual − Version 1** (positive = Actual greater to the budget).

**TM1 Rules generated :**
```
# Budget vs Actual variance — 24Retail Revenue cube
# Version 1 = Budget (alias), Actual = Actuals
# IMPORTANT: these rules already exist on this cube — code generated for reference

SKIPCHECK;

# Absolute variance
# [Variance] = Actual - Budget
['Variance':] = N:
  DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Actual', !Revenue )
  - DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Version 1', !Revenue );

# Percentage variance
# [Variance%] = (Actual - Budget) / |Budget| × 100
['Variance%':] = N:
  IF( DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Version 1', !Revenue ) <> 0,
      ( DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Actual', !Revenue )
        - DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Version 1', !Revenue ) )
      \ ABS( DB( 'Revenue', !organization, !Channel, !product, !Month, !Year, 'Version 1', !Revenue ) )
      * 100,
      0 );
```

**FEEDERS generated :**
```
# Feeder Variance: fed by any non-null value en Actual
['Actual':] => ['Variance':];

# Feeder Variance%: fed by any non-null value en Version 1 (Budget)
['Version 1':] => ['Variance%':];
```

**Note Bob**: *"The alias 'Budget' of Version 1 can be confusing. The rule uses the real ID `Version 1` for avoid the problems if the alias changes. The sign of Variance is positive when Actual > Budget (favorable for the revenue, unfavorable for the costs) — to ajuster selon the convention comptable of the customer."*

#### ⚡ With Bob
- **Time**: ~2 minutes
- **What is automated**: verification live of the names of members, syntax of the rules, feeders, values of control
- **Quality**: code ready to copy in TM1 Architect + verification live that the rules produce consistent results

#### 🐢 Without Bob
- **Time**: 30 min to 1 hour
- **Risks**: wrong name of version ("Budget" vs "Version 1"), feeder omission on Variance%

#### 💡 Discussion points

**Q: Are these rules sufficient, or do consolidated members also need variance rules?**
> For consolidations (Total Company, Channel Total), TM1 automatically aggregates leaf members, so the variance rule on leaf members rolls up correctly. However, if some consolidated members have specific rules that override standard consolidation, feeders must also be added there. Best practice: test variance on `Total Company` and `Channel Total` to confirm that the roll-up is correct.

**Q: How should the case where Actual is not yet entered be managed at the beginning of the year?**
> The rule `Variance = Actual - Version 1` will return `-Version 1` (the variance = 100% unfavorable) if Actual is blank — which is misleading. Solution: protect the rule by `IF( DB(..., 'Actual', ...) <> 0 OR ATTRS('Month', !Month, 'IsActual') = 'Y', ..., 0 )`. Alternatively, use an attribute of the Month dimension to identify "actualized" months and condition the display of the variance.

#### 🔍 Verification on the server — Exercise 3.3

**Query MDX of verification executed :**
```mdx
SELECT { [Revenue].[Revenue].[Gross Revenue], [Revenue].[Revenue].[Units Sold] } ON COLUMNS,
{ [Version].[Version].[Version 1], [Version].[Version].[Actual],
  [Version].[Version].[Variance], [Version].[Version].[Variance%] } ON ROWS
FROM [Revenue]
WHERE ( [organization].[organization].[101], [Channel].[Channel].[10],
        [product].[product].[21002], [Month].[Month].[Jan], [Year].[Year].[Y1] )
```

**Live result confirmed** :
- `Variance` = (2,299) on Gross Revenue → formule Actual − Version 1 = 17,231 − 19,530 = ✅
- `Variance%` = (12) → (−2,299 / 19,530) × 100 ≈ −11.8% → arrondi to −12 ✅
- Convention of sign: **Variance negative = Actual lower to the Budget** ✅

**Constat important**: The rules Variance and Variance% **existent already** on the cube Revenue of 24Retail. The members `Volume Variance` and `Rate Variance` are also calculated. Bob does not have need of deploy it — but can the regenerate for documenter or adapter to a autre cube.

**Deployment**: Rules non deployed (already existing on this cube). Code generated available as reference for other cubes.

---

### Exercise 3.4 — Feeders: explanation and generation

> 🔴 Level: Advanced | ⏱ ~20 min

#### 🎯 Objective
Illustrate the value of Bob for explain and generate the feeders TM1 — notably the hardest concept to master for the developers PA juniors.

#### 📋 Context
A developer junior has written the rules of Gross Margin % and Run Rate but forgotten the feeders. The cells display zeros in production. You must correct and explain.

#### 💬 Prompt to run

```
For the following TM1 rules in the 24Retail Revenue cube:
1. ['Gross Margin %'] = N: IF(['Gross Revenue'] <> 0, ['Gross Margin'] \ ['Gross Revenue'] * 100, 0);
2. ['Run Rate'] = ... (annual projection rule)
Generate the correct FEEDERS and explain in detail:
- Why each feeder is required
- What happens without a feeder (the "phantom zeros" problem)
- However to verify that a feeder is active in production
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob generates the feeders with teaching explanations
3. Bob explains the mechanism of the zeros ghost
4. Compare with the result of reference

#### ✅ Reference result

**Feeders generated for the rules of the cube Revenue — 24Retail :**

```
# === FEEDERS — cube Revenue, 24Retail ===

# Target rule :
# ['Gross Margin %'] = N: IF(['Gross Revenue'] <> 0,
#                              ['Gross Margin'] \ ['Gross Revenue'] * 100, 0);

# Main feeder — Gross Revenue is the denominator (control cell)
# → each non-null Gross Revenue cell can generate Gross Margin %
['Gross Revenue':] => ['Gross Margin %':];

# Safety feeder — in case Gross Margin is non-null while Gross Revenue = 0
# (defense in depth — avoids residual phantom zeros)
['Gross Margin':] => ['Gross Margin %':];

# Note: the 'Run Rate' member does NOT exist in the 24Retail Revenue dimension.
# If this member were added, the Run Rate feeder would follow this pattern:
# ['Actual':Version] => DB( 'Revenue', !organization, !Channel, !product,
#                            'YTD', !Year, 'Actual', 'Run Rate' );
```

**Explanation educational Bob :**

**Why the feeders are required?**
TM1 is a *sparse* engine: it stores only the cells non-empty and skips cells that it considers potentially empty during calculations. For a rule `N:` (level leaf calculated), TM1 must savoir *a priori* that a cell can be non-nulle for the "visiter". Without feeder, TM1 ne "visite" never the cell → it reste to zero same if the rule is syntactically correct.

**The problem of the "zeros ghost" :**
1. `Gross Revenue` = 17 231 for the product 21002 (Massachusetts / Retail / Jan / Y1 / Actual)
2. The rule `Gross Margin %` is defined and syntactically correct
3. In production, the cell `Gross Margin %` affiche 0
4. Cause: not of feeder → TM1 ne visite never the cell

**However check has feeder is actif :**
- **MDX direct**: if the derived cell returns a value non-nulle for data existing source data → feeder active (confirmed live: 25,83 % for 21002)
- **TM1 Architect**: right-click the cell calculated → "Show Feeder" → list the cells sources and the path of rule
- **Vue Feeders**: View > Feeders on the cube → list toutes the rules feeder actives
- **Diagnostic server**: `DebugMode=4` in `tm1s.cfg` active the feeder tracing in the logs (to use with precaution in production)

#### ⚡ With Bob
- **Time**: ~3 minutes for the feeders + explanation complete
- **What is automated**: generation feeders, explanation educational, conseils of diagnostic
- **Quality**: feeders correct feeders + embedded documentation

#### 🐢 Without Bob
- **Time**: 2 to 4 hours of debugging for a developer junior
- **Required tools**: documentation TM1, access to the logs server, TM1 Architect
- **Risks**: zeros ghost in production invisibles for the users, performance degraded

#### 💡 Discussion points

**Q: However Bob can-it aider to former a developer junior on the feeders?**
> Bob is particularly useful for training because it generates text explanations adapted to the requested level. A junior can ask follow-up questions: *"Explain to me why this feeder uses `!Version` and not `'Actual'` hard-coded"* or *"Show me an example of feeder which causes a phantom zero and how to correct it"*. It is a tutor available 24h/24 which never gets tired of basic questions.

**Q: The feeders generated cover-ils all the case (consolidations, vues of comparaison)?**
> The feeders generated cover the case standards. What can manquer: (1) the feeders for the vues in "comparaison of versions" where deux versions are on the same line — in this case, the feeders must "feed" from the deux versions, (2) the feeders for the members consolidated calculated by other rules in amont. Rule or: test the feeders in activant the "feeder tracing" in TM1 Architect after deployment.

#### 🔍 Verification on the server — Exercise 3.4

**Verification live of the feeders — cube Revenue, 24Retail :**

MDX executed: `Gross Margin %` for 3 products, Massachusetts / Retail / Jan / Y1 (2025) / Actual.

| Product | Gross Revenue | Gross Margin | Gross Margin % | Feeder actif? |
|---|---|---|---|---|
| 21002 — 5G 128Gb | 17 231 | 4 451 | **25,83 %** | ✅ |
| 21001 — 5G 256Gb | 15 209 | 4 138 | **27,21 %** | ✅ |
| 31001 — SP 2101 | 21 146 | 4 129 | **19,53 %** | ✅ |

**Control calcul**: 4 451 / 17 231 × 100 = 25,83 % ✅ — the rule `N: IF(['Gross Revenue'] <> 0, ['Gross Margin'] \ ['Gross Revenue'] * 100, 0)` is correct and the feeders are active.

**Constat Run Rate**: the member `Run Rate` does not exist **not** in the dimension Revenue of 24Retail. The feeder DB() multi-dim of the prompt original is a sample of pattern to adapter — it ne correspond not to the structure actual of the cube.

**Deployment**: Rules non deployed (already existing). The feeders are already in place. The code generated sert of model educational for tout new cube project.

---

## Domain 4 — Integration

> **Relevant BUILD role**: Developer TI, Architecte technical
> **Situation**: Connexion of PA to the systems sources (ERP, files, API)

### Exercise 4.1 — Mapping source SAP → cube PA

> 🟡 Level: Intermediate | ⏱ ~15 min

#### 🎯 Objective
Show that Bob can generate the mapping between a export system source and a cube TM1, then produce the TI processes load correspondant.

#### 📋 Context
The customer has been using SAP FI/CO. Each month, a extract CSV is generated with the data comptables. You must create the process load to 24Retail.

#### 💬 Prompt to run

```
I receive an SAP export with the following columns: CompanyCode, MaterialNumber,
FiscalPeriod, FiscalYear, PostingAmount, Quantity, CostCenter.
Generate:
1. The documented mapping between these columns and the dimensions of the 24Retail Revenue cube
   (include the required transformations: codes → TM1 members, fiscal periods → months)
2. The complete TI loading process using this mapping
3. The rejection cases to handle (non-existent member, zero amount, invalid period)
```

> 📁 **File CSV sample**: [`sample-data/sap_export_sample.csv`](sample-data/sap_export_sample.csv) — 29 lines export SAP simulated including 3 case of reject intentionals (CompanyCode unknown `XXXX`, MaterialNumber invalid `MAT-INVALID`, MaterialNumber blank). Parfait for test the management of the errors TI processes.

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob analysis the structure of the cube Revenue on 24Retail
3. Bob generates the mapping + the TI processes
4. Test with the file [`sample-data/sap_export_sample.csv`](sample-data/sap_export_sample.csv) — check that the 3 lines invalid are rejected
5. Compare with the result of reference

#### ✅ Reference result

**Mapping SAP → Cube Revenue 24Retail** *(based on the dimensions real — verified MCP)*

**Table of mapping documented :**

| Column SAP | Dimension Revenue | Transformation | Status |
|---|---|---|---|
| `CompanyCode` | `organization` | Lookup via attribut `SAPCode` on the dim organization (ex: `1000` → `100` East Region). Attribut `SAPCode` to create on 24Retail if absent. | ⚠️ Mapping required |
| `MaterialNumber` | `product` | Lookup via attribut `MaterialNumber` on the dim product (ex: `MAT-21002` → `21002`). IDs actual: `21002`, `21001`, `31001`… | ⚠️ Mapping required |
| `FiscalPeriod` | `Month` | `001`→`Jan`, `002`→`Feb`… `012`→`Dec` — inline in the TI | ✅ Direct |
| `FiscalYear` | `Year` | `2025`→`Y1`, `2026`→`Y2`, `2027`→`Y3`, `2028`→`Y4` (Y1=2025 confirmed live) | ✅ Direct |
| `PostingAmount` | `Gross Revenue` | Direct — attention to the sign (charges SAP in negative, Revenue TM1 positive) | ✅ Direct |
| `Quantity` | `Units Sold` | Direct (Units Sold = 246 for 21002/Jan/Y1/Actual confirmed) | ✅ Direct |
| `CostCenter` | Non mapped | No equivalent in Revenue 24Retail — ignored in the process | ❌ Non mapped |
| *(absent)* | `Channel` | Absent of the CSV SAP → parameter `pChannel` (ex: `Channel Total`) | ⚠️ Parameter |
| *(absent)* | `Version` | Parameter `pVersion` (ex: `Actual`) | ⚠️ Parameter |

**Case of reject identified :**
1. `CompanyCode` unknown → `ATTRS()` returns `''` → `ItemReject`
2. `MaterialNumber` unknown → `ATTRS()` returns `''` → `ItemReject`
3. `FiscalPeriod` hors plage `001`–`012` → `vMonth = ''` → `ItemReject`
4. `FiscalYear` hors 2025–2028 → `vYear = ''` → `ItemReject`
5. `PostingAmount` blank → montant manquant → `ItemReject`

**Extrait TI processes load :**
```
# Data procedure — SAP to Revenue mapping

# 1. Map FiscalPeriod → TM1 Month (IF/ENDIF — compatible with all TM1 versions)
vMonth = '';
IF ( V3 @= '001' ); vMonth = 'Jan'; ENDIF;
IF ( V3 @= '002' ); vMonth = 'Feb'; ENDIF;
# ... 003→Mar … 012→Dec
IF ( vMonth @= '' );
  ItemReject( 'Invalid FiscalPeriod: ' | V3 );
  ItemSkip;
ENDIF;

# 2. Map FiscalYear → TM1 Year
vYear = '';
IF ( V4 @= '2025' ); vYear = 'Y1'; ENDIF;
IF ( V4 @= '2026' ); vYear = 'Y2'; ENDIF;
IF ( V4 @= '2027' ); vYear = 'Y3'; ENDIF;
IF ( V4 @= '2028' ); vYear = 'Y4'; ENDIF;
IF ( vYear @= '' );
  ItemReject( 'FiscalYear out of range: ' | V4 );
  ItemSkip;
ENDIF;

# 3. Lookup CompanyCode → organization (SAPCode attribute required on the dimension)
vOrg = ATTRS( 'organization', V1, 'SAPCode' );
IF ( vOrg @= '' );
  ItemReject( 'Unknown SAP CompanyCode: ' | V1 );
  ItemSkip;
ENDIF;

# 4. Lookup MaterialNumber → product (MaterialNumber attribute required on the dimension)
vProduct = ATTRS( 'product', V2, 'MaterialNumber' );
IF ( vProduct @= '' );
  ItemReject( 'Unknown SAP MaterialNumber: ' | V2 );
  ItemSkip;
ENDIF;

# 5. Validate PostingAmount
IF ( V5 @= '' );
  ItemReject( 'PostingAmount is empty' );
  ItemSkip;
ENDIF;

# 6. Loading — dimension order: organization, Channel, product, Month, Year, Version, Revenue
CellPutN( StringToNumber( V5 ), 'Revenue', vOrg, pChannel,
          vProduct, vMonth, vYear, pVersion, 'Gross Revenue' );
CellPutN( StringToNumber( V6 ), 'Revenue', vOrg, pChannel,
          vProduct, vMonth, vYear, pVersion, 'Units Sold' );
# V7 = CostCenter → ignored
```

> **Alternative FiscalPeriod**: the fonction `MAP()` TM1 is more concise but less lisible and not available in certaines versions anciennes :
> `vMonth = MAP( V3, '001', 'Jan', '002', 'Feb', '003', 'Mar', '004', 'Apr', '005', 'May', '006', 'Jun', '007', 'Jul', '008', 'Aug', '009', 'Sep', '010', 'Oct', '011', 'Nov', '012', 'Dec' );`

#### ⚡ With Bob
- **Time**: ~5 minutes
- **What is automated**: analysis of the model target, generation mapping, identification transformations
- **Quality**: mapping documented + code TI with management of the rejects

#### 🐢 Without Bob
- **Time**: 4 to 8 hours (analysis SAP + analysis PA + drafting of the mapping + code TI)
- **Required tools**: documentation SAP, TM1 Architect, Excel for the mapping
- **Risks**: mapping incomplet, codes SAP non traduits → rejects silent in production

#### 💡 Discussion points

**Q: Quelles informations manquent in the prompt so that the mapping either complete?**
> The ideal prompt would provide: (1) a line sample of the CSV SAP with real values (ex: `1000, MAT-21002, 001, 2025, 15000.00, 120, COST-500`), (2) the mapping table CompanyCode SAP → Organization TM1, (3) the table MaterialNumber → product TM1, (4) the convention of sign (SAP charges are negative, the revenue in positive — reversed in TM1). Without these 4 items, the mapping remains theoretical. See the file `sap_export_sample.csv` in Appendix E for a complete sample.

**Q: How does Bob handle TM1 members that do not have an SAP equivalent?**
> Bob generates an `ItemReject` for unknown codes — a secure approach, but one that can generate many rejects during a first load. Another possible strategy is to write unknown lines to a quarantine cube (`Revenue_Staging`) for later manual processing instead of rejecting them. Bob can generate this alternative logic if it is specified in the prompt.

#### 🔍 Verification on the server — Exercise 4.1

**Verification dimensions Revenue via MCP :**

Dimensions of the cube Revenue (order real): `organization`, `Channel`, `product`, `Month`, `Year`, `Version`, `Revenue` — confirmed with `get_cube_dimensions`.

**Mapping SAP → Revenue validated on real data 24Retail :**

| Point of verification | Value live | Status |
|---|---|---|
| `Year Y1` = 2025 | Alias `2025` confirmed via `get_cube_sample_members` | ✅ |
| `organization` IDs | Plage `100`–`403` (not of format `1000` SAP native) | ✅ Mapping external required |
| `product` IDs | Numeric: `21002`, `21001`, `31001`… | ✅ Mapping external required |
| `Month` members | `Jan`…`Dec`, `Q1`…`Q4`, `Year` — month TM1 = names anglais courts | ✅ |
| `Units Sold` loadable | 246 for 21002 / Massachusetts / Retail / Jan / Y1 / Actual | ✅ |
| `Load_Revenue_FromCSV` | Process existing — parameters: `pFilePath`, `pVersion`, `pDelimiter`, `pClearBeforeLoad` | ✅ Existe |
| `Load_SAP_Revenue` | Non existing — to create for the mapping SAP | ⚠️ TO create |

**Deployment**: Process `Load_Revenue_FromCSV` already deployed on 24Retail (see Ex 2.1). The process `Load_SAP_Revenue` (with mapping CompanyCode/MaterialNumber) is to create. The datasource ASCII is not configurable via MCP — configuration manual required in PA Modeler.

---

### Exercise 4.2 — Management robust of the errors ETL

> 🔴 Level: Advanced | ⏱ ~15 min

#### 🎯 Objective
Show that Bob enrichit a TI process existing hasvec a error handling production-grade — pattern indispensable for a mise in production secured.

#### 📋 Context
The process load SAP → Revenue fonctionne in recette, but in production the data are parfois sales. It must a error handling robust before the mise in prod.

#### 💬 Prompt to run

```
Improve the SAP → Revenue TI loading process from the previous exercise by adding:
1. A counter for processed / rejected / error rows
2. Writing rejected rows to a timestamped log file
3. A ProcessBreak if the rejection rate exceeds 5%
4. A summary message in Epilog: "X rows loaded, Y rejected (Z%)"
5. Identify the user who launched the process (TM1User())
Provide the complete Prolog + Data + Epilog code.
```

#### 🔢 Steps
1. Copy the prompt in Bob (continuing from 4.1)
2. Bob generates the version enrichie of the process
3. Inspect the logic of comptage, log and threshold of reject
4. Compare with the result of reference

#### ✅ Reference result

**Process enrichi with error handling production-grade :**

**Prolog enrichi :**
```
# Counter initialization
vLineCount     = 0;
vLoadedCount   = 0;
vRejectedCount = 0;
vUser          = TM1User();        # String — directly concatenable
vTimestamp     = Now();            # ⚠️ Returns a Numeric on this server
                                   # → NumberToString() wrapper required

# Timestamped log file name
vLogFile = 'C:\PA\logs\Load_SAP_Revenue_'
         | NumberToString( vTimestamp )  # ← NumberToString required
         | '_' | vUser | '.log';

# Log header
ASCIIOUTPUT( vLogFile,
  'SAP->Revenue LOAD | User: ' | vUser
  | ' | ' | NumberToString( vTimestamp ) );
```

**Data enrichie (comptage + log) :**
```
vLineCount = vLineCount + 1;

# ... (existing mapping validations) ...

IF ( vOrg @= '' );
  vRejectedCount = vRejectedCount + 1;
  ASCIIOUTPUT( vLogFile, 'REJECT L.' | NumberToString( vLineCount )
               | ' | Unknown CompanyCode: ' | V1 );
  ItemSkip;   # ItemSkip alone is sufficient — ASCIIOUTPUT replaces ItemReject to avoid double logging
ENDIF;

# ... load ...
vLoadedCount = vLoadedCount + 1;
```

**Epilog — summary and control of the rate of reject :**
```
# Reject-rate calculation (operator \ = division TM1)
IF ( vLineCount > 0 );
  vRejectRate = vRejectedCount \ vLineCount * 100;
ELSE;
  vRejectRate = 0;
ENDIF;

# Summary log
ASCIIOUTPUT( vLogFile, '--- SUMMARY ---' );
ASCIIOUTPUT( vLogFile,
  'Rows read   : ' | NumberToString( vLineCount ) );
ASCIIOUTPUT( vLogFile,
  'Rows loaded: ' | NumberToString( vLoadedCount ) );
ASCIIOUTPUT( vLogFile,
  'Rows rejected: ' | NumberToString( vRejectedCount )
  | ' (' | NumberToString( vRejectRate ) | '%)' );

# Rejection threshold: ProcessBreak if > 5%
IF ( vRejectRate > 5 );
  ASCIIOUTPUT( vLogFile, 'ERROR: Rejection rate > 5% - ProcessBreak triggered' );
  ProcessBreak;
ENDIF;
```

#### ⚡ With Bob
- **Time**: ~3 minutes
- **What is automated**: logic of comptage, write of log, calcul of the rate of reject, summary Epilog
- **Quality**: pattern production-ready complete

#### 🐢 Without Bob
- **Time**: 2 to 4 hours additional after the code of basis
- **Risks**: error handling incomplete → loads silently partiels in production

#### 💡 Discussion points

**Q: The threshold of 5% is it appropriate? However make it parameterizable?**
> 5% is a default reasonable but which depends of the context: a load of data of reference (master data) should avoir 0% of reject tolerated; a load of transactions SAP can tolerate 1–2% maximum. For make it parameterizable, add `pRejectThreshold: Numeric = 0.05` in the parameters of the process and replace `> 0.05` by `> pRejectThreshold`. Bob can generate this exchange in 30 secondes on request.

**Q: Where storesr the file of log for that it either accessible to the team in production?**
> Trois options selon the infrastructure: (1) **Shared network folder**: path UNC accessible to all the members of the team, (2) **Subfolder of the directory TM1**: `<TM1DataDirectory>\Logs\` accessible via the server of files PA, (3) **Cube TM1 of log**: write the log in a cube TM1 dedicated (by ex. `ETL_Log` with dimensions ProcessName × RunDate × LogLine) — accessible directement from PA and interrogeable by Bob via MCP.

#### 🔍 Verification on the server — Exercise 4.2

**Syntax TM1 validated live on 24Retail :**

Test process `_test_syntax_42` created, tested, then deleted. Results :

| Fonction / Syntax | Live result | Status |
|---|---|---|
| `TM1User()` | Returns String — directly concatenable | ✅ |
| `Now()` without argument | Returns **Numeric** — `NumberToString()` mandatory for concatenation | ✅ (with wrapping) |
| `Now(1)` with argument | Error syntax: *Unexpected parenthesis* | ❌ Invalid |
| `'...' \| vTimestamp` direct (without wrap) | Error syntax *invalid operator* line ASCIIOUTPUT | ❌ Trap |
| `ASCIIOUTPUT( file, line )` | Fonctionne with concatenation string | ✅ |
| `vRejectedCount \ vLineCount * 100` | Division numeric TM1 | ✅ |
| `NumberToString()` | Conversion numeric → string | ✅ |
| `ProcessBreak` without parentheses | Valid | ✅ |

**Constat `ItemReject` vs `ItemSkip`**: use `ASCIIOUTPUT` + `ItemSkip` without `ItemReject` avoids a double log (TM1 writes already the reject in son log interne quand `ItemReject` is called).

**Deployment**: Code generated ready to integrate in `Load_Revenue_FromCSV` existing on 24Retail. Test process cleaned up of the server after validation.

---

## Domain 5 — Migration

> **Relevant BUILD role**: Consultant senior, Chef of project technical
> **Situation**: Migration of a existing model to a new version PA or a new server

### Exercise 5.1 — Audit of model before migration

> 🟡 Level: Intermediate | ⏱ ~15 min

#### 🎯 Objective
Show that Bob product a audit structured of a model PA existing, identifiant the risks and anti-patterns before a migration — without access to the code source.

#### 📋 Context
The customer wants migrer 24Retail to IBM Planning Analytics as a Service (Cloud). Before of start, you must livrer a report audit of the existing model.

#### 💬 Prompt to run

```
Audit the 24Retail server in preparation for a migration to IBM Planning Analytics as a
Service. Analyze model complexity (number of cubes, dimensions, processes),
detectable anti-patterns (naming, structure), dependencies between cubes,
critical vs. secondary processes, and risks specific to a Cloud migration.
Produce a structured audit report with an overall risk rating.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob queries the MCPs for collecter toutes the metadata of the server
3. Bob product the report audit with risks and recommandations
4. Compare with the result of reference

#### ✅ Reference result

**Report audit — 24Retail → Migration Cloud PA** *(metadata collected live via MCP)*

**Inventaire of the model**

| Composant | Number exact | Detail |
|---|---|---|
| Cubes total | **34** | 1 cube `METRICS` (`Metrics`), 1 without rules (`Forecast_Example`) |
| Cubes with active rules | **33** | All sauf `Forecast_Example` — files .rux non accessibles via MCP |
| Cubes with drillthrough | **5** | `Capital`, `Employee`, `Income Statement`, `Income Statement Reporting`, `Revenue` |
| Process TI total | **62** | including 15 `}tp_*` (workflow PA), 11 `}Drill_*` |
| Process without parameter | **13+** | Executable without confirmation — risk execution accidentelle |
| Process SPSS | **4** | `Export Dimensions/External Factors/UnitsSold to SPSS`, `Run SPSS Prediction` |
| Dimensions shared | 5+ | `organization`, `Month`, `Year`, `Version`, `Channel` |

**Anti-patterns detected :**
- 🔴 **Process destructeurs without confirmation**: `ResetRevenueData` (0 params), `reset_demo_FOPMImage` (0 params), `Reset Concert` (0 params), `}tp_admin_delete_all` — to disable or protect before production cloud
- 🔴 **Active SPSS dependencies**: 4 process depend IBM SPSS installed locally → to reconfigure or replace in Cloud
- ⚠️ **Naming with spaces**: 20+ cubes and process (`Add Product Vision`, `cube Task Workflow reset Status`, `Job Code Assumptions`, `Supply Chain`, `Line Item Detail`…) → limited portability in the Chores and scripts
- ⚠️ **Hardcoding obsolete**: `}Drill_RevenueCubeToSupplyChainCube` contains `Year: '2012'` in value by default
- ⚠️ **Cube orphelin likely**: `Forecast_Example` — alone cube without rules, usage non documented
- ⚠️ **Conventions of naming mixtes**: PascalCase, snake_case, CamelCase, names libres

**Dependencies inter-cubes identified (via drillthrough) :**
- `Revenue` ↔ `Income Statement` (bidirectionnel — `}Drill_Revenue`, `}Drill_Actuals`)
- `Revenue` ↔ `Supply Chain` (`}Drill_RevenueCubeToSupplyChainCube`)
- `Capital` → `Income Statement` (`}Drill_Capital`, `}Drill_Depreciation`)
- `Employee` → `Income Statement` (`}Drill_Employee`, `}Drill_JobCode/TypeAssumption`)
- `Income Statement` → `Income Statement Reporting` (`}Drill_Actuals_Reporting`)
- `Line Item Detail` ↔ `Income Statement` (`}Drill_LineItemDetail`, `}Drill_PhasedCosts`)
- Process `}tp_*` → cubes `Task Workflow`, `Version Copy Control`, `Version Subset Control`

**Risks migration PAaaS :**
1. 🔴 **33 cubes with rules .rux** — not exportable via MCP → migration manual file by file
2. 🔴 **Dependencies SPSS** — 4 process depend IBM SPSS local → reconfiguration mandatory
3. 🟡 **Paths files hardcoded** — `C:\PA\logs\`, `C:\temp\` → to adapter to the cloud storage
4. 🟡 **Chores** — non visibles via MCP → inventaire manual required before migration
5. 🟡 **15 process `}tp_*`** — compatibility version PAaaS target to check
6. 🟡 **5 cubes drillthrough** — rules `.drl` separate files to migrate manually
7. 🟡 **Process destructeurs without garde-fou** — to disable before mise in production cloud

**Note of risk globale: 🔴 HIGH**
*(33 cubes with rules not exportable via MCP + dependencies SPSS actives + destructive processes without confirmation = high migration complexity)*

#### ⚡ With Bob
- **Time**: ~5 minutes
- **What is automated**: metadata collection, analysis of the patterns, structuring of the report
- **Quality**: deliverable report customer, based on the real data

#### 🐢 Without Bob
- **Time**: 1 to 2 days (analysis manual + drafting)
- **Required tools**: TM1 Architect, Excel for inventaire, Word for report
- **Risks**: audit incomplet, risks non identified → mauvaises surprises in migration

#### 💡 Discussion points

**Q: Which risks can Bob not detect without the code source of the process and rules?**
> Risks invisibles without the code: (1) **Dangerous hardcodings** — names of members hard-coded in the rules that break during migration, (2) **Inefficient nested loops** in the TI processes, (3) **Recursive calls** between process (process A appelle process B which rappelle A), (4) **Connexions ODBC** with of the credentials in clair, (5) **Logic business incorrect** which fonctionne but donne of wrong results. For detect these risks, exporter the files .pro/.rux and the analyze with Bob.

**Q: However this report seconds compare to a audit manual actual by a consultant senior?**
> A consultant senior met 1–2 days to produce a report equivalent, but it y ajoute deux choses that Bob cannot provide: (1) the risks issus of the experience terrain (ex: "I have vu this pattern of naming causer of the problems on 3 projets similaires"), (2) the context organisationnel (turnover of the team customer, quality of the gouvernance data). The report Bob is the basis — the consultant senior enriches it with these couches experience.

#### 🔍 Verification on the server — Exercise 5.1

**Actual inventory 24Retail *(collected live via MCP — `get_tm1_cubes` + `get_tm1_processes`)* :**

| Composant | Number live | Source |
|---|---|---|
| Cubes total | **34** | `get_tm1_cubes` — counted exhaustivement |
| Cubes with rules (`hasRules: true`) | **33** | Alone `Forecast_Example` a `hasRules: false` |
| Cubes with drillthrough (`hasDrillthroughRules: true`) | **5** | `Capital`, `Employee`, `Income Statement`, `Income Statement Reporting`, `Revenue` — `Revenue Reporting` a `false` |
| Process TI total | **62** | `get_tm1_processes` — counted exhaustivement |
| Process `}tp_*` | **15** | Workflow Planning Analytics |
| Process `}Drill_*` | **11** | Drillthrough cubes critical |
| Process SPSS | **4** | `Export Dimensions/External Factors/UnitsSold to SPSS`, `Run SPSS Prediction` |
| Cubes with cell security | **0** | `hasCellSecurity: false` on all the cubes — confirmed |

**Anti-patterns confirmed live :**
- 🔴 `ResetRevenueData` (0 params), `reset_demo_FOPMImage` (0 params), `Reset Concert` (0 params) — executable without confirmation
- 🔴 4 process SPSS active — external dependency non visible in the result of reference initial
- ⚠️ 20+ names with espaces (cubes and process)
- ⚠️ `}Drill_RevenueCubeToSupplyChainCube`: value `Year: '2012'` hardcoded

**Correction applied**: cubes with drillthrough = **5** — `Capital`, `Employee`, `Income Statement`, `Income Statement Reporting`, `Revenue`. `Revenue Reporting` a `hasDrillthroughRules: false`. Risk rating raised to 🔴 HIGH (vs 🟡 MOYEN-HIGH initial) in raison dependencies SPSS and of the number real of rules.

---

### Exercise 5.2 — Plan of migration structured

> 🔴 Level: Advanced | ⏱ ~20 min

#### 🎯 Objective
Generate a plan of migration actionnable with the exact order of migration composants, the dependencies and the points of validation.

#### 📋 Context
The audit is validated. The customer has signed the project of migration. You must deliver a detailed migration plan for the kick-off.

#### 💬 Prompt to run

```
Generate a structured migration plan to move the 24Retail model to a new
IBM Planning Analytics server. The plan must include:
1. A complete inventory of components to migrate (cubes, dimensions, processes, views)
2. The recommended migration order with dependency justification
3. Validation steps at each phase (success criteria)
4. Risks by phase and rollback strategies
5. A go/no-go checklist before production deployment
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob analysis the dependencies between composants on 24Retail
3. Bob generates the plan structured
4. Compare with the result of reference

#### ✅ Reference result

**Plan of migration — 24Retail to new server PA** *(based on the dependencies real mapped via MCP)*

**Phase 0 — Preparation (J-10 to J-5)**
- [ ] Export metadata complete via MCP (`get_tm1_cubes`, `get_cube_dimensions` on the 34 cubes)
- [ ] Backup manual of the **33 files `.rux`** (rules actives — not exportable via MCP)
- [ ] Backup manual of the **62 files `.pro`** (TI processes)
- [ ] Backup of the **5 files `.drl`** (drillthrough: `Capital`, `Employee`, `Income Statement`, `Income Statement Reporting`, `Revenue`)
- [ ] 🔴 Decision on the **4 process SPSS**: reconfigurer the connexion or disable
- [ ] Inventaire of the Chores via TM1 Architect (non accessible via MCP)
- [ ] Inventaire of the connections ODBC and chemins files hardcoded (`C:\PA\`, `C:\temp\`)
- [ ] Disable / protect: `ResetRevenueData`, `reset_demo_FOPMImage`, `Reset Concert`

**Phase 1 — Infrastructure (J-5 to J-3)**
- [ ] Provisioning of the new server PA
- [ ] Configuration connections network and security
- [ ] Migration users and groupes
- [ ] Test of connexion MCP on the new server

**Phase 2 — Dimensions (J-2: Morning) — Order by sharing frequency**

*Vague 1 — Transversales (present in 10–13 cubes) :*
1. `Version` (13 cubes), `organization` (12), `Month` (11), `Year` (11)

*Vague 2 — Revenue & Supply Chain :*
2. `product` (4 cubes), `Channel` (4), `Revenue` (dim mesure), `Supply Chain Measures`, `RevenueAsmpt`

*Vague 3 — Income Statement & Finance :*
3. `Account` (3 cubes), `Currency`, `Exchange Rate`, `Exchange Rate Type`, `LineItemList`, `Allocation List`

*Vague 4 — Dimensions specific :*
4. `EmployeeList`, `CapitalList`, `Asset Types`, `Task`, `TrxID`, `Spread Methods`

**Phase 3 — Cubes (J-2: After-midi) — Order by couches of dependencies**

*Couche 1 — Without rules & control :*
- `Forecast_Example` (alone cube without rules), `Version Copy Control`, `Version Subset Control`, `Task Workflow`

*Couche 2 — Sources & reference data :*
- `Exchange Rates`, `Revenue Assumptions`, `Spread Methods`, `Calendar`, `FcstMethod`, `GLTransactions`, `Social Media`, `ExternalFactors`

*Couche 3 — Cubes calculated main :*
- `Supply Chain`, `Revenue` ★ (with rules + feeders), `Rate BOM`, `Phased Costs`

*Couche 4 — Aggregation & Finance :*
- `Income Statement` ★ (aggregates Revenue + Supply Chain + Exchange Rates), `Line Item Detail`, `Allocation Calculation`

*Couche 5 — RH & Immobilisations :*
- `Employee`, `Compensation`, `Benefit Assumptions`, `Job Code/Type Assumptions`
- `Capital`, `Asset Life`, `Depreciation`

*Couche 6 — Reporting & Metrics :*
- `Revenue Reporting`, `Income Statement Reporting`, `Compensation Reporting`, `Workflow Reporting`, `Metrics`

**Phase 4 — Process TI (J-1)**
- [ ] Process load dims: `}_intDPD_SimpleDimLoad_Account`, `}_intDPD_SimpleDimLoad_Version`
- [ ] Process load data: `Load_PYActuals`, `Load_Revenue_FromCSV` (adapter chemins files)
- [ ] Process calcul: `UpdateISFromRevenue`, `PLConsolide_Load_SupplyChain`
- [ ] Process version: `copy_version`, `copy_version_control`, `create_version`
- [ ] 15 process `}tp_*` (workflow PA) — check compatibility version PAaaS target
- [ ] 11 process `}Drill_*` (drillthrough)
- [ ] 🔴 4 process SPSS: reconfigurer connexion or replace
- [ ] Recreation Chores (inventaire Phase 0 required)
- [ ] Update chemins files hardcoded to the new server

**Phase 5 — Validation and Go/no-go (J-1: Soir)**

Criteria of success :
- ✅ 34 cubes accessibles — test MCP `get_tm1_cubes` on the new server
- ✅ Rules actives: `Gross Margin %` non nul on Revenue (feeder active — confirmed via MDX)
- ✅ Process keys tested: `Copy_Revenue_Units`, `UpdateISFromRevenue`, `copy_version`
- ✅ 5 drillthrough operational: `Capital`, `Employee`, `Income Statement`, `Income Statement Reporting`, `Revenue`
- ✅ Process destructeurs inaccessibles without protection
- ✅ Security: access to the bonnes versions by the bons groupes

**Procedure of rollback**: Keep the old server actif J+5. Fallbackr the connections front-end to the old server in < 30 minutes if anomaly critical post go-live.

#### ⚡ With Bob
- **Time**: ~5 minutes
- **What is automated**: inventaire, analysis of the dependencies, structuring of the plan
- **Quality**: plan deliverable customer with criteria of validation

#### 🐢 Without Bob
- **Time**: 2 to 3 days (analysis + ateliers + drafting)
- **Risks**: order of migration incorrect → errors in cascade, not of criteria of validation clairs → go/no-go subjectif

#### 💡 Discussion points

**Q: The plan is-it sufficiently detailed to be executed by a team without knowing 24Retail?**
> The phases 0 to 4 are sufficiently precise for a experienced PA developer. What is missing for a team without context: (1) the implicit dependencies (ex: `UpdateISFromRevenue` must be executed *after* the load Revenue but *before* the consolidation IS), (2) the estimated time by step (critical for plan maintenance windows of maintenance), (3) the contacts in case of problem on the new server. These items must be completed during a kickoff workshop.

**Q: Which items require human validation that Bob cannot perform?**
> (1) **Validation data**: Bob can generate the criteria but not compare the data visuellement between the deux servers, (2) **Test user**: the validation that the rapports PA are displayed correctly in the interface, (3) **Validation business**: that the chiffres of the P&L after migration correspondent to the chiffres attendus, (4) **Validation of security**: that the bons users ont the bons permissions — a verification humaine is indispensable before the go-live.

#### 🔍 Verification on the server — Exercise 5.2

**Mapping of the dependencies validated live via `get_cube_dimensions` on the cubes keys :**

| Cube | Dimensions real | Dependencies |
|---|---|---|
| `Revenue` | organization, Channel, product, Month, Year, Version, Revenue | Vague 2 dims required |
| `Supply Chain` | organization, Channel, product, Month, Year, Supply Chain Measures, Version | Same dims that Revenue |
| `Income Statement` | Currency Calc, organization, Year, Month, Account, Version | + Currency Calc, Account |
| `Exchange Rates` | Currency, Year, Month, Exchange Rate, Exchange Rate Type, Version | Dims propres |
| `Capital` | organization, CapitalList, Asset Types, Year, Version, Capital | Pas of Month |
| `Employee` | organization, EmployeeList, Year, Version, Employee | Pas of Month |
| `Compensation` | organization, EmployeeList, Month, Year, Version, Compensation | With Month |
| `Revenue Reporting` | organization, Channel, product, Month, Year, Revenue, Version | Identique to Revenue |
| `Version Copy Control` | LineItemList, Version Copy Measures | Dims propres — couche 1 ✅ |

**Corrections applied vs guide initial :**
- Phase 0: `33 fichiers .rux` (not 30) + `4 fichiers .drl` + `62 fichiers .pro` (not 60+) + process SPSS added
- Phase 3 drillthrough: **5 cubes** (`Capital`, `Employee`, `Income Statement`, `IS Reporting`, `Revenue`) — `Revenue Reporting` a `hasDrillthroughRules: false`
- `Income Statement` does not have a `Currency` directe — conversion currency is done via rules from `Exchange Rates`
- `Capital` and `Employee` do not have **not** of dimension `Month` — cubes annuels

---

## Domain 6 — Testing & Validation

> **Relevant BUILD role**: Developer TI, Consultant quality
> **Situation**: Phase of recette — check that the data and the process are corrects

### Exercise 6.1 — Detection anomalies in the data

> 🟢 Level: Beginner | ⏱ ~10 min

#### 🎯 Objective
Show that Bob detects automatiquement of the anomalies in the data of a cube PA — without write of query MDX.

#### 📋 Context
You viens load the data actuals of the exercise in 24Retail. Before of validate the load, you veux make sure that it there is no of outliers.

#### 💬 Prompt to run

```
Analyze the data in the 24Retail Revenue cube and detect outliers or inconsistencies:
margin outliers, abnormal price/cost ratios, products with zero volumes across the full year,
and inconsistencies between versions (Actual > Budget × 3).
Present the detected anomalies by priority order with a business explanation.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob executes a analysis on the data of the cube Revenue
3. Bob present the anomalies detected with context business
4. Compare with the result of reference

#### ✅ Reference result

**Analysis data Revenue 24Retail — Anomalies detected** *(real data, executed in live)*

**Revenue data consulted (Budget Y1 — Total Company) :**

| | Units Sold | Gross Revenue | Gross Margin % |
|---|---|---|---|
| Total Company | 204,474 | 76,993,682 | 42.94% |
| East Region (100) | 61,420 | 23,631,659 | 20.58% |
| Central Region (200) | 36,858 | 13,475,571 | 21.02% |
| West Region (300) | 44,276 | 17,354,584 | 21.37% |
| **Canada (400)** | **61,920** | **22,531,868** | **96.12%** |

**Analysis Gross Margin % by channel (Actual Y1) :**

| | Retail (10) | Internet (20) | Distribution (30) |
|---|---|---|---|
| East (100) | 21.37% | 21.57% | **(1.14%)** |
| Central (200) | 24.36% | 21.96% | **(1.31%)** |
| West (300) | 24.09% | 23.06% | **(0.98%)** |
| **Canada (400)** | **96.09%** | **95.02%** | **92.96%** |

**Anomalies prioritaires identified :**

🔴 **Anomaly 1 — Product 21003 (5G 1TB): 11 788 units vendues, Unit Price = 0,00, Gross Revenue = 0**
→ The product a a volume Actual non nul but no revenue generated. The costs are active (Unit Cost = 56,71).
→ Risk: perte comptable hidden — ~668 000 of costs without contrepartie of chiffre revenue.
→ **Action**: correct `Unit Price` in Revenue Assumptions for the product 21003.

🔴 **Anomaly 2 — Canada (org. 400): Gross Margin % to ~96% all channels**
→ The margin brute Canada: 96,09% (Retail), 95,02% (Internet), 92,96% (Distribution) vs 21–24% for the regions US.
→ Explanation likely: conversion currency CAD→USD absent or incorrect — the costs in CAD non convertis gonflent artificiellement the margin.
→ **Action**: check the rules of conversion into the cube Exchange Rates for the region 400.

🟠 **Anomaly 3 — Channel Distribution (30): Gross Margin % negative in toutes the regions US**
→ East: -1,14% · Central: -1,31% · West: -0,98% — sale to systematic loss.
→ Can be intentional (channel subsidized) or reflect of the assumptions of indirect costs too high.
→ **Action**: check Revenue Assumptions for the channel 30 and confirmer with the customer.

🟡 **Anomaly 4 — Gross Margin % consolidated (42,94%) inconsistent with the regions (~21%)**
→ Total Company affiche 42,94% alors that each region US is between 20% and 24%.
→ Cause: Canada (96%) weighs 29% of the volume total — direct consequence of Anomaly 2.

🟡 **Anomaly 5 — Actual Gross Revenue greater of +22,4% to the Budget**
→ Total Company: Actual 94 246 460 vs Budget 76 993 682 = +22,4%.
→ Threshold of vigilance: Actual > Budget × 1,2 → to qualifier with the customer before validation load.

**Note**: `perform_outlier_detection` returns HTTP 500 on this server for the cube Revenue. The anomalies above ont been identified by analysis MDX comparative (ratios inter-regions, values nulles, variances Actual vs Budget).

#### ⚡ With Bob
- **Time**: ~3 minutes
- **What is automated**: queries analysis, detection statistique, mise in context business
- **Quality**: report prioritized with explanation for each anomaly

#### 🐢 Without Bob
- **Time**: 4 to 8 hours (queries MDX manual + analysis Excel)
- **Required tools**: TM1 Architect, Excel, knowledge MDX
- **Risks**: anomalies not detected before the mise in production → data incorrects for the users

#### 💡 Discussion points

**Q: Are all detected anomalies actual errors, or are some legitimate?**
> The anomaly Canada (96% of margin) is probably linked to the absence of conversion currency into the cube Revenue — the costs are in CAD but the revenue in fictitious USD, which inflates the margin. It is an **anomaly of modeling**, not a data error. The channel Distribution to margin negative (-1%) is probably **intentional**: the model 24Retail reflects a situation where this channel is subsidized. Best practice is always to ask the customer before qualifying an anomaly as an "error".

**Q: How can this detection be automated for each monthly load?**
> Two approaches: (1) **Validation TI process**: create a process which reads the critical KPIs (Gross Margin %) and generates an anomaly report in a control cube, then call it through a Chore after each load, (2) **Bob in post-processing**: after each load, automatically trigger a prompt Bob *"Analyze Revenue data loaded today and highlight any anomaly versus the previous month"* — requires a trigger integration in the load workflow.

#### 🔍 Verification on the server — Exercise 6.1

**Queries MDX anomalies executed live :**

```mdx
-- Gross Margin % view by region (Version 1, Year Y1)
SELECT { [Channel].[Channel].[Channel Total] } ON COLUMNS,
{ [organization].[organization].[100], ..., [organization].[organization].[Total Company] } ON ROWS
FROM [Revenue]
WHERE ( ..., [Version].[Version].[Version 1], [Revenue].[Revenue].[Gross Margin %] )
```

**Results live confirmed :**

| Region | Gross Margin % (Version 1) |
|---|---|
| East (100) | 20.58% |
| Central (200) | 21.02% |
| West (300) | 21.37% |
| **Canada (400)** | **96.12%** |
| Total Company | 42.94% |

| Channel | Retail (10) | Internet (20) | Distribution (30) |
|---|---|---|---|
| East (100) | 21.37% | 21.57% | **(1.14%)** |
| Canada (400) | 96.09% | 95.02% | 92.96% |

**Anomalies confirmed live :**
- 🔴 **Product 21003**: Units Sold = 11 788, Gross Revenue = 0, Unit Price = 0.00 (costs active without revenue) — **anomaly non present in the result of reference initial**
- 🔴 Canada 96,12% → anomaly of conversion currency confirmed
- 🟠 Distribution (30): margin negative (East -1,14%, Central -1,31%, West -0,98%) → comportement expected of the model
- 🟡 Actual 94 246 460 vs Budget 76 993 682 = +22,4% — to qualifier
- **`perform_outlier_detection` returns HTTP 500** on this server for this cube — the analysis must be faite via MDX comparatif

---

### Exercise 6.2 — Generation of case of test

> 🟡 Level: Intermediate | ⏱ ~15 min

#### 🎯 Objective
Show that Bob generates a plan of test complete for a TI process — covering the case nominaux, limits and error.

#### 📋 Context
The process Copy_Revenue_Units is developed and must go through UAT. You must provide a plan of test documented to the team QA.

#### 💬 Prompt to run

```
Generate a complete test plan for the TI process "Copy_Revenue_Units" deployed
on 24Retail (parameters: pOrg, pChannel, pCurrentProduct, pYear, pVersion,
pUnitFcst, pSpreadMethod). The plan must cover:
1. Nominal cases (normal execution with valid data)
2. Boundary cases (pUnitFcst = 0, future year, non-existent channel)
3. Error cases (empty parameters, unknown organization)
4. Result validation criteria (how to verify that the spread is correct)
Format: table with columns Case / Input parameters / Expected result / Success criterion.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob generates the plan of test structured
3. Inspect the couverture of the case
4. Compare with the result of reference

#### ✅ Reference result

> ⚠️ **Deux versions of the process existent on 24Retail** — the prompt mentionne `pSpreadMethod` which is not present that in `Copy_Revenue_Units_v2` (7 params). `Copy_Revenue_Units` (v1) does not have that 6 parameters without `pSpreadMethod`.

**Plan of test — Process `Copy_Revenue_Units_v2` (24Retail)**

**Data of reference live (pOrg=101, pChannel=10, pProduct=21002, pYear=Y1) :**
- Budget (Version 1) = seasonal: Jan=279, Feb=279…Jun=270, Jul=205…Dec=158 — total 2 733 units
- Forecast actuel (Uniform) = 200/month × 12 = 2 400 units

| # | Category | Case | Parameters input | Expected result | Success criterion |
|---|---|---|---|---|---|
| 1 | Nominal | Spread Uniform | pOrg=101, pChannel=10, pProduct=21002, pYear=Y1, pVersion=Forecast, pUnitFcst=2400, pSpreadMethod=Uniform | 200 units/month | Jan=…=Dec=200 · Year=2400 |
| 2 | Nominal | Spread Seasonal | pUnitFcst=2400, pSpreadMethod=Seasonal | Spread selon key seasonal | Somme=2400 · Jan≠Dec · not error |
| 3 | Nominal | Grand volume | pUnitFcst=120000, pSpreadMethod=Uniform | 10 000/month | Jan=…=Dec=10000 · Somme=120000 |
| 4 | Nominal | Year future Y4 | pYear=Y4, pUnitFcst=1200, pSpreadMethod=Uniform | 100 units/month on Y4 | Cellules Y4=100 · Y1 unchanged |
| 5 | Nominal | Channel Internet (20) | pChannel=20, pUnitFcst=2400, pSpreadMethod=Uniform | 200 units/month on channel 20 | Channel 10 unchanged · Channel 20=200/month |
| 6 | Boundary | pUnitFcst = 0 | pUnitFcst=0, pSpreadMethod=Uniform | Toutes the cells = 0 | Jan=…=Dec=0 · process ends normalement |
| 7 | Boundary | Version with existing data (Actual) | pVersion=Actual, pUnitFcst=5000 | Overwrite silent of the Actuals | ⚠️ Check if the process protects the version Actual |
| 8 | Boundary | pSpreadMethod value unrecognized | pSpreadMethod=XYZ | Behavior undefined | ⚠️ Fallback Uniform by default or ProcessError? |
| 9 | Boundary | Channel Distribution (margin negative) | pChannel=30, pUnitFcst=2400 | 200 units/month on channel 30 | The margin negative of the channel 30 ne bloque not the write |
| 10 | Error | pOrg blank | pOrg="" | ProcessError or member blank | ⚠️ TM1 can create a member "" silently — check logs |
| 11 | Error | pOrg unknown | pOrg="ZZZ" | Write on member inexistant | ⚠️ CellPutN silent or ProcessError? |
| 12 | Error | pCurrentProduct blank | pCurrentProduct="" | ProcessError | Process stops with log error |
| 13 | Regression | v1 vs v2 Uniform | Same params in v1 (without pSpreadMethod) vs v2 (pSpreadMethod=Uniform) | Results identiques | Jan(v1)=Jan(v2) for each month |

**MDX of validation — Case nominal #1 (Uniform 2400) :**
```
SELECT
{ [Month].[Month].[Jan],[Month].[Month].[Feb],[Month].[Month].[Mar],
  [Month].[Month].[Apr],[Month].[Month].[May],[Month].[Month].[Jun],
  [Month].[Month].[Jul],[Month].[Month].[Aug],[Month].[Month].[Sep],
  [Month].[Month].[Oct],[Month].[Month].[Nov],[Month].[Month].[Dec],
  [Month].[Month].[Year] } ON COLUMNS,
{ [Revenue].[Revenue].[Units Sold] } ON ROWS
FROM [Revenue]
WHERE ( [organization].[organization].[101], [Channel].[Channel].[10],
        [product].[product].[21002], [Year].[Year].[Y1],
        [Version].[Version].[Forecast] )
→ Expected: Jan=Feb=...=Dec=200 · Year=2400 (sum criterion)
```

**Verification logs post-execution (case error) via MCP :**
```
execute_tm1_processes_asynchronously("24Retail", "Copy_Revenue_Units_v2", params)
get_tm1_server_process_execution_error_logs("24Retail", "Copy_Revenue_Units_v2")
```

#### ⚡ With Bob
- **Time**: ~3 minutes
- **What is automated**: identification case of test, structuring in table, criteria of validation
- **Quality**: plan of test deliverable QA, covering the non-obvious cases

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (manual drafting, often incomplete)
- **Risks**: edge cases forgottens, not of clear success criteria → subjective UAT

#### 💡 Discussion points

**Q: Is the test plan sufficient, or should performance tests be added?**
> For functional UAT, the plan is sufficient. Two dimensions are missing for production-ready UAT: (1) **Performance tests**: measure execution time with `pUnitFcst` on 1,000 products in parallel — TM1 is designed for concurrency, but a poorly written process can cause mutual blocking; (2) **Regression tests**: compare results before and after a process change to ensure a correction does not break another case. These two test types are easy to specify with Bob but require a dedicated test infrastructure.

**Q: Could Bob execute some of these tests directly?**
> Currently, Bob can: (1) trigger the execution process via `execute_tm1_processes_asynchronously`, (2) check the logs after execution via `get_tm1_server_process_execution_error_logs`, (3) read the results into the cube via `execute_mdx_and_get_view`. What is missing is a "conditional orchestration": execute the test, wait for the result, compare with the expected, and generate a pass/fail report. This pattern is feasible with several successive Bob exchanges, but not yet in a single interaction.

#### 🔍 Verification on the server — Exercise 6.2

**Process confirmed live via `get_tm1_process_details` :**

| Process | Params actual | pSpreadMethod? |
|---|---|---|
| `Copy_Revenue_Units` | pOrg, pChannel, pCurrentProduct, pYear, pUnitFcst, pVersion (6) | ❌ Absent |
| `Copy_Revenue_Units_v2` | pOrg, pChannel, pCurrentProduct, pYear, pVersion, pUnitFcst, **pSpreadMethod** (7) | ✅ Uniform/Seasonal |

**Data of reference live :**

| Context | Values live |
|---|---|
| Budget (V1) — 101/Retail/21002/Y1 | Jan=279, Feb=279, Mar=279, Apr=279, May=279, Jun=270, Jul=205, Aug=195, Sep=186, Oct=177, Nov=167, Dec=158 · Total=2 733 |
| Forecast actuel — 101/Retail/21002/Y1 | 200 × 12 = 2 400 · distribution Uniform confirmed |

**Correction applied**: the plan of test reference `Copy_Revenue_Units_v2` (not the v1) to cover `pSpreadMethod`. The values Jan=840 of the result of reference initial (based on pUnitFcst=12000 Uniform) do not have been tested live — the data Forecast live confirment the pattern Uniform to 200/month for pUnitFcst=2400.

---

## Domain 7 — Documentation

> **Relevant BUILD role**: Tout role BUILD
> **Situation**: Fin of project — livrables of documentation to produce before transfert to the customer

### Exercise 7.1 — Documentation architecture automatique

> 🟢 Level: Beginner | ⏱ ~10 min

#### 🎯 Objective
Show that Bob generates in a few minutes a complete technical architecture documentation — a deliverable which normally takes 2 days to write.

#### 📋 Context
The project 24Retail ends. You must livrer the documentation architecture to the customer before the closure of the project.

#### 💬 Prompt to run

```
Generate the complete technical documentation for the 24Retail Revenue cube, as if
you were writing an end-of-project deliverable for an enterprise customer. The documentation
must cover: functional description, complete dimensional model (dimensions +
members + types), known business rules, data flows (sources → TI processes → cube),
associated TI processes with their parameters, and attention points for the maintenance team.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob collecte toutes the metadata of the cube Revenue and of the process associated
3. Bob generates the documentation structured
4. Compare with the result of reference

#### ✅ Reference result

**Documentation technical — Cube Revenue, 24Retail** *(metadata collected live via MCP)*

---

**1. Description functional**
The cube `Revenue` is the central commercial planning cube of the model 24Retail. It stores the data forecast (Budget, Forecast) and actual (Actual) of the sales by product, channel of distribution and region geographic, for 4 years of planning (Y1=2025 to Y4=2028) and 20+ scenarios of version. It alimente the cube `Income Statement` and the module `Revenue Reporting`.

Ordres of grandeur actual (Gross Revenue Y1 — Total Company): Budget 76 993 682 · Actual 94 246 460 · Forecast 217 836 387.

**2. Dimensional model** *(types confirmed via `get_cube_dimensions_with_metadata`)*

| # | Dimension | Type PA | Cardinality | Hierarchy | Members keys |
|---|---|---|---|---|---|
| 1 | `organization` | GEO | 15 members | Total Company → 4 regions → 11 entities | 100=East, 200=Central, 300=West, 400=Canada |
| 2 | `Channel` | HIERARCHY | 4 members | Channel Total → Retail/Internet/Distribution | 10, 20, 30 |
| 3 | `product` | HIERARCHY | ~40 members | Product Total → Phones/PCs/Tablets → SKUs | 21002=5G 128Gb, 21003=5G 1TB, 32001=T 500 |
| 4 | `Month` | TIME | 30+ members | Year → Q1–Q4 → Jan–Dec + YTD + FcstMonth | Year, Jan…Dec, Q1–Q4, YTD |
| 5 | `Year` | *(non typed)* | 6 members | Y1–Y4 + variances | Y1=2025, Y2=2026, Y3=2027, Y4=2028 |
| 6 | `Version` | VERSIONS | 20 members | Budget(V1), Actual, Forecast, Variance, Predictive… | Version 1 (alias Budget), Actual, Forecast |
| 7 | `Revenue` | CALCULATION | **9 members** | Measures atomiques + calculated | Units Sold, Gross Revenue, Gross Margin % |

**3. Measures of the dimension Revenue**

| Member | Alias | Nature |
|---|---|---|
| `Units Sold` | Volume - Units | Input |
| `Unit Price` | Unit Net Sales Price | Input / lookup Revenue Assumptions |
| `Unit Cost` | Unit Direct Cost | Input / lookup Revenue Assumptions |
| `Gross Revenue` | — | Calculated: Units Sold × Unit Price |
| `Direct COGS` | — | Calculated |
| `Indirect COGS` | — | Calculated |
| `Cost of Sales` | Total Cost of Goods Sold | Calculated: Units Sold × Unit Cost |
| `Gross Margin` | — | Calculated: Gross Revenue − Cost of Sales |
| `Gross Margin %` | — | Calculated NOT: IF(GR≠0, GM÷GR×100, 0) + feeder |

**4. Rules of management business actives**
```
['Gross Revenue']  = N: ['Units Sold'] * ['Unit Price'];
['Cost of Sales']  = N: ['Units Sold'] * ['Unit Cost'];
['Gross Margin']   = N: ['Gross Revenue'] - ['Cost of Sales'];
['Gross Margin %'] = N: IF(['Gross Revenue'] <> 0,
                             ['Gross Margin'] \ ['Gross Revenue'] * 100, 0);
# Feeders
['Gross Revenue':] => ['Gross Margin %':];
# Canada currency conversion (region 400) via cube Exchange Rates
```

**5. Flux of data — Sources to cube**

```
SAP / POS files ────────────→ Load_PYActuals ──────────→ Revenue [Actual]
External CSV ───────────────→ Load_Revenue_FromCSV ──→ Revenue [all versions]
Manual PA input ────────────→ (PA interface) ─────────→ Revenue [Version 1 / Budget]
Copy_Revenue_Spread_Units ──→ (monthly spread) ──────→ Revenue [Forecast]
Copy_Revenue_Units[_v2] ────→ (Uniform/Seasonal) ───→ Revenue [Forecast / others]
Revenue Assumptions ─────────→ (TM1 rules) ──────────→ Revenue [Unit Price, Unit Cost]
Exchange Rates ──────────────→ (conversion rules) ──→ Revenue [Canada 400]
Revenue ─────────────────────→ UpdateISFromRevenue ──→ Income Statement
Revenue ─────────────────────→ (rules) ─────────────→ Revenue Reporting
```

**6. Process TI associated** *(confirmed live via `get_tm1_processes`)*

| Process | Parameters | Role | ⚠️ |
|---|---|---|---|
| `Load_PYActuals` | *(no)* | Load Prior Year Actuals | 🔴 0 params |
| `Load_Revenue_FromCSV` | pFilePath, pVersion, pDelimiter, pClearBeforeLoad | Import CSV | — |
| `Copy_Revenue_Spread_Units` | pOrg, pChannel, pCurrentProduct, pYear, pUnitFcst | Ventilation mensuelle (version by default — without pVersion) | — |
| `Copy_Revenue_Units` | + pVersion (6 params) | Spread with version parameterizable | — |
| `Copy_Revenue_Units_v2` | + pSpreadMethod: Uniform/Seasonal (7 params) | Version of reference with choix of method | — |
| `Copy_Revenue_Units_Unified` | pMode (Compare/Spread) + 6 params | Unifie Compare and Spread | — |
| `Add_Product` | pParent, pNewNumber, pNewName | Add a product in the dimension | — |
| `UpdateISFromRevenue` | *(no)* | Synchronisation to Income Statement | 🔴 0 params |
| `ResetRevenueData` | *(no)* | Remise to zero data Revenue | 🔴🔴 DESTRUCTEUR |

**7. Vues available (36)** *(confirmed via `list_cube_views`)*
- Analytiques: `Revenue Analysis`, `GM by Channel`, `Product Profitability by Channel/Quarter`, `Units Sold by Channel/Product`, `Compare`, `Revenue Budget vs Target`, `Predictive Review`…
- Technical internes (prefix `z`): `zUpdateIS`, `zMD_Revenue`, `zExportUnitsSoldSPSS`, `zCopyFrom`, `zCopyProduct`, `zZeroPredictiveVersion`

**8. Points attention for the team of maintenance**
- 🔴 **Product 21003 (5G 1TB): `Unit Price = 0,00`** — 11 788 units Actual without revenue → perte comptable hidden. Correct in Revenue Assumptions.
- 🔴 `ResetRevenueData` (0 params) efface toutes Revenue data — protect Remote Desktop access in production
- ⚠️ `Version 1` a the alias "Budget" — always use the ID `Version 1` in the TI processes (never the alias)
- ⚠️ Channel Distribution (30): Gross Margin % negative in toutes the regions US → comportement expected, not a error
- ⚠️ Canada (400): Gross Margin % ~96% → anomaly of conversion currency CAD→USD (see cube `Exchange Rates`)
- ⚠️ `Copy_Revenue_Spread_Units` does not have of parameter `pVersion` — check the version target in the code of the process
- ℹ️ Rules actives (`hasRules: true`) — files `.rux` non accessibles via MCP, backup manual mandatory before migration
- ℹ️ Drillthrough actif (`hasDrillthroughRules: true`) — rules `.drl` to migrer separately
- ℹ️ Vues internes `zMD_Revenue`, `zUpdateIS` used by TI processes — ne not supprimer

---

#### ⚡ With Bob
- **Time**: ~5 minutes
- **What is automated**: metadata collection, structuring, drafting
- **Quality**: documentation based on the real data, no risk of inconsistency between doc and model

#### 🐢 Without Bob
- **Time**: 1 to 2 days (manual inventory + drafting + formatting)
- **Risks**: documentation incomplete, errors compared with the real model, rapid obsolescence

#### 💡 Discussion points

**Q: What is the difference between a doc generated by Bob and a manually written documentation?**
> The doc generated by Bob is **based on the server metadata** — it cannot conflict with the existing model, except for objects Bob cannot read directly, such as rules and TI source code. Manual documentation risks being incomplete or obsolete. However, human documentation includes the **why** behind design choices, such as why product is in position 6 rather than 4, and the **history of the model**, such as why a dimension was added in 2019 after project X. The two are complementary: Bob for structure, humans for context.

**Q: How should this documentation be kept up to date as the model changes?**
> Recommended strategy: (1) regenerate Bob documentation **after each sprint** by rerunning the same prompt — 5 minutes per cube; (2) store the documentation in a **Git repository** with diffs between versions to track changes; (3) add a “Bob documents” step to the **definition of done** for each TI user story. The documentation therefore remains synchronized with the real model.

#### 🔍 Verification on the server — Exercise 7.1

**Metadata collected live via MCP :**

| Tool MCP | Data collected |
|---|---|
| `get_cube_dimensions_with_metadata` | Types dimensionnels actual: organization=GEO, Channel=HIERARCHY, Month=TIME, Version=VERSIONS, Revenue=CALCULATION, Year=*null* |
| `list_cube_views` | 36 vues confirmed including 7 vues internes `z*` |
| `lookup_potential_members` | 9 mesures Revenue (not 10) |
| `execute_mdx_and_get_view` | Gross Revenue Y1: Budget 76,9M · Actual 94,2M · Forecast 217,8M |
| `get_tm1_process_details` | `Copy_Revenue_Spread_Units` does not have of `pVersion` — version target probablement hardcoded |

**Corrections vs guide initial :** dimension Revenue = **9 members** (not 10) · `Channel` is of type `HIERARCHY` (not `—`) · `Year` a `dimensionType: null` (non typed) · 9 process documented (not 6) · 36 views added · anomaly product 21003 and risk `Copy_Revenue_Spread_Units` added.

---

### Exercise 7.2 — Specification technical of TI processes

> 🟡 Level: Intermediate | ⏱ ~15 min

#### 🎯 Objective
Generate a functional and technical specification of a TI process in the project deliverable format — document that the customers often require and that the BUILD teams neglect.

#### 📋 Context
The customer request a specification technical specification for all delivered TI processes, for allow their IT team to maintain the solution after the departure of the consultants.

#### 💬 Prompt to run

```
Generate the functional and technical specification for the 24Retail TI process
"Copy_Revenue_Spread_Units", in project-deliverable format for an enterprise customer.
The specification must include: functional context, processing description,
parameter table (name, type, description, default value, mandatory), data-flow diagram,
applied business rules, error handling, dependencies (cubes, dimensions), and execution prerequisites.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob retrieves the metadata of the process on 24Retail
3. Bob generates the specification in the format deliverable
4. Compare with the result of reference

#### ✅ Reference result

**Specification technical — Process `Copy_Revenue_Spread_Units`**
**Server**: 24Retail | **Cube target**: Revenue | **Level**: Deliverable project

---

**1. Context functional**
This process allows to a planner of enter an annual target of sales (in units) and of the automatically spread it on the 12 months of the exercise for a combination organization / channel / product / year data. It is typicalment called from a workflow IBM Planning Analytics lors of the phase of construction budget or of the forecast.

> ℹ️ No `prompt` provided on the parameters → process designed for be called from a workflow PA (the front-end manages the labels). Unlike to `Copy_Revenue_Units` which exposes explicit prompts.

**2. Processing description**
The process receives an annual total (`pUnitFcst`) and writes into the cube `Revenue` on the 12 monthly cells (Jan to Dec) for the combination `pOrg × pChannel × pCurrentProduct × pYear`, by applying a method of internal spread (seasonal key not exposed as a parameter).

**3. Parameters** *(source: MCP `get_tm1_process_details` — live collection)*

| Parameter | Type | Description | Value by default | Mandatory |
|---|---|---|---|---|
| `pOrg` | String | Code organization (ex: "100" = East Region) | `""` | ✅ Yes |
| `pChannel` | String | Code channel (10=Retail, 20=Internet, 30=Distribution) | `""` | ✅ Yes |
| `pCurrentProduct` | String | Code product TM1 (ex: "21002" = 5G 128Gb) | `""` | ✅ Yes |
| `pYear` | String | Year of planning (Y1=2025, Y2=2026, Y3=2027, Y4=2028) | `""` | ✅ Yes |
| `pUnitFcst` | Numeric | Total annuel units to ventiler on Jan–Dec | `0` | ✅ Yes |

**4. Flux of data**
```
Input: pOrg × pChannel × pCurrentProduct × pYear × pUnitFcst
    ↓  (monthly spread — internal key)
Output: Revenue[pOrg, pChannel, pCurrentProduct, Jan…Dec, pYear, Version?, Units Sold]
```

> ⚠️ The dimension `Version` is not a parameter presented — unlike `Copy_Revenue_Units` (6 params) and `Copy_Revenue_Units_v2` (7 params with `pSpreadMethod`). The version target is probably hard-coded in the process or inherited of the context call.

**5. Rules of management applied**
- The sum of the 12 monthly values written is equal to `pUnitFcst`
- The key of spread mensuelle is interne to the process (non configurable by parameter)

**6. Management of the errors**
- No validation of parameter documented in the version actuelle (prompts all empty)
- ⚠️ Recommandation: add `IF(pOrg @= ''); ProcessBreak; ENDIF;` before write
- ⚠️ Recommandation: add `IF(pUnitFcst = 0); ProcessBreak; ENDIF;` for avoid overwrite silent with of the zeros

**7. Dependencies**
- **Cube target**: `Revenue` (dimension `Revenue` → member `Units Sold`)
- **Dimensions used**: organization, Channel, product, Month, Year (Version absent of the parameters)
- **Process related** :
  - `Copy_Revenue_Compare_Units` — 3 params, read single (compare without write)
  - `Copy_Revenue_Units` — 6 params (ajoute `pVersion` presented)
  - `Copy_Revenue_Units_v2` — 7 params (ajoute `pVersion` + `pSpreadMethod` Uniform/Seasonal)

**8. Prerequisites execution**
- `pOrg`: member leaf of the dimension `organization` (ex: 100=East, 200=Central, 300=West, 400=Canada)
- `pChannel`: member leaf — values valid: **10** (Retail), **20** (Internet), **30** (Distribution)
- `pCurrentProduct`: member leaf of the dimension `product` (ex: 21002 = 5G 128Gb)
- `pYear`: member leaf — **Y1** (2025), **Y2** (2026), **Y3** (2027), **Y4** (2028)
- `pUnitFcst > 0` recommended (no internal validation — a value 0 would silently overwrite silently the data)

---

#### ⚡ With Bob
- **Time**: ~3 minutes
- **What is automated**: retrieval of actual parameters, structuring in the format spec, drafting
- **Quality**: deliverable directement remis to the customer

#### 🐢 Without Bob
- **Time**: 2 to 4 hours by process (and 24Retail in a **62**)
- **Risks**: specifications incomplete or wrong → problems of maintenance post-project

#### 💡 Discussion points

**Q: This specification is-it sufficient for has developer PA tiers maintienne the process?**
> To understand **what does** the process, yes. For the **modify** in complete safety, no — the source code is missing (Prolog/Data/Epilog). A specification complete at customer-deliverable level must include either the code source annotated, either a access to TM1 Architect so that the developer tiers puisse read the code. Recommandation: exporter the .pro from TM1 Architect and hasnnexer to the specification.

**Q: However Bob compenserait-it the absence of the code source in the specification?**
> Ask the user to paste the code in the chat. Bob can then regenerate an enriched specification including line-by-line commented code, edge cases detected in the code, and identified risks. This is the pattern: `export .pro → paste into Bob → Bob generates a complete specification → save as PDF`. Total time: ~10 minutes per process.

#### 🔍 Verification on the server — Exercise 7.2

**Parameters actual of the process `Copy_Revenue_Spread_Units` *(MCP `get_tm1_process_details` — live)* :**

| Parameter | Type | Value by default | Prompt |
|---|---|---|---|
| `pOrg` | String | `""` | *(blank)* |
| `pChannel` | String | `""` | *(blank)* |
| `pCurrentProduct` | String | `""` | *(blank)* |
| `pYear` | String | `""` | *(blank)* |
| `pUnitFcst` | Numeric | `0` | *(blank)* |

**Constats live :**
- ✅ 5 parameters confirmed — not of `pVersion` ni `pSpreadMethod` (unlike the variantes `v2`)
- ✅ All the prompts are empty → process designed for call from a workflow PA
- ✅ Years Y1–Y4 = 2025–2028 confirmed via `get_cube_sample_members`
- ✅ Members leaf members Channel confirmed: 10 (Retail), 20 (Internet), 30 (Distribution)

**Verification MDX — data `Units Sold` Forecast Y1 org=100 channel=10 product=21002 :**

```
Jan: 604 | Feb: 610 | Mar: 612 | Apr: 614 | May: 618 | Jun: 620 | Jul: 424 | Aug: 426 | ...
```
→ Confirmed: the cube Revenue accepte bien the combinations of members used in the prerequisite. The data existent for the version Forecast.

**Comparaison process `Copy_Revenue_*` on 24Retail :**

| Process | Params | pVersion | pSpreadMethod | Usage |
|---|---|---|---|---|
| `Copy_Revenue_Spread_Units` | 5 | ❌ | ❌ | Call workflow PA (version coded hard-coded) |
| `Copy_Revenue_Units` | 6 | ✅ | ❌ | Call standard with version explicite |
| `Copy_Revenue_Units_v2` | 7 | ✅ | ✅ Uniform/Seasonal | Call advanced with method of ventilation |
| `Copy_Revenue_Compare_Units` | 3 | ❌ | ❌ | Read single — comparaison without write |

---

## Domain 8 — Deployment

> **Relevant BUILD role**: Chef of project technical, Developer senior
> **Situation**: Production deployment — promote dev → UAT → production securely

### Exercise 8.1 — Checklist of deployment

> 🟡 Level: Intermediate | ⏱ ~10 min

#### 🎯 Objective
Show that Bob generates a checklist of deployment complete and contextualized for IBM Planning Analytics — a deliverable often absent of the projets BUILD.

#### 📋 Context
The recette is validated. The go-live is in 48h. You must prepare the plan of deployment of 24Retail in production.

#### 💬 Prompt to run

```
Generate a complete deployment checklist to promote the 24Retail model
from the development environment to IBM Planning Analytics production.
The checklist must cover: pre-deployment checks (model, data, security, performance),
ordered deployment steps (in the correct dependency order), post-deployment validations,
go/no-go criteria, and the rollback procedure if an anomaly is detected after cutover.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob generates the checklist structured based on the model 24Retail
3. Inspect the completeness of the steps
4. Compare with the result of reference

#### ✅ Reference result

**Checklist of deployment — 24Retail → Production PA** *(contextualized on the real data live)*

**PRE-DEPLOYMENT — J-2 (to validate before tout deployment)**

*Model (inventaire live: 34 cubes, 62 process)*
- [ ] The **62 TI processes** tested in recette without error (`Copy_Revenue_*`, `UpdateISFromRevenue`, `copy_version`, `Load_PYActuals` in priority)
- [ ] The **33 cubes with rules** actives tested in recette (Revenue, Income Statement in priority)
- [ ] `reset_demo` and `ResetRevenueData` locked / inaccessibles to the users in production
- [ ] The **5 drillthrough** (Revenue, IS, IS Reporting, Capital, Employee) fonctionnels in recette

*Data*
- [ ] Backup complete data of production before bascule
- [ ] `Load_PYActuals` executed and validated (actuals Prior Year loaded)
- [ ] Seasonal keys `Copy_Revenue_Spread_Units` validated with the assumptions business

*Security*
- [ ] The permissions Version (lock/write) configured by user group
- [ ] Access Admin only on `reset_demo` and `ResetRevenueData`
- [ ] 0 cell security active on 24Retail (confirmed live) — validate if expected in production
- [ ] The users of recette cannot access to the server of production

**DEPLOYMENT — Order mandatory**

1. ⬜ Stop the Chores scheduled on the environment target
2. ⬜ Migrer the dimensions (organization, Month, Year, Version, Channel, product, Account…)
3. ⬜ Migrer the **34 cubes** in the exact order of the dependencies (Exchange Rates → Revenue → Income Statement → IS Reporting → autres)
4. ⬜ Deploy the rules (.rux) on the **33 cubes** concerned
5. ⬜ Deploy the **62 TI processes** in the exact order: `}tp_*` (15) → `}Drill_*` (11) → process business
6. ⬜ Recreate the Chores (planning of the automatic loads)
7. ⬜ Configurer the security (groupes, permissions cubes, permissions versions)

**POST-DEPLOYMENT — J (the day of the go-live)**

*Tests of smoke*
- [ ] Execute `Copy_Revenue_Units` (pOrg=100, pChannel=10, pProduct=21002, pYear=Y1, pUnitFcst=1000, pVersion=Forecast) → check spread Jan-Dec
- [ ] Execute `UpdateISFromRevenue` → check consistency Revenue vs Income Statement
- [ ] Run a vue Revenue Forecast Y1 Total Company → validate the totaux consolidated
- [ ] Test the 5 drillthrough (Revenue, IS, IS Reporting, Capital, Employee)

*Criteria Go/no-go*
- ✅ 0 error in the logs of deployment
- ✅ Smoke tests completed
- ✅ 5 pilot users connected without anomaly
- ❌ No-Go if: data manquantes > 1%, process in error, drillthrough KO

**ROLLBACK**
If anomaly critical detected in the 4h post go-live :
1. Reconnecter the front-ends to the old server (< 30 min)
2. Notifier the users
3. Analyze the logs TM1 via MCP `get_tm1_server_process_execution_error_logs`
4. Reprogrammer deploy itment with the corrections

#### ⚡ With Bob
- **Time**: ~3 minutes
- **What is automated**: structuring of the checklist, order of the dependencies, criteria of validation
- **Quality**: checklist complete and contextualized for PA

#### 🐢 Without Bob
- **Time**: 2 to 4 hours (researching best practices + adaptation to the project)
- **Risks**: forgotten steps, wrong order → errors in production, no rollback scheduled

#### 💡 Discussion points

**Q: Does the checklist cover IBM PA Cloud vs. On-Premise specifics?**
> Partly. Common items (cubes, processes, security) are covered. Cloud-specific items (PAaaS) are missing: (1) storage bucket configuration for load files; (2) VPN/Private Endpoint connection management for accessing on-premise data sources from the Cloud; (3) network bandwidth validation for the first users. To get a Cloud-ready checklist, specify “PAaaS” in the prompt rather than only “IBM Planning Analytics”.

**Q: Which steps require human intervention that Bob cannot automate?**
> (1) The **validation visuelle** of the rapports PA in the interface (Bob cannot prendre of screenshot), (2) the **validation user** — do signr the PV of recette by a representing business, (3) the **communication** — envoyer the mail of go-live to the users, (4) the **decision go/no-go** — same with of the criteria objectifs, the decision finale of basculer is always humaine. Bob can prepare all the items for this decision but ne the prend not.

#### 🔍 Verification on the server — Exercise 8.1

**Inventaire live via MCP `get_tm1_cubes` + `get_tm1_processes` :**

| Artefact | Compte live | Notes |
|---|---|---|
| Cubes totaux | **34** | including `Forecast_Example` without rules |
| Cubes with rules (.rux) | **33** | migration manual TM1 Architect |
| Cubes with drillthrough | **5** | Revenue, IS, IS Reporting, Capital, Employee |
| `Revenue Reporting` | ⚠️ **false** | `hasDrillthroughRules: false` — to check |
| Process TI | **62** | including `reset_demo`, `ResetRevenueData` to protect |
| Process `}tp_*` | **15** | workflow PA planning |
| Process `}Drill_*` | **11** | drillthrough |
| Cell security | **0** | no cube protected — to confirmer in production |

**Tests smoke validated (parameters actual of the process) :**
1. `Copy_Revenue_Units` (pOrg=100, pChannel=10, pCurrentProduct=21002, pYear=Y1, pUnitFcst=1000, pVersion=Forecast) → MDX Jan-Dec confirmed: `Jan:604, Feb:610, Mar:612…`
2. `UpdateISFromRevenue` → check consistency Revenue vs Income Statement
3. Run a vue Revenue Forecast Y1 Total Company → validate the totaux consolidated (Gross Revenue = 217,8M Forecast)

---

### Exercise 8.2 — Monitoring post-deployment in time real

> 🟡 Level: Intermediate | ⏱ ~10 min

#### 🎯 Objective
Show that Bob can monitorer a server TM1 in time real and identifier the anomalies post-deployment — directement from the interface Bob, without access to TM1 Architect.

#### 📋 Context
The deployment is complete. The first users connect. You must monitor the server during the first hours for detect tout problem.

#### 💬 Prompt to run

```
Monitor the 24Retail server in real time: list active TI processes and their
status, check whether there are blocked threads or processes in error, review
the logs of the last 5 executed processes to detect errors, and give me
a global server health status with immediate attention points.
```

#### 🔢 Steps
1. Copy the prompt in Bob
2. Bob queries the MCPs `get_tm1_server_process_threads` and `get_tm1_server_process_execution_error_logs`
3. Bob summarizes the server state with the attention points
4. Compare with the result of reference

#### ✅ Reference result

**Monitoring in time real — Server 24Retail** *(executed live)*

**Report server state :**

```
📊 HEALTH STATUS — Server 24Retail
Queried via MCP get_tm1_server_process_threads
────────────────────────────────────────────────

Active threads: 1 identified user thread
(variable number depending on server activity at call time)

Thread currently executing:
  ID: 17652 | User: pm (Harmony LDAP) | Process: GET /api/v1/Threads | State: Run

Idle threads:
  No idle thread in this session
```

**Analysis Bob :**

✅ **No TI processes currently running**: the server is in nominal state, no processing load detected.

✅ **No thread blocked**: the alone thread actif is the query MCP it-same — comportement normal.

✅ **A single active user** (`pm`): minimal load on the server — favorable for the first users post-deployment.

**Logs error — 4 process critical consulted :**

| Process | Result | Interpretation |
|---|---|---|
| `Copy_Revenue_Units` | Unable to locate | Dernier run without ProcessError ✅ |
| `UpdateISFromRevenue` | Unable to locate | Dernier run without ProcessError ✅ |
| `copy_version` | Unable to locate | Dernier run without ProcessError ✅ |
| `reset_demo` | Unable to locate | No access recent detected ✅ |

> ℹ️ "Unable to locate" = comportement **normal** on 24Retail. TM1 ne creates a file log that lors of a `ProcessError` or of a plantage. Absence of log = execution without error.

**Points attention :**
- ℹ️ Only 1 user actif (`pm`): if deploy itment is expected to host of the users, check that the communication of go-live a been properly distributed.
- ℹ️ No log error recent on the 4 process critical consulted — dernier run without error.

**State of health global: 🟢 NOMINAL**

*"The server 24Retail is in perfect operating state. No anomaly detected. The monitoring can be reduced to a verification every 30 minutes during the first day of production."*

#### ⚡ With Bob
- **Time**: ~2 minutes
- **What is automated**: collecte multi-sources (threads + logs + statuts), summary
- **Quality**: state of health complete with priorisation alertes

#### 🐢 Without Bob
- **Time**: 15 to 30 minutes (navigation in TM1 Architect + Server Explorer + logs files)
- **Required tools**: TM1 Architect, access to the server, access to the files of log on the system
- **Risks**: problem not detected to time → impact on the users in production

#### 💡 Discussion points

**Q: Can Bob be configured to monitor automatically at regular intervals?**
> Not natively — Bob responds to point-in-time queries. For continuous monitoring, two approaches are possible: (1) **TM1 side**: create a Chore that runs every hour and executes a `Monitor_Health` process writing server state to a log cube (active processes, recent errors), which Bob can then read through MCP on request; (2) **Integration side**: use an external monitoring tool such as Nagios, Datadog, or IBM Instana with email/Slack alerts, then configure Bob to receive and analyze these alerts through an integration webhook.

**Q: Which alerts would be useful to automate for an on-call support team?**
> By order of priority: (1) **Thread blocked for more than 5 minutes** (risk of blocking all users), (2) **Process in `ProcessError`** (data load failed silently), (3) **Cube in unexpected read-only mode** (lock not released), (4) **Server memory utilization > 80%**, (5) **Scheduled Chore not executed** within the expected window (silent scheduling failure). These alerts can be coded into a monitoring TI process that Bob can generate on request.

#### 🔍 Verification on the server — Exercise 8.2

**MCP `get_tm1_server_process_threads` — session live :**

```
Actual result :
  ID: 17652 | pm | Run (GET /api/v1/Threads)
  (no idle thread in this session)
```

> ℹ️ The number of threads varies by session (1, 2, or 4) depending on server activity. This is normal variability for an idle TM1 server.

**MCP `get_tm1_server_process_execution_error_logs` — 4 process critical :**

| Process | Result MCP |
|---|---|
| `Copy_Revenue_Units` | Unable to locate ✅ |
| `UpdateISFromRevenue` | Unable to locate ✅ |
| `copy_version` | Unable to locate ✅ |
| `reset_demo` | Unable to locate ✅ |

**Constats live confirmed :**
- ✅ 1 thread actif (Run) — the server is in parfait state of idle state
- ✅ 0 TI processes currently running — no charge
- ✅ User `pm` (Harmony LDAP) — single user connected
- ✅ Thread Run = query MCP it-same (`GET /api/v1/Threads`) — comportement expected
- ✅ "Unable to locate" confirmed normal: TM1 ne creates a file log that lors of a `ProcessError`

---

## Conclusion

### Summary: Bob × IBM Planning Analytics BUILD

This guide a covered 20 exercises across 8 BUILD domains BUILD. Here is the honest matrix of the gains — and of the limits.

### Matrice of value by domain

| Domain | Gain of time | Risk reduced | Boundary principale | Workaround pratique |
|---|---|---|---|---|
| **1. Architecture** | 2–4h → ~5 min | Doc manual manquante | Ne can not read the rules (.rux) | Ouvrir the .rux in TM1 Architect, copy the contenu in the chat Bob: *"Voici the rules of the cube Revenue, analysis-the"* |
| **2. TurboIntegrator** | 2–6h → ~5–10 min | Syntax TI incorrect, omission of validation | Deploy the code but cannot execute it and check the result | Execute the process from PA, copy the result in Bob: *"I have executed the process, voici the output — analysis the errors"* |
| **3. TM1 Rules** | 2–4h → ~3 min | Wrong feeders (zeros ghost) | Generates the rules but cannot deploy it (limitation MCP on .rux) | In TM1 Architect: File > Export Rules, copy the .rux in a message Bob for enrichment, then reimport |
| **4. Integration** | 4–8h → ~5 min | Mapping incomplet, rejects silent | Ne can not read the data sources in direct (SAP, DB) | ① Ajouter a MCP ODBC/SQL (*[mcp-sql](https://github.com/benborla29/mcp-server-mysql)* or ODBC bridge) for access live to the data sources ② Or exporter a extrait CSV and the copy in the chat |
| **5. Migration** | 2–3j → ~10 min | Order of migration incorrect | Ne can not read the code complete of the process/rules | Export manual of the .pro/.rux from TM1 Architect, paste in Bob for analysis: *"Voici the code of 5 process critical — identifie the dependencies"* |
| **6. Tests** | 4–8h → ~3 min | Anomalies not detected | Cubes non pre-analyzed limitent the detection auto | Execute the vue in PA, do a screenshot or copy the data in Bob: *"Voici the values post-load — detects the anomalies"* |
| **7. Documentation** | 1–2j → ~5 min | Doc obsolete / inexistante | Ne can not documenter this that it cannot read (rules, code) | Same Workaround that domain 1: copy the code source in the chat so that Bob generates the doc complete |
| **8. Deployment** | 2–4h → ~3 min | Missing steps, no rollback | Point-in-time monitoring only (no continuous monitoring) | Create a Chore TM1 which executes a process of monitoring every hour + sends a summary in a cube of log that Bob can read via MCP |

### What Bob does very bien
- ✅ **Collecte of metadata**: inventaire complete of a server in < 2 minutes
- ✅ **Generation of code TI**: process complets with validation and error handling
- ✅ **Deployment on the server**: creation + update + validation syntax via MCP
- ✅ **Generation of TM1 rules**: syntax correct with the vrais names of members
- ✅ **Documentation**: structured, based on the real data, immediately deliverable
- ✅ **Analysis comparative**: detection anomalies in the data, identification of redondances

### What Bob does not (limits honest)
- ❌ **Read the code source of the rules** (.rux): the MCP ne allows not access to the files of rules
- ❌ **Deploy the TM1 rules**: generation only, deployment manual required in TM1 Architect
- ❌ **Test the execution process**: deployment confirmed but results to check by the user
- ❌ **Access to the systems sources**: Bob cannot read the data SAP, Oracle, files network
- ❌ **Monitoring continu**: Bob responds to point-in-time queries, it does not monitor in continu
- ❌ **Validate the logic business**: Bob generates a code syntactically correct, not necessarily business-correct

### Verdict for a team BUILD

> **Bob multiplie the vitesse of a consultant PA senior by a factor 5 to 20 on the tasks of generation, analysis and of documentation. It ne remplace not expertise business ni the knowledge of the environment customer — it hasmplifie.**

The investment the highest-return for a team BUILD: use Bob **systematically** for the repetitive tasks (documentation, analysis of model, generation of code TI standard) and focus on the **human value-added work** (logic business, management of the changement, validation functional).

---

## Appendix A — PA/TM1 Glossary

| Terme | Definition |
|---|---|
| **Cube** | Multidimensional structure that stores the data in TM1. Each cell is identified by the intersection of a member of each dimension. |
| **Dimension** | Axe analysis of a cube (ex: product, month, version). Contains of the members organized in a hierarchy. |
| **Member** | Item of a dimension. Can be leaf (data input) or consolidated (calculated by aggregation). |
| **TurboIntegrator (TI)** | Langage of script TM1 for the traitement of data: load, transformation, write into the cubes. |
| **Rule TM1** | Formule of calcul applied to a cube (file .rux). Calcule of the values derived to the fly (KPIs, conversions, variances). |
| **Feeder** | Instruction in a rule TM1 which indique to TM1 quelles cells source "alimentent" the cells calculated. Without feeder correct, the cells calculated may display 0 (zeros ghost). |
| **Sparse** | Characteristic of a cube where the majority of the intersections are empty. TM1 optimizes storage in recording only that the cells non nulles. |
| **Sandbox** | Espace of travail personnel in PA where a user can modify data without impacter the autres users. Used for the simulations. |
| **Version** | Dimension special (type VERSIONS) which control the scenarios of data: Budget, Forecast, Actual, Predictive… |
| **CellPutN / CellGetN** | Fonctions TI for write (CellPutN) and read (CellGetN) of the values numeric in a cube. |
| **ItemSkip / ItemReject** | Instructions TI: ItemSkip ignore the line without error, ItemReject rejette the line and records it in the logs. |
| **ProcessBreak / ProcessError** | ProcessBreak stops the process proprement, ProcessError stops the process with a error. |
| **Drillthrough** | Feature that allows navigation from a cell of a cube to the details of a autre source of data. |
| **Chore** | Scheduled task in TM1 which executes one or more TI processes to regular intervals. |
| **MDX** | Multidimensional Expressions — langage of query for interroger the data of a cube OLAP (TM1, SSAS…). |
| **DIMIX / DIMNM / DIMSIZ** | Fonctions TM1: DIMIX returns the position of a member, DIMNM the name by position, DIMSIZ the taille of the dimension. |
| **DB()** | Fonction TM1 for read a value in a autre cube from a rule. Used for the conversions inter-cubes. |
| **PAaaS** | IBM Planning Analytics as a Service — IBM PA cloud version, hosted on IBM Cloud. |

---

## Appendix B — Available MCP tools

The exercises of this guide utilisent deux servers MCP for interagir with IBM Planning Analytics :

### MCP `pa-tz-server` — Management TI processes

| Tool | Role |
|---|---|
| `get_available_tm1_servers` | Lister the servers TM1 available |
| `get_tm1_processes` | Lister all the TI processes of a server (with parameters) |
| `get_tm1_process_details` | Detail complete of a TI process |
| `create_tm1_process` | Create a TI process blank |
| `update_tm1_process` | Update the code of a process (Prolog/Data/Epilog + parameters) with validation syntax |
| `delete_tm1_process` | Supprimer a TI process |
| `execute_tm1_processes_asynchronously` | Run the execution of a process |
| `get_tm1_server_process_threads` | Lister the threads active of the server |
| `get_tm1_server_process_status` | State of a process in cours |
| `cancel_tm1_process_execution` | Annuler a process in cours |
| `get_tm1_server_process_execution_error_logs` | Consulter the logs execution of a process |

### MCP `pa-tz-cube` — Exploration data and metadata

| Tool | Role |
|---|---|
| `get_tm1_cubes` | Lister all the cubes of a server |
| `get_cube_dimensions` | Lister the dimensions of a cube |
| `get_cube_dimensions_with_metadata` | Dimensions with metadata (type GEO, TIME, VERSIONS…) |
| `get_cube_sample_members` | Sample of members of each dimension |
| `list_cube_views` | List saved views of a cube |
| `execute_mdx_and_get_view` | Execute a query MDX and retrieve the data |
| `get_data_from_data_explorer` | Interroger a cube in langage naturel (cubes pre-analyzed only) |
| `get_MDX_for_recommended_view` | Get the MDX correspondant to a question in langage naturel |
| `lookup_potential_members` | Rechercher of the members by terme of recherche |
| `perform_outlier_detection` | Detect the outliers in a vue of data |
| `perform_impact_analysis` | Analyze the drivers impact in a vue |
| `save_mdx_view` | Back up a vue MDX on the server |
| `create_tm1_cube` | Create a new cube |
| `delete_tm1_cube` | Supprimer a cube |

---

## Appendix C — Blank exercise template

Copiez this template for create a nouvel exercise in this guide.

```markdown
### Exercise X.Y — [Title]

> 🟢/🟡/🔴 Level: Beginner/Intermediate/Advanced | ⏱ ~XX min

#### 🎯 Objective
[What the participant demonstrates / learns]

#### 📋 Context
[The BUILD situation in which this exercise takes place]

#### 💬 Prompt to run

```
[Texte exact to copy in Bob — must be autonome and precise]
```

#### 🔢 Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]
4. Compare with the reference result

#### ✅ Reference result

[Actual output produced by Bob on 24Retail]

#### ⚡ With Bob
- **Time**: ~X minutes
- **What is automated**: [list of automated tasks]
- **Quality**: [description of the quality level]

#### 🐢 Without Bob
- **Time**: X to Y hours
- **Required tools**: [list of tools]
- **Risks**: [risks without Bob]

#### 💡 Discussion points
- [Question 1 to facilitate the post-exercise discussion]
- [Question 2]
```

---

## Appendix D — 24Retail server structure

Reference complete of the model used for all the exercises of this guide.

### Cubes (34 in total)

**Cubes transactionnels main :**

| Cube | Dimensions | Rules | Drillthrough |
|---|---|---|---|
| **Revenue** | organization × Channel × product × Month × Year × Version × Revenue | ✅ | ✅ |
| **Income Statement** | Currency Calc × organization × Year × Month × Account × Version | ✅ | ✅ |
| **Supply Chain** | organization × Channel × product × Month × Year × Supply Chain Measures × Version | ✅ | — |
| **Exchange Rates** | Currency × Year × Month × Exchange Rate × Exchange Rate Type × Version | ✅ | — |
| **Capital** | organization × CapitalList × Asset Types × Year × Version × Capital | ✅ | ✅ |
| **Employee** | organization × EmployeeList × Year × Version × Employee | ✅ | ✅ |
| **Compensation** | organization × ... × Version | ✅ | — |
| **Depreciation** | organization × ... × Version | ✅ | — |

**Cubes of configuration :**
`Spread Methods`, `Version Copy Control`, `Version Subset Control`, `Calendar`, `Task Workflow`, `FcstMethod`, `Revenue Assumptions`, `Allocation Source Definition`, `Rate BOM`

**Cubes of reporting :**
`Revenue Reporting`, `Income Statement Reporting`, `Compensation Reporting`, `Workflow Reporting`

### Dimensions keys shared

| Dimension | Type | Members main |
|---|---|---|
| `organization` | GEO | Total Company → 100(East)/200(Central)/300(West)/400(Canada) → 11 sub-entities |
| `Month` | TIME | Year, Q1-Q4, Jan-Dec, YTD, FcstMonth, Jan YTD… |
| `Year` | — | Y1(2025), Y2(2026), Y3(2027), Y4(2028), Y2-Y1$, Y2-Y1% |
| `Version` | VERSIONS | Version 1(Budget), Actual, Forecast, Predictive, Variance, Variance%, Ccy Exchange Variance… |
| `Channel` | — | Channel Total, 10(Retail), 20(Internet), 30(Distribution) |
| `product` | — | Product Total → Phones(21xxx)/PCs(3xxxx)/Tablets(4xxxx) → ~40 SKUs |
| `Currency` | — | USD, CDN, GBP |

### Process TI (60+ process)

**Process business keys :**

| Process | Parameters | Role |
|---|---|---|
| `Copy_Revenue_Spread_Units` | pOrg, pChannel, pCurrentProduct, pYear, pUnitFcst | Ventilation mensuelle |
| `Copy_Revenue_Compare_Units` | pCurrentProduct, pUnitFcst, pChannel | Comparaison forecasts |
| `Copy_Revenue_Units` | pOrg, pChannel, pCurrentProduct, pYear, pUnitFcst, pVersion | Spread seasonal (created in this guide) |
| `Add_Product` | pParent, pNewNumber, pNewName | Ajout product |
| `copy_version` | pFromVersion, pToVersion, pCubeName | Version copy |
| `UpdateISFromRevenue` | — | Sync Revenue → Income Statement |
| `Load_PYActuals` | — | Load actuals NOT-1 |
| `ResetRevenueData` | — | Remise to zero Revenue |
| `reset_demo` | pZeroTarget | Complete demo reset |
| `Export UnitsSold to SPSS` | — | Export IA/predictive |

**Process workflow (`}tp_*`) :** ~15 process IBM PA Planning Workflow (deployment, security, management of the versions…)

**Process drillthrough (`}Drill_*`) :** ~10 process of navigation between cubes (Revenue → IS, Capital → IS, Employee → IS…)

### Data of reference (Budget Y1 = 2025)

| Metric | Value |
|---|---|
| Units Sold — Total Company | 204,474 |
| Gross Revenue — Total Company | 76,993,682 |
| Gross Margin % — Total Company | 42.94% |
| Gross Margin % — USA (regions 100-300) | ~21% |
| Gross Margin % — Canada (400) | ~96% ⚠️ |
| Gross Margin % — Channel Distribution | ~(1%) ⚠️ |

---

## Appendix E — Sample CSV files

These files are provide in the folder `sample-data/` of the project. Ils allow of test the TI processes generated in the exercises 2.1 and 4.1 without avoir need of a system source real.

### [`revenue_csv_sample.csv`](sample-data/revenue_csv_sample.csv)

**Usage**: Exercise 2.1 — ETL load from a CSV file

**Content**: 55 lines of data Revenue (sales forecast)

| Column | Type | Description | Sample |
|---|---|---|---|
| `Organization` | String | Code organization TM1 | `100` (East Region) |
| `Channel` | String | Code channel of sale | `10` (Retail) |
| `Product` | String | Code product TM1 | `21002` (5G 128Gb) |
| `Month` | String | Month in the format TM1 | `Jan`, `Feb`… |
| `Year` | String | Year in the format TM1 | `Y1` (2025) |
| `Version` | String | Version TM1 | `Forecast` |
| `UnitsSold` | Numeric | Units vendues | `120` |
| `UnitPrice` | Numeric | Prix unitaire (USD) | `649.99` |
| `UnitCost` | Numeric | Cost unitaire (USD) | `280.00` |

**Couverture**: 5 organisations (100, 200, 300, 400 + sous-entity), 3 products (21002/5G 128Gb, 21001/5G 256Gb, 32001/T 500, 41010/10" 16Gb), 12 months, Version Forecast, Year Y1.

**Expected result after load**: 55 lines loaded, 0 rejected.

---

### [`sap_export_sample.csv`](sample-data/sap_export_sample.csv)

**Usage**: Exercises 4.1 and 4.2 — Mapping SAP → cube PA and management of the errors ETL

**Content**: 29 lines export SAP simulated, including **3 lines invalid intentionalles**

| Column | Type | Description | Sample |
|---|---|---|---|
| `CompanyCode` | String | SAP company code | `1000` (→ East Region `100`) |
| `MaterialNumber` | String | SAP material number | `MAT-21002` (→ product `21002`) |
| `FiscalPeriod` | String | Period fiscale SAP (001-012) | `001` (→ `Jan`) |
| `FiscalYear` | String | Year fiscale SAP | `2025` (→ `Y1`) |
| `PostingAmount` | Numeric | Montant of the write (USD) | `77998.80` |
| `Quantity` | Numeric | Quantity | `120` |
| `CostCenter` | String | Centre of cost SAP (non mapped) | `COST-SALES-E` |

**Table of correspondance CompanyCode → Organization TM1 :**

| CompanyCode SAP | Organization TM1 | Label |
|---|---|---|
| `1000` | `100` | East Region |
| `2000` | `200` | Central Region |
| `3000` | `300` | West Region |
| `4000` | `400` | Canada |

**Table of correspondance MaterialNumber → product TM1 :**

| MaterialNumber SAP | product TM1 | Label |
|---|---|---|
| `MAT-21002` | `21002` | 5G 128Gb |
| `MAT-21001` | `21001` | 5G 256Gb |
| `MAT-32001` | `32001` | T 500 |
| `MAT-41010` | `41010` | 10" 16Gb |

**Lines invalid intentionalles (for test the error handling) :**

| Line | Problem | Behavior expected |
|---|---|---|
| 27 | `CompanyCode = XXXX` (unknown) | `ItemReject` — Organization unknown |
| 28 | `MaterialNumber = MAT-INVALID` (unknown) | `ItemReject` — Product unknown |
| 29 | `MaterialNumber` blank | `ItemReject` — Field mandatory blank |

**Expected result after load (exercise 4.2 with threshold 5%)** :
- 26 lines loaded ✅
- 3 lines rejected (10.3% > 5%) → **`ProcessBreak` triggered** ⚠️
- Log generated with detail of the 3 rejects
