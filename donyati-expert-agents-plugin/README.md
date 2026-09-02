# Donyati Expert Agents — Claude Plugin

**v2.4.0** — Access Donyati's 26+ AI expert agents and client knowledge from **Claude Code**, **Claude Desktop**, or **claude.ai web**.

Built for Sales, Marketing, Presales, and Customer Success teammates who want to consult expert agents, compare platforms, summarize documents, and search Donyati's verified knowledge base directly from Claude — terminal, desktop app, or browser.

> **Three surfaces, one backend.** Claude Code gets the full plugin with `/donyati-*` slash commands (uses a `dea_*` API key). Claude Desktop and claude.ai web use the same MCP server added as a **Custom Connector** — no API key needed; you sign in with your Donyati Microsoft 365 account. See `USAGE_GUIDE.md` §3 (Code) and §3B (Desktop / web) for setup.

---

## Setup — Claude Code (3 commands)

> Using Claude Desktop or claude.ai web? Skip to the **Claude Desktop / web** section below.

### 1. Get an API key from Matt

Keys are admin-issued during early access. Ping Matt (`mjanecek@donyati.com`) and he'll send your `dea_*` key via Slack or Teams.

### 2. Save the key as an environment variable

Open a terminal, paste this (with your real key), then hit enter:

```bash
echo 'export DONYATI_API_KEY="dea_paste_your_key_here"' >> ~/.zshrc && source ~/.zshrc
```

(Linux/WSL: use `~/.bashrc` instead of `~/.zshrc`.)

### 3. Install the plugin inside Claude Code

Open Claude Code, then run these two slash commands:

```
/plugin marketplace add Donyati/donyati-plugins
/plugin install donyati-expert-agents@donyati-plugins
```

Claude Code clones the repo and installs the plugin automatically — no git knowledge required.

### Verify

```
/donyati-setup
```

You should see your key name, owner email, and rate limit. If you do, you're done.

---

## Commands (v2.4.0)

**Talk to experts**

| Command | What it does |
|---|---|
| `/donyati-ask` | Ask any platform expert (auto-routes by keywords) |
| `/donyati-compare` | Multi-agent platform comparison with neutral synthesis |
| `/donyati-knowledge` | Search the verified platform knowledge base |
| `/donyati-platforms` | List all platforms we cover with article counts |
| `/donyati-agents` | Full agent roster — platforms, industries, Donyati specialty agents |

> **Tip:** add `— industry: insurance` (or any slug from `/donyati-agents`) to `/donyati-ask` for an industry-framed answer — 16 verticals supported.

**Client & project knowledge** (Sales / Presales / CS)

| Command | What it does |
|---|---|
| `/donyati-clients` | List client organizations |
| `/donyati-projects` | List a client's projects/engagements |
| `/donyati-new-client` | Create a new client organization |
| `/donyati-new-project` | Create a new project/engagement |
| `/donyati-client-search` | Search a client/project's captured knowledge |
| `/donyati-add-knowledge` | Record new facts into a client/project (confirmed immediately) |
| `/donyati-briefing` | Pull a client briefing before a meeting |
| `/donyati-upload` | Add documents (RFP, transcript, notes) to a client/project |

**Deliverables** (Sales / Presales)

| Command | What it does |
|---|---|
| `/donyati-deliverables` | List and generate deliverables for a client — 8 types: RFP response (PowerPoint), executive summary, client briefing, assessment summary, custom summary (web decks), requirements, data source inventory, and response repository (Word docs) |
| `/donyati-rfp-response` | Generate an RFP/proposal response deck (PowerPoint) |
| `/donyati-demo-assessment` | Score a vendor demo against the client's requirement areas — evidence tier and a verified transcript citation behind every score |

**Documents & setup**

| Command | What it does |
|---|---|
| `/donyati-summarize` | Domain-aware document summarization |
| `/donyati-review` | Critical expert review — findings, gaps, risks, recommendations |
| `/donyati-sow-review` | Havagi CIO-style SOW/RFP audit before it goes to the client |
| `/donyati-start` | Guided entry point for new users |
| `/donyati-setup` | First-time setup (Claude Code) — verify your API key |
| `/donyati-help` | Plugin command reference |

**Admin**

| Command | What it does |
|---|---|
| `/donyati-posture` | Cloud compliance posture — admin keys only |

Run `/donyati-help` from inside Claude Code or Claude Desktop for the full reference. **These same commands now work as slash commands in the Claude Desktop connector** (see below).

---

## Platforms covered (26+)

Oracle EPM · Oracle ERP · Oracle Integration · Oracle AI · SAP · OneStream · Microsoft D365/Power Platform · Microsoft AI · Workday · Workday AI · Salesforce · Salesforce AI · Snowflake · Snowflake AI · Databricks · Databricks AI · Informatica · Stibo Systems · AWS · Google Cloud · CCH Tagetik · Anaplan · BlackLine · BlackLine AI · Planful · Prophix

Run `/donyati-platforms` for the live list.

---

## Setup — Claude Desktop / claude.ai web

Two steps, no terminal, no API key — your Donyati Microsoft 365 account *is* the authentication. Requires a Pro / Max / Team / Enterprise Claude plan.

1. **Open Settings → Connectors → Add custom connector** (Desktop: ⌘+, → Connectors. Web: profile menu → Settings → Connectors).
2. Fill in:
   - **Name:** `Donyati Expert Agents`
   - **Remote MCP server URL:** `https://expert-agents.donyati.com/api/mcp`
3. Click **Add** / **Connect**. A **Microsoft sign-in popup** opens — sign in with your Donyati account (same one you use for Outlook/Teams/the Expert Agents web app). Approve. The tools and slash commands appear.
4. **Verify:** type `/` in a new chat — you should see the `donyati-*` commands under the connector. Or say *"Use the Donyati whoami tool to confirm my access."*

**Slash commands now work in Desktop too.** The connector exposes the same `/donyati-*` commands as Claude Code (`/donyati-ask`, `/donyati-compare`, `/donyati-client-search`, `/donyati-add-knowledge`, `/donyati-briefing`, `/donyati-review`, `/donyati-sow-review`, `/donyati-summarize`, `/donyati-knowledge`, `/donyati-clients`, `/donyati-projects`, `/donyati-platforms`, `/donyati-agents`, `/donyati-help`) — they fill in a short form and drive the right tool for you. You can still just describe what you want in plain English instead. Full translation table + examples in `USAGE_GUIDE.md` §3B.

---

## Alternative: MCP-only install in Claude Code (no plugin)

If you want the MCP tools in Claude Code without the slash command shortcuts:

```bash
claude mcp add --transport http expert-agents \
  https://expert-agents.donyati.com/api/mcp \
  --header "Authorization: Bearer $DONYATI_API_KEY"
```

You'll get `consult`, `compare`, `summarize`, `review_document`, `review_sow`, `review_assessment`, `whoami`, `list_agents`, `list_platforms`, `list_organizations`, `create_organization`, `list_projects`, `create_engagement`, `search_knowledge`, `search_client_knowledge`, `add_client_knowledge`, `get_client_briefing`, `ingest_client_document`, `list_deliverables`, `generate_deliverable`, `search_compliance_posture`, `connect_readai`, `list_my_readai_meetings`, and `ingest_readai_meeting` — just no `/donyati-*` slash commands.

---

## Updating the plugin

To pull the latest version of the plugin from the marketplace:

```
/plugin marketplace update donyati-plugins
/plugin install donyati-expert-agents@donyati-plugins
```

Then restart Claude Code. A plain `/reload-plugins` does **not** fetch from the marketplace — you must re-run the `install` command to get the latest version.

---

## Roles & permissions

Per-caller RBAC is now live — your key or account only sees the client organizations and engagements you've been granted access to. Admin keys and service keys (`dea_*`) have unrestricted access. Regular keys operate under a "global until granted" model: until you're explicitly assigned to a client organization, you see all public data; once assigned, you see only that organization's knowledge.

If you ingest content via the plugin, it will be stamped with your owner email so admins can attribute submissions in the Review Queue.

**Usage is logged.** Every tool call records the tool name, response time, and outcome, attributed to your key's owner email (Claude Code) or your signed-in Microsoft 365 account (Desktop / web). For the expert-agent commands the question you asked is also stored (first 2,000 characters) with your email, model, and token counts — the answer is not. Donyati admins use this for adoption and cost reporting. See USAGE_GUIDE.md §2 "What gets logged".

A per-caller rate limit applies (60 req/min per server instance; connecting and listing tools are exempt). Exceeding it returns an explicit "rate limit exceeded" response with a retry delay rather than a silent failure.

---

## Admin links (Donyati admins only)

- **API keys**: https://expert-agents.donyati.com/admin/api-keys
- **API usage** (per-user, per-key, per-endpoint stats): https://expert-agents.donyati.com/admin/api-usage
- **Web app**: https://expert-agents.donyati.com

---

## Releasing a new plugin version (maintainers)

The marketplace installs from the **public repo `github.com/Donyati/donyati-plugins`** (branch
`main`, subdir `donyati-expert-agents-plugin/`) — NOT from Azure DevOps and NOT from this
internal repo. The standard ship flow pushes Azure only, so a plugin release is not live to
users until the public repo is synced:

1. Bump `plugin.json` version + update `CHANGELOG.md`.
2. Ship to dev/main via the normal Azure flow.
3. **Run `ORG=Donyati tools/sync-plugin-public.sh`** — copies the plugin into `Donyati/donyati-plugins` and pushes. This is the actual publish step.
4. Users pull it with `/plugin marketplace update donyati-plugins` + re-install.

---

## Need help?

- Stuck on setup? → see `USAGE_GUIDE.md` for the full walkthrough with troubleshooting
- Want ready-made prompts? → see `PROMPT_TEMPLATES.md` for copy-paste templates (Desktop + Code)
- Bug or feature request? → ping Matt
