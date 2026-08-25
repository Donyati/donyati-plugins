---
name: donyati-help
description: Show every Donyati Expert Agents command available in this plugin, grouped by team
---

# /donyati-help — Plugin Command Reference

This plugin connects Claude to Donyati's Expert Agents platform (`expert-agents.donyati.com`) so you can ask experts, work with client knowledge, and review documents — from Claude Code **or** the Claude Desktop connector.

**New here?** Run `/donyati-start` for a guided walkthrough that routes you to the right command.

## Talk to experts (everyone)

| Command | What it does |
|---|---|
| `/donyati-ask <question>` | Ask any platform expert (auto-routes by keywords) |
| `/donyati-compare <topic>` | Multi-agent platform comparison with neutral synthesis |
| `/donyati-knowledge <query>` | Search the verified platform knowledge base |
| `/donyati-platforms` | List all platforms we cover |
| `/donyati-agents` | Full agent roster — platforms, industries, Donyati specialty agents |

> **Tip:** add `— industry: insurance` (or any slug from `/donyati-agents`) to `/donyati-ask` for an industry-framed answer — 16 verticals supported.

## Client & project knowledge (Sales / Presales / CS)

| Command | What it does |
|---|---|
| `/donyati-clients [filter]` | List client organizations |
| `/donyati-projects <client>` | List a client's projects/engagements |
| `/donyati-new-client <name>` | Create a new client organization |
| `/donyati-new-project <client> <name>` | Create a new project under a client |
| `/donyati-client-search <client> <query>` | Search a client/project's captured knowledge |
| `/donyati-add-knowledge <client> <notes>` | Record new facts into a client/project (confirmed immediately) |
| `/donyati-upload <client>` | Add a client document (RFP, transcript, notes) to a client/project |
| `/donyati-briefing <client> [project]` | Pull a client briefing before a meeting |

## Accelerator Library (everyone)

| Command | What it does |
|---|---|
| `/donyati-accelerators <query>` | Search delivery accelerators — reusable scripts, templates, workbooks, toolkits (filter by platform/category) |

## Deliverables (Sales / Presales)

| Command | What it does |
|---|---|
| `/donyati-deliverables [client]` | List + generate deliverables for a client — 8 types: RFP response (PowerPoint), executive summary, client briefing, assessment summary, custom summary (web decks), requirements, data source inventory, response repository (Word) |
| `/donyati-rfp-response <client>` | Generate an RFP / proposal response deck (PowerPoint) |

## Documents

| Command | What it does |
|---|---|
| `/donyati-summarize <content>` | Domain-aware document summarization |
| `/donyati-review <content>` | Critical expert review — findings, gaps, risks, recommendations |
| `/donyati-sow-review <content>` | Havagi CIO-style SOW/RFP audit before it goes to the client |

## Admin

| Command | What it does |
|---|---|
| `/donyati-posture` | Cloud compliance posture — admin keys only |

## Setup & help

| Command | What it does |
|---|---|
| `/donyati-setup` | First-time setup (Claude Code) — verify your API key works |
| `/donyati-help` | This screen |

## Two ways to use it

- **Claude Code (CLI):** install the plugin; you get these slash commands plus the MCP tools. Run `/donyati-setup` once with your `DONYATI_API_KEY`.
- **Claude Desktop:** add the custom connector (`https://expert-agents.donyati.com/api/mcp`) and sign in. The same commands appear as slash commands from the connector — no API key needed.

## Need help?

- Web app: https://expert-agents.donyati.com
- Admin / request a key (Claude Code): https://expert-agents.donyati.com/admin/api-keys
- Usage stats (admins): https://expert-agents.donyati.com/admin/api-usage
