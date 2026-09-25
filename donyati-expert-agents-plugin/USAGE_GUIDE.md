# Donyati Expert Agents — Plugin Usage Guide

**Version:** 2.11.0  ·  **Audience:** Sales / Presales / Marketing / Customer Success  ·  **Last updated:** 2026-09-25

This guide walks you from "I want to use this" to "I'm getting real work done from Claude" in under five minutes. Complements the README (quick reference) with troubleshooting, role-based workflow examples, and what to do when something goes wrong.

> **Works in four places.** Donyati Expert Agents runs in **Claude Code** (full plugin with `/donyati-*` slash commands, requires an API key), **Claude Desktop** (Mac/Windows app, via Custom Connector + Donyati SSO), **claude.ai web** (also via Custom Connector + SSO), and **ChatGPT** (via connector + SSO). Pick whichever fits your workflow — same backend, same answers, same access rules.
>
> **On ChatGPT?** Everything works except the `/donyati-*` slash commands, which are an MCP feature Claude surfaces and ChatGPT does not. You ask in plain English instead and get the same tools. Setup is §3C.

---

## 1. What this plugin does

The plugin connects **Claude** (Code, Desktop, or web) to Donyati's **Expert Agents platform** (`expert-agents.donyati.com`), and the same platform is reachable from **ChatGPT** through a connector. You get:

- **Expert consultations** — ask any of 26+ platform specialists a question
- **Multi-agent comparisons** — get unbiased platform comparisons (e.g. Snowflake vs Databricks)
- **Document summarization** — domain-aware summaries of proposals, SOWs, transcripts
- **Knowledge search** — pull verified citations from Donyati's curated knowledge base
- **Identity verification** — confirm your access before relying on it

In Claude Code and the Claude Desktop / web connector these come with `/donyati-*` slash commands. Everywhere — including ChatGPT — you can invoke the same tools in plain English (e.g., *"Use the Donyati OneStream expert to..."*) and the model picks the right tool.

---

## 2. Before you start

What you need depends on which client you're using:

| Using | What you need | Auth method |
|---|---|---|
| **Claude Code** | Claude Code CLI (https://claude.com/claude-code), Terminal, **an API key from Matt** (`mjanecek@donyati.com`) | `dea_*` API key |
| **Claude Desktop** | Claude Desktop app (https://claude.ai/download), **Pro / Max / Team / Enterprise plan**, your **Donyati Azure AD login** | SSO popup |
| **claude.ai web** | A browser, **Pro / Max / Team / Enterprise plan**, your **Donyati Azure AD login** | SSO popup |
| **ChatGPT** | A ChatGPT plan that allows custom connectors, your **Donyati Azure AD login** | SSO popup |

**Connector users — Claude Desktop, claude.ai and ChatGPT alike — do not need an API key** — you authenticate with your normal Donyati Microsoft 365 / Azure AD account when you add the connector. Claude Code is the only surface that uses the `dea_*` key system.

If you're on Claude Code and don't have a key yet, request one from Matt with:
- Your name (used to label the key in admin logs)
- **Your Donyati email** — every key is issued to a named owner, so your usage is attributed to you rather than landing in an anonymous bucket
- The team you're on (Sales / Presales / Marketing / CS)

You'll receive the key back, usually within a few hours.

### What gets logged

Worth knowing up front: **your tool calls are recorded.** Each call logs which command you ran, how long it took, and whether it succeeded, attributed to your key's owner email (Claude Code) or your signed-in Microsoft 365 account (any connector, Claude or ChatGPT).

Donyati admins use this for adoption and cost reporting — who is getting value from which agents, and what each agent costs to run.

To be straight with you about what that includes: for the expert-agent commands, **the question you asked is stored too** (up to the first 2,000 characters), alongside your email, the model used, and the token counts. The agent's *answer* is not stored by this telemetry. This is not new behaviour introduced by the plugin — the web app has always recorded it — but it is worth knowing when you are typing.

Practical upshot: treat these commands like any other internal Donyati system. Client-confidential detail in a prompt is fine where it would be fine in the web app; it is not anonymous.

---

## 3. Setup — Claude Code (3 commands, 5 minutes)

You'll run **one command in Terminal** (to save your key) and **two commands inside Claude Code** (to install the plugin). That's it — no git, no cloning, no path hunting.

> **Using Claude Desktop or claude.ai web instead?** Skip to §3B — it's even simpler (no terminal, no key, just SSO).

### Step 1 — Save your key as an environment variable (Terminal)

Open Terminal and paste this **one line** (replace `dea_paste_your_key_here` with the actual key Matt sent you), then press Enter:

```bash
echo 'export DONYATI_API_KEY="dea_paste_your_key_here"' >> ~/.zshrc && source ~/.zshrc
```

> **Linux / Windows WSL users:** replace `~/.zshrc` with `~/.bashrc` in both spots.

> **Important — straight quotes only.** When you paste, make sure the quotes are straight (`"`), not curly (`"`). Email clients and Slack sometimes convert them. If your install fails later with a 401 error, curly quotes are the most common cause.

To check that it stuck:

```bash
echo $DONYATI_API_KEY
```

You should see your key printed back. If you see nothing, the export didn't take — try opening a new Terminal window and running the echo again.

### Step 2 — Install the plugin (inside Claude Code)

Open Claude Code (just type `claude` in Terminal or use whatever method you normally do). Then type these two slash commands, one at a time:

```
/plugin marketplace add Donyati/donyati-plugins
```

Claude Code will clone the marketplace internally. You don't need to know git or have a GitHub login — Claude Code handles it.

```
/plugin install donyati-expert-agents@donyati-plugins
```

This registers the plugin, all `/donyati-*` slash commands, and the connection to the Expert Agents server.

### Step 3 — Verify

Still inside Claude Code:

```
/donyati-setup
```

You should see something like:

```
Key: matt-laptop
Owner: matt.janecek@donyati.com
Key ID: 12
Scopes: (none — full access in v2)
Rate limit: 60 req/min per server instance
```

(On Claude Desktop / claude.ai web there is no API key, so the same check shows
**User ID** and your Microsoft 365 address instead of a Key ID.)

If you see that, **you're done.** Try `/donyati-help` to see everything you can do. Then skip to §4.

If you see an error, jump to §7 (Troubleshooting).

### Never used Terminal before?

That's fine. Here's the minimum you need:

| Task | How |
|---|---|
| **Open Terminal (macOS)** | ⌘+Space, type "Terminal", press Enter |
| **Open Terminal (Windows)** | Search "WSL" or "Git Bash" in Start menu |
| **Paste a command** | ⌘+V (Mac) or Ctrl+Shift+V (Linux/WSL) |
| **Run a command** | Press Enter after pasting |
| **See what's in your env** | Type `echo $VARNAME` and press Enter |

That's all you need for this guide. If you get stuck at any step, screenshot the error and send to Matt.

---

## 3B. Setup — Claude Desktop and claude.ai (web)

Two steps, no terminal, no API key. Authentication is your normal Donyati Microsoft 365 / Azure AD login — same SSO popup you use everywhere else.

### Step 1 — Add the Custom Connector

**Claude Desktop:**
1. Open Claude Desktop → **Settings** (⌘+, on Mac) → **Connectors**
2. Click **Add custom connector** (may be under an *Advanced* section)
3. Fill in:
   - **Name:** `Donyati Expert Agents`
   - **Remote MCP server URL:** `https://expert-agents.donyati.com/api/mcp`
4. Click **Add** / **Connect**

**claude.ai web:**
1. Go to https://claude.ai → click your profile → **Settings** → **Connectors**
2. Scroll to **Custom Connectors** → **Add custom connector**
3. Same Name + URL as above. Click **Add**.

### Step 2 — Sign in with Azure AD

When you click Add/Connect, a **Microsoft sign-in popup** opens. Use your Donyati Microsoft 365 account — the same one you use for Outlook, Teams, and the Expert Agents web app. Approve the popup. The connector shows its tools **and slash commands** as available.

That's it. There's no API key field, no authorization header, no `Bearer` token to paste. Your Azure AD session *is* the authentication.

### Step 3 — Verify

Start a new Claude conversation and type:

> *"Use the Donyati whoami tool to confirm my access."*

Claude calls the `whoami` tool and reports back your name, email, and active scopes. If that works, **you're done.**

### Slash commands work here too

Type `/` in a new chat and you'll see the `donyati-*` commands listed under the connector. Pick one and it fills in a short form (e.g. the client name and what to search for), then drives the right tool for you — exactly like Claude Code.

You can also just describe what you want in plain English and mention "Donyati" or the tool name; Claude picks the right tool. Plain-English equivalents:

| Slash command | Or say (plain English) |
|---|---|
| `/donyati-ask` | "Ask the Donyati OneStream expert how they handle intercompany eliminations." |
| `/donyati-compare` | "Use Donyati to compare Snowflake and Databricks for AI/ML workloads." |
| `/donyati-client-search` | "Search Joyson's Donyati knowledge for the close timeline." |
| `/donyati-add-knowledge` | "Record in Donyati that Meridian's CFO wants to go live before year end." |
| `/donyati-summarize` | "Summarize this SOW using the Donyati summarize tool." *(paste content)* |
| `/donyati-review` | "Have the Donyati expert review this SOW for scope risk." *(paste content)* |
| `/donyati-sow-review` | "Use the Donyati review_sow tool on this SOW and give me the critical findings before I send it." *(paste content)* |
| `/donyati-knowledge` | "Search Donyati's knowledge base for predictive forecasting articles." |
| `/donyati-platforms` | "List all Donyati expert platforms." |
| `/donyati-agents` | "List all Donyati expert agents — platforms, industries, and specialty agents." |

All workflow examples in §5 work identically on Desktop/web — by slash command or plain English.

### Desktop/web troubleshooting

- **"Couldn't reach the MCP server" when adding the connector** → typically a typo in the URL. Confirm it's exactly `https://expert-agents.donyati.com/api/mcp` (note `/api/mcp` at the end).
- **Microsoft sign-in popup blocked** → unblock popups for claude.ai (web) or for the Claude Desktop app. The connector relies on a popup for SSO.
- **"Unauthorized" / Azure AD error after sign-in** → your Microsoft account isn't in Donyati's tenant. Sign in with your `@donyati.com` Microsoft account, not a personal one.
- **No "Custom Connectors" option visible** → your Claude plan doesn't include it. Upgrade to Pro / Max / Team / Enterprise, or fall back to Claude Code (any plan).
- **Tools appear but every call returns an error** → the OAuth access token may have expired (24 h lifetime). Open the connector settings and reconnect — the SSO refresh is silent if you're already signed into Microsoft elsewhere.

---

## 3C. Setup — ChatGPT

Same connector as Claude Desktop, same sign-in, no API key. What differs is that ChatGPT does not show the `/donyati-*` slash commands — see the note at the end of this section.

### Step 1 — Add the connector

1. In ChatGPT, open **Settings → Connectors** (on some plans this sits under a **Developer mode** or **Advanced** toggle; the exact place moves between releases and plans).
2. Choose to add a **custom connector / MCP server**.
3. Fill in:
   - **Name:** `Donyati Expert Agents`
   - **MCP server URL:** `https://expert-agents.donyati.com/api/mcp`
4. Add / connect.

If you cannot find a way to add a custom connector at all, your ChatGPT plan or your workspace admin does not allow them. That is a ChatGPT-side setting, not something Donyati controls — Claude Code works on any plan if you need a fallback.

### Step 2 — Sign in with Azure AD

A **Microsoft sign-in** opens. Use your Donyati Microsoft 365 account — the same one you use for Outlook, Teams, and the Expert Agents web app. Approve it. There is no API key field and no token to paste; your Azure AD session is the authentication.

### Step 3 — Verify

In a new chat:

> *"Use the Donyati whoami tool to confirm my access."*

You should get back your name, email, and active scopes. If that works, you're done.

### How you drive it — plain English, not slash commands

The `/donyati-*` commands are MCP **prompts**. Claude surfaces those; ChatGPT does not. Every command is still available to you as a **tool** — you just ask for it:

| Instead of | Say |
|---|---|
| `/donyati-ask` | "Ask the Donyati OneStream expert how they handle intercompany eliminations." |
| `/donyati-compare` | "Use Donyati to compare Snowflake and Databricks for AI/ML workloads." |
| `/donyati-clients` | "List my Donyati client organizations." |
| `/donyati-projects` | "List the Donyati projects for Joyson." |
| `/donyati-client-search` | "Search Joyson's Donyati knowledge for the close timeline." |
| `/donyati-add-knowledge` | "Record in Donyati that Meridian's CFO wants to go live before year end." |
| `/donyati-briefing` | "Give me the Donyati client briefing for Meridian." |
| `/donyati-summarize` | "Summarize this SOW using the Donyati summarize tool." *(paste content)* |
| `/donyati-review` | "Have the Donyati expert review this SOW for scope risk." *(paste content)* |
| `/donyati-sow-review` | "Use the Donyati review_sow tool on this SOW and give me the critical findings before I send it." *(paste content)* |
| `/donyati-knowledge` | "Search Donyati's knowledge base for predictive forecasting articles." |
| `/donyati-deliverables` | "Use Donyati to generate a client briefing deck for Meridian." |
| `/donyati-platforms` | "List all Donyati expert platforms." |
| `/donyati-agents` | "List all Donyati expert agents — platforms, industries, and specialty agents." |

**Name the client and the project.** The slash commands look the client up for you; in plain English you should say it — *"using Acme's FCC project"* — so the answer is grounded in that engagement's knowledge instead of being a generic platform answer.

### ChatGPT troubleshooting

- **No way to add a custom connector** → your plan or workspace admin does not allow them. Nothing to fix on the Donyati side.
- **"Couldn't reach the server"** → check the URL is exactly `https://expert-agents.donyati.com/api/mcp`, including `/api/mcp`.
- **"Unauthorized" after sign-in** → you signed in with a personal Microsoft account. Use your `@donyati.com` one.
- **You cannot see a client you expect** → access is your own, by Microsoft account, and identical to what you see in the web app. Connecting from ChatGPT grants nothing extra. Ask an admin for the client grant.
- **Tools stop working after a day** → the access token has a 24 h lifetime. Reconnect the connector; the refresh is silent if you are still signed into Microsoft.

---

## 4. The commands

> The `/donyati-*` slash commands work in **Claude Code and the Claude Desktop / web connector**. **ChatGPT does not show them** — use the plain-English phrasings in §3C instead. Same tools, same data, same answers, whichever way you ask.

**Talk to experts**

| Command | What it does | When to use it |
|---|---|---|
| `/donyati-ask` | Ask any platform expert | Quick fact-check, deep dive, "what's our take on X" |
| `/donyati-compare` | Multi-agent platform comparison | Pitch prep, RFP responses, scoping |
| `/donyati-knowledge` | Search curated knowledge base | Pull verified citations for content |
| `/donyati-accelerators` | Search the delivery accelerator library — scripts, templates, workbooks, toolkits | Before building something Donyati has already built |
| `/donyati-platforms` | List all platforms covered | When you don't know the right slug |
| `/donyati-agents` | Full agent roster — platforms, industries, Donyati specialty agents | When you want the complete picture, or an industry slug for the tip below |

> **Tip:** add `— industry: insurance` (or any of 15 other verticals — get the slug from `/donyati-agents`) to `/donyati-ask` for an industry-framed answer.

**Client & project knowledge** (Sales / Presales / CS)

| Command | What it does | When to use it |
|---|---|---|
| `/donyati-clients` | List client organizations | Find the right client |
| `/donyati-projects` | List a client's projects | Scope to one engagement |
| `/donyati-client-search` | Search a client/project's knowledge | Meeting prep, "what do we know about X" |
| `/donyati-add-knowledge` | Record new facts to a client/project (saved as drafts for admin review) | Right after a call or meeting |
| `/donyati-briefing` | Pull a client briefing | Before a sales call or status review |
| `/donyati-upload` | Add a document — RFP, transcript, notes — to a client/project | You have the file and want it searchable |
| `/donyati-case-study` | Write up an engagement as a case study — drafts the internal, client-named account and the sanitized version the agents can quote. **Claude Code only**: it drafts locally; submit the result with `/donyati-contribute` or paste it into `/case-studies/new` on the platform | The work is finished and the lesson is worth keeping |
| `/donyati-contribute` | Submit an accelerator, internal doc or case study for review; resubmit one sent back; list your submissions and their status. Files are scanned first: a blocked upload is never submitted | You built something reusable, or a reviewer asked for changes |
| `/donyati-new-client` | Create a client organization | The client is not in the list yet |
| `/donyati-new-project` | Create a project/engagement under a client | New workstream, new SOW, new assessment |
| `/donyati-poc` | Track a proof-of-concept — create, list, move it through its stages | Work that starts as a POC before it is a project |

**Deliverables** (Sales / Presales)

| Command | What it does | When to use it |
|---|---|---|
| `/donyati-deliverables` | List and generate deliverables for a client — 11 types: RFP response (PowerPoint), executive summary, client briefing, assessment summary, custom summary (web decks), requirements, data source inventory, response repository (Word docs), plus swimlane, roadmap and architecture diagram (interactive HTML) | Anytime you need a client-ready artifact fast |
| `/donyati-rfp-response` | Generate an RFP/proposal response deck (PowerPoint) | RFP due, proposal in flight |
| `/donyati-proposals` | Answer an RFP question by question: list proposals, read the question tree, draft grounded answers with sources, accept answers, export to Word | Working an RFP question by question rather than generating a whole deck at once |
| `/donyati-interactive` | Generate an interactive HTML deliverable — swimlane, roadmap, architecture diagram — with viewer, download and share links | A process or roadmap that reads better clickable than as a slide |
| `/donyati-demo-assessment` | Score a vendor demo against the client's requirement areas, with a verified transcript citation behind every score | Vendor selection, platform bake-off, demo debrief — see §4.10 |
| `/donyati-assessment` | Draft a client assessment from a project's ingested material, and check what became of the ones already drafted | You want the client to answer the questions the material does not |
| `/donyati-review-assessment` | Expert review of an assessment template's dimensions, questions and maturity levels | Before a template goes in front of a client |

For a more visual PowerPoint version of a client briefing, assessment summary, or custom
summary, ask for `deck_style: designed`. The option is ignored by non-PowerPoint deliverables,
and executive summaries stay `standard` because their denser content does not fit the designed
deck archetypes.

**Documents & setup**

| Command | What it does | When to use it |
|---|---|---|
| `/donyati-summarize` | Domain-aware summarization | Long SOW, meeting transcript, RFP packet |
| `/donyati-review` | Critical expert review | A deliverable before it goes to the client |
| `/donyati-sow-review` | Havagi CIO-style SOW/RFP audit | Before a SOW or RFP response goes to the client |
| `/donyati-start` | Guided entry point | Your first session, or showing someone else theirs |
| `/donyati-setup` | Verify your key (Claude Code) | First time, or after key rotation |
| `/donyati-help` | Show all commands | Anytime |

**Admin**

| Command | What it does | When to use it |
|---|---|---|
| `/donyati-posture` | Cloud compliance posture (admin keys only) | Security/compliance questions from a client or internal audit |

### 4.1 `/donyati-ask` — ask an expert

```
/donyati-ask How do you handle intercompany eliminations in OneStream?
/donyati-ask Best Anaplan calculation strategy for a 100M-cell driver model?
/donyati-ask What's the migration path from Hyperion Planning to Oracle EPBCS?
```

The platform is auto-detected from keywords. To force a platform:

```
/donyati-ask platform: snowflake — explain Cortex Search vs Cortex Analyst
```

### 4.2 `/donyati-compare` — head-to-head

```
/donyati-compare Snowflake vs Databricks for AI/ML workloads
/donyati-compare Anaplan vs Workday Adaptive vs Planful for FP&A
/donyati-compare SAP S/4HANA vs Oracle Fusion ERP for a mid-market manufacturer
```

Each platform's expert analyzes the topic independently — they don't see each other's responses — then a neutral synthesis layer pulls them into a fair comparison. **Output is grounded in Donyati's knowledge base, not vendor marketing.**

### 4.3 `/donyati-summarize` — domain-aware summaries

```
/donyati-summarize [paste an SOW]
/donyati-summarize [paste a meeting transcript] — output: action_items
/donyati-summarize [paste an assessment report] — content_type: assessment, output: full
```

**Options:**
- `content_type`: `proposal` · `sow` · `assessment` · `report` · `general`
- `platform`: any platform slug for domain context
- `output`: `executive_summary` · `key_points` · `action_items` · `full`

Output includes a summary, key points, and platform-specific insights.

### 4.4 `/donyati-knowledge` — search the knowledge base

```
/donyati-knowledge intercompany eliminations
/donyati-knowledge platform: oracle-epm query: data integration
/donyati-knowledge AIH Tagetik analytics
```

Returns up to 10 matching articles with title, platform/domain, and excerpt. Use these as **vetted citations** for marketing content, RFP responses, or briefings — they're sourced from Donyati's curated library, not the open web.

### 4.5 `/donyati-platforms` — list coverage

```
/donyati-platforms
```

No arguments. Returns the full list of platforms with article counts. Use this when you're unsure of the platform slug to pass to `/donyati-ask` or `/donyati-knowledge`.

### 4.6 `/donyati-agents` — full agent roster

```
/donyati-agents
```

No arguments. Returns the complete list of Donyati agents — platform experts, industry-vertical agents (16 slugs, e.g. `industry: insurance`), and Donyati specialty agents. Use this to find the right industry slug for the `/donyati-ask` tip above, or to see what's covered before a scoping call.

### 4.7 `/donyati-sow-review` — Havagi SOW/RFP audit

```
/donyati-sow-review [paste the SOW or RFP response]
```

Runs the document through **Havagi**, Donyati's CIO-style reviewer, via the `review_sow` tool. Returns a verdict, critical findings first, then a short list of edits to make before the client sees it. Use this the same way you'd use `/donyati-review`, but when the artifact specifically is a SOW or RFP response headed out the door.

### 4.8 `/donyati-setup` — verify your key

Already covered in §3 Step 3. Run this:
- After receiving a new key
- If you suspect your key was rotated
- If MCP tools start failing with auth errors

### 4.9 `/donyati-help` — command reference

```
/donyati-help
```

Shows all commands, grouped by team.

### 4.10 `/donyati-demo-assessment` — score a vendor demo

```
/donyati-demo-assessment <client> <vendor>
```

Turns a demo transcript into a scorecard against the client's requirement areas — evidence tier
and a verified transcript citation behind every score, with what the vendor deferred called out
rather than counted as a gap. For vendor selections, platform bake-offs, and demo debriefs. Pull
the transcript in from read.ai first if the demo was a recorded call, or upload it with
`/donyati-upload`.

---

## 5. Workflow examples by role

### 5.1 Sales / Presales — pitch prep

**Scenario:** You have a discovery call tomorrow with a manufacturer evaluating Anaplan and Workday Adaptive for FP&A.

```
/donyati-platforms
```
→ confirm we cover both. Note the slugs (`anaplan`, `workday`).

```
/donyati-compare Anaplan vs Workday Adaptive for mid-market manufacturer FP&A — emphasize sales planning and S&OP integration
```
→ get a side-by-side from independent experts plus a neutral synthesis. Drop highlights into your prep doc.

```
/donyati-knowledge migration from Excel and Hyperion to Anaplan
```
→ pull vetted points for "what does the journey look like" if asked.

### 5.2 Presales — SOW review before send

**Scenario:** The SOW is drafted and going to the client tomorrow morning.

```
/donyati-sow-review [paste the SOW]
```
→ Havagi audits it CIO-style: verdict first, critical findings before minor ones, then a short list of edits to make before it ships.

Fix the critical findings, re-run if the changes were substantial, then send.

### 5.3 Marketing — fact-checked content

**Scenario:** You're writing a blog post on AI features in modern FP&A platforms.

```
/donyati-knowledge query: predictive forecasting
```
→ pull articles across Anaplan PlanIQ, Workday Illuminate, Oracle EPM AI, Planful Predict.

```
/donyati-ask Compare Anaplan PlanIQ to Workday Illuminate for predictive forecasting — facts only, no marketing language
```
→ get an unbiased comparison you can adapt into copy with confidence.

```
/donyati-compare Snowflake Cortex AI vs Databricks AI Functions for embedded ML in data apps
```
→ generate a comparison table for a sidebar.

### 5.4 Customer Success — pre-call briefing

**Scenario:** You have a check-in with a client running Oracle EPBCS.

```
/donyati-ask What are common 30-day post-launch issues with EPBCS data integration?
```
→ get expert-level talking points anchored in Donyati's experience.

```
/donyati-summarize [paste last week's meeting transcript] — output: action_items
```
→ pull action items so you can confirm what's done before the call.

### 5.5 Anyone — quick fact-check

**Scenario:** You're in a conversation, someone makes a claim, you want to verify it.

```
/donyati-ask Does Workday Adaptive support row-level security on plan-input forms?
```

Two seconds, one expert, real answer. No browser tabs.

---

## 6. Tips & best practices

**Be specific.** "Best Anaplan model design for a 50M-cell workforce planning use case in retail" beats "Anaplan modeling tips." The experts have depth — give them context.

**Use `/donyati-platforms` first** if you don't know the slug. Saves a round trip.

**Combine commands.** Use `/donyati-knowledge` to find articles, then `/donyati-ask` to dig into a specific one.

**Comparisons need ≥ 2 platforms.** If you say "compare X" with no comparator, the system will pick one for you — usually fine but you have less control.

**Don't paste secrets.** Anything you send to `/donyati-summarize` lands in our LLM logs. Same rule as the web app.

**Ask follow-ups.** Each command is one turn — but Claude Code is conversational, so you can keep asking. The expert agent doesn't carry state, but Claude Code does.

---

## 7. Troubleshooting

### "DONYATI_API_KEY not set"

Your shell didn't pick up the export. Check:

```bash
echo $DONYATI_API_KEY
```

If empty, you either:
- Forgot to open a new Terminal window after running the export
- Used curly quotes (`"`) instead of straight (`"`)
- Added to the wrong file (`~/.zshrc` vs `~/.bashrc`)

Re-run the Step 1 line with the correct file path and straight quotes.

### "Unauthorized" / 401 errors

Three possibilities:
1. **Key revoked** — ask Matt to issue a new one
2. **Key has a typo** — re-copy from the original message; keys have no spaces
3. **You're using an old `EXPERT_AGENTS_API_KEY`** — v2 uses `DONYATI_API_KEY`. Re-run Step 1 with the new name

Test directly with curl:

```bash
curl -H "Authorization: Bearer $DONYATI_API_KEY" https://expert-agents.donyati.com/api/v1/whoami
```

A working key returns JSON with your key id, name, owner email, and scopes.

### `/plugin marketplace add` fails

If the marketplace add returns an error:
- Check your network — the command clones from GitHub internally
- Make sure you're on a recent Claude Code version (the marketplace feature is current as of late 2025)

If it still fails, fall back to the MCP-only install in §8.

### "Tool not found: <tool name>"

The plugin's MCP server isn't connected. Check:

```
/plugin
```

Then look at the Installed tab. If `donyati-expert-agents` is listed but tools are missing, run `/reload-plugins`.

If the plugin isn't installed at all, re-run Step 2 of §3.

### Slash commands not showing up

If `/donyati-ask` doesn't autocomplete, the plugin didn't load. Run:

```
/plugin
```

If `donyati-expert-agents` isn't in the Installed tab, re-run the marketplace install. If it's there but commands don't work, run `/reload-plugins`.

### Slow responses

The expert agents call out to OpenRouter; first response can take 5-15 seconds. Subsequent responses on the same conversation are usually faster. If responses consistently take > 30s, ping Matt — could be a model routing issue.

### Rate limit hit

Default is 60 requests/minute per caller, enforced per server instance. Connecting and listing tools is exempt, so reconnecting never costs you quota — only actual tool calls count.

If you go over, you get an explicit "rate limit exceeded" response with a retry delay rather than a silent failure or a dropped connection. Wait the stated number of seconds and carry on, or ask Matt to raise your limit.

---

## 8. Fallback: MCP-only install in Claude Code (no plugin)

If the marketplace install in §3 gives you trouble, you can install just the MCP server in Claude Code. You'll get all the tools but lose the `/donyati-*` slash command shortcuts (you'll call the tools by their MCP names instead — e.g., ask Claude Code *"use the consult tool to ask…"*).

In Terminal:

```bash
claude mcp add --transport http expert-agents \
  https://expert-agents.donyati.com/api/mcp \
  --header "Authorization: Bearer $DONYATI_API_KEY"
```

Verify with:

```bash
claude mcp list
```

You should see `expert-agents` in the list. Open Claude Code and ask it to list available MCP tools.

*(For Claude Desktop / claude.ai web there's no plugin to begin with — the Custom Connector + SSO setup in §3B is the standard path, not a fallback.)*

---

## 9. Privacy & data handling

- **API keys are scoped to you.** Each teammate gets a unique key. Don't share them.
- **RBAC is live.** Your key or account only sees client organizations and engagements you've been granted access to. Admin keys and service keys have unrestricted access. Regular keys follow a "global until granted" model: you see all public data until assigned to a specific organization, then you see only that organization's knowledge.
- **Requests are logged.** Admins can see per-user / per-key usage at `/admin/api-usage` for billing and audit.
- **Content you summarize/ingest is stored.** It can land in the knowledge base or Review Queue depending on the endpoint. Don't paste anything you wouldn't paste into the web app.

---

## 10. Recent releases

**Shipped in v2.11.0** — contribute from Claude:

- `/donyati-contribute` — submit an accelerator, internal doc or case study for review, or check
  where your submissions stand. Tools: `submit_accelerator`, `submit_internal_doc`,
  `submit_case_study`, `list_my_submissions`. Same scan and review queue as **Knowledge ›
  Contribute** on the web; nothing is published until an approver says so. Needs a signed-in
  person, not a service key.

**Shipped in v2.8.0** — vendor demo scoring:

- `/donyati-demo-assessment` — score a vendor's demo against the client's requirement areas,
  with an evidence tier and a verified transcript citation behind every score (§4.10)
- read.ai import: `connect_readai`, `list_my_readai_meetings`, `ingest_readai_meeting` — pull a
  recorded call in as a project document without leaving the chat

**Shipped in v2.7.0** — the accelerator library:

- `/donyati-accelerators` — search the reusable scripts, templates, workbooks and toolkits
  consultants have already built. Published accelerators only, so it looks empty until
  contributors publish into it

**v2.6.0 — interactive deliverables — is written but not published.** The skill and its three
tools are on the `dev` branch; the published plugin does not carry them, which is why 2.7.0
skipped the number rather than reusing it. When it lands you get `/donyati-interactive`, which
authors a swimlane, roadmap or architecture diagram as a JSON spec, renders it to a
self-contained interactive HTML page, and returns viewer, download and revocable share links.

**Shipped in v2.4.0** — SOW review, agent roster, and compliance posture:

- `/donyati-sow-review` — Havagi CIO-style SOW/RFP audit before it goes to the client
- `/donyati-agents` — full agent roster (platform + industry + Donyati specialty agents)
- `/donyati-posture` — cloud compliance posture queries (admin-only)
- Industry lens on `/donyati-ask` — `industry: insurance` (and 15 more verticals)
- 4 new deliverable types via `/donyati-deliverables` — assessment summary, requirements, data source inventory, response repository
- Web decks consolidated into `/donyati-deliverables` — the standalone deck command from v2.3.0 was retired since it did the same job

**Shipped in v2.3.0** — deliverables generation and document management:

- `/donyati-new-client`, `/donyati-new-project` — create client organizations and engagements
- `/donyati-upload` — add client documents (RFP, transcripts, notes)
- `/donyati-deliverables` — list and generate client deliverables
- `/donyati-rfp-response` — generate proposal/RFP response decks (PowerPoint)
- `/donyati-start` — guided entry point for new users

---

## 11. Getting help

- **Setup or auth issues** → run `/donyati-setup` first
- **Bugs or feature requests** → ping Matt (`mjanecek@donyati.com`)
- **Web app**: https://expert-agents.donyati.com
- **Admin (key management)** *(admins only)*: https://expert-agents.donyati.com/admin/api-keys
- **Usage stats** *(admins only)*: https://expert-agents.donyati.com/admin/api-usage
- **README**: short reference at `donyati-expert-agents-plugin/README.md`
- **Prompt templates**: copy-paste reusable prompts at `donyati-expert-agents-plugin/PROMPT_TEMPLATES.md`

---

## 12. Plugin reach — what is web-only, and why

Not everything in the Expert Agents web app is meant to be reachable from Claude. Some of it is
held back on purpose; some of it is a gap somebody is tracking. Without that written down, the
two look identical from the connector — you ask for a client's contacts, nothing happens, and
you cannot tell whether that is a bug worth reporting.

This table is the answer for the features shipped between 20 August and 4 September 2026.
Measured against the connector on 2026-09-07.

| Feature | On the connector? | Why |
|---|---|---|
| **Client contacts** | No — web-only for now | Deliberate. The contacts design puts `/v1` sync and an MCP tool in **Phase 3, step 10**, and blocks both on a field-level precedence table being implemented first. Until an external writer knows which system wins per field, a connector write would silently overwrite a curated contact. Ask an admin, or use the Contacts tab on the client |
| **Client references** | No | A tracked gap, not a decision: **AB#6083** covers reference search on `/v1` and MCP. Approved client references are exactly the thing you want to hand when a prospect asks who else you have done this for, so this is a real absence |
| **Lineage read** | **Yes** — `get_lineage` | Closed since this was reported. Pass a `pocSlug` or a `projectId` and it walks the funnel behind and ahead of it, including a conversion later undone. Wrapped by `/donyati-poc` and `/donyati-projects` |
| **Assessment proposals** | On MCP as `propose_assessment`; **not** on the `/v1` REST API | Split on purpose. Drafting and listing are conversational work and reach the connector via `/donyati-assessment`. **Pushing** a proposal makes it client-facing and **withdrawing** takes it back from respondents mid-flight; both stay signed-in acts in the admin console. The missing `/v1` route is a gap for server integrations, under **AB#6104** |
| **Per-POC Azure cost** | Partly — `list_pocs` returns spend to a caller holding the cost permissions | Reading is reachable and gated (see `/donyati-poc`). **Administering** it is not: attributing a resource to a POC rewrites the cost map for every reader of it, and a mis-attribution stays wrong until someone notices. That stays on the mapping page |
| **POC → SOW** | No — web-only | Deliberate. The action creates a `client_engagements` row and generates a client SOW against it, and it is guarded by a scope assertion before any write. A one-shot connector call that creates a client record as a side effect is the wrong shape for it. Start the POC from `/donyati-poc`, then generate the SOW from the POC page |

**What "web-only" costs you.** Every row above marked web-only means a consultant working in
Claude has to open a browser to finish the job. That is a real cost and it is accepted here for
specific reasons — a write that could overwrite curated data, an act that reaches the client, or
a side effect that creates records. It is not a general position that the connector is a
read-only surface.

**Where to file the gaps.** Reference search on the connector is AB#6083. Remaining demo
assessment surfaces are AB#5972. Everything else on this page: ping Matt.

---

*© 2026 Donyati, LLC. Internal use only.*
