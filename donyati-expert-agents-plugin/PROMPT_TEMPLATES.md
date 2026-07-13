# Donyati Expert Agents — Reusable Prompt Templates

**Version:** 2.4.0  ·  **Audience:** Sales / Presales / Marketing / Customer Success  ·  **Last updated:** 2026-07-12

Copy-paste prompt templates for the Donyati Expert Agents connector. Written for **Claude Desktop / claude.ai web** (plain English — Claude picks the MCP tool). Each template lists the **Claude Code** slash-command equivalent too.

**How to use:** copy a template, replace every `<PLACEHOLDER>`, paste into Claude. Keep the literal words *"Donyati"* and the tool name — that's what routes the request to this connector.

> **Never paste client secrets, credentials, or PII.** Everything sent to `consult` / `summarize` / `review_assessment` lands in Donyati's LLM logs. Same rule as the web app.

---

## 1. Oracle EPM client document review (headline flow)

**When:** you have a client SOW, design doc, status report, or assessment and want it reviewed through an Oracle EPM expert lens. Swap `oracle-epm` for any platform slug (`sap`, `onestream`, `workday`, …).

### 1A. Full review flow (scope → summarize → critique → cite)

Run these as four turns in one Claude conversation so context carries:

```
Turn 1 — scope
List Donyati client organizations matching "<CLIENT NAME>".

Turn 2 — confirm engagement
List Donyati projects for organization id <ORG ID FROM TURN 1>.

Turn 3 — domain summary
Use the Donyati summarize tool on the document below.
platform: oracle-epm
content_type: <report | sow | proposal | assessment>
output: full
Give me the summary, key points, and Oracle EPM-specific insights.

<PASTE DOCUMENT TEXT>

Turn 4 — expert critique
Use the Donyati consult tool, platform oracle-epm. Acting as our Oracle EPM
delivery expert, review the same document for: technical gaps, integration
risks, missing close/consolidation controls, and anything that contradicts
EPM Cloud leading practice. Be specific and cite where in the document.
```

### 1B. Single-shot critique (skip scoping)

```
Use the Donyati consult tool with platform oracle-epm. Act as our Oracle EPM
expert reviewing this client document. Flag technical gaps, integration risks,
missing controls, and weak assumptions. Cite the section for each finding.
End with the top 3 things to fix before this goes to the client.

<PASTE DOCUMENT TEXT>
```

**Claude Code:** `/donyati-ask platform: oracle-epm — review this client doc for gaps and risks: <text>`

---

## 2. Assessment / maturity snapshot review

**When:** the artifact is a structured assessment exported as JSON (dimensions, questions, maturity levels, recommendations). Returns impact-ranked suggestions, not prose.

```
Use the Donyati review_assessment tool.
expertId: <oracle-epm | sap | onestream | workday | ...>
expertName: <e.g. Oracle EPM>
Here is the assessment snapshot JSON:

<PASTE JSON SNAPSHOT>
```

**Claude Code:** no direct slash command — phrase it the same way; Claude Code calls the MCP tool.

---

## 3. Cross-platform comparison (pitch / RFP prep)

**When:** prepping for a discovery call, RFP response, or scoping where the client is weighing ≥2 platforms. Independent experts analyze separately, then a neutral layer synthesizes — grounded in Donyati's KB, not vendor marketing.

```
Use Donyati to compare <PLATFORM A> vs <PLATFORM B> [vs <PLATFORM C>] for
<USE CASE — e.g. mid-market manufacturer FP&A>. Emphasize <DECISION DRIVERS —
e.g. sales planning, S&OP integration, time-to-value>. Give me the independent
expert takes plus the neutral synthesis, and a one-paragraph recommendation
I can put in a prep doc.
```

**Claude Code:** `/donyati-compare <Platform A> vs <Platform B> for <use case> — emphasize <drivers>`

> Always name ≥2 platforms. If you give only one, the system picks a comparator for you.

---

## 4. Pre-call client briefing (Customer Success)

**When:** a check-in or QBR with an existing client; you want talking points anchored in Donyati's experience plus open action items.

```
Turn 1
List Donyati projects for organization id <ORG ID> (run a Donyati org search
for "<CLIENT>" first if you don't have the id).

Turn 2
Use the Donyati consult tool, platform <PLATFORM>. Give me likely talking
points and common 30/60/90-day post-launch issues for a client running
<PRODUCT — e.g. Oracle EPBCS>, focused on <THEIR CURRENT PHASE>.

Turn 3 (optional)
Use the Donyati summarize tool, output action_items, on last meeting's notes:

<PASTE TRANSCRIPT / NOTES>
```

**Claude Code:** `/donyati-ask platform: <slug> — common post-launch issues for <product>` then `/donyati-summarize <notes> — output: action_items`

---

## 5. Document summarization (SOW / transcript / proposal)

**When:** a long document you need digested, optionally through a platform lens.

```
Use the Donyati summarize tool on the content below.
content_type: <proposal | sow | assessment | report | general>
platform: <slug, or omit for no domain lens>
output: <executive_summary | key_points | action_items | full>

<PASTE CONTENT>
```

**Claude Code:** `/donyati-summarize <content> — content_type: <type>, output: <format>`

---

## 6. Knowledge-backed content & claims (Marketing)

**When:** writing a blog post, RFP answer, or one-pager and you need *vetted* facts (Donyati's curated library, not the open web).

```
Turn 1 — find sources
Search Donyati's knowledge base for "<TOPIC>"<, platform <slug> if relevant>.

Turn 2 — turn into copy
Use the Donyati consult tool<, platform <slug>>. Using only Donyati's verified
positions, write <FORMAT — e.g. 3 tight paragraphs / a comparison table /
5 bullet claims> on <TOPIC>. Facts only, no vendor marketing language.
```

**Claude Code:** `/donyati-knowledge <topic>` then `/donyati-ask <draft request>`

---

## 7. Quick expert fact-check

**When:** mid-conversation, someone makes a claim, you want it verified in seconds.

```
Donyati consult, platform <slug>: <YES/NO OR FACTUAL QUESTION —
e.g. "Does Workday Adaptive support row-level security on plan-input forms?">
```

**Claude Code:** `/donyati-ask <question>` (auto-routes by keywords)

---

## 8. Discover what's available

```
List all Donyati expert platforms with article counts.
```
Use this when you don't know the right platform slug. **Claude Code:** `/donyati-platforms`

```
Use the Donyati whoami tool to confirm my access.
```
First-time check or after an auth error. **Claude Code:** `/donyati-setup`

---

## 9. Havagi SOW review before send

**When:** the SOW or RFP response is drafted and about to go to the client — you want a CIO-style gut check on scope, assumptions, and risk before it ships.

```
Use the review_sow tool on the attached SOW. Give me the verdict, the critical findings first,
and a short list of edits I should make before the client sees it.

<PASTE SOW / RFP RESPONSE TEXT>
```

**Claude Code:** `/donyati-sow-review [paste the SOW]`

---

## Platform slugs (quick reference)

`oracle-epm` · `oracle-erp` · `oracleai` · `sap` · `onestream` · `microsoft` · `microsoftai` · `workday` · `workdayai` · `salesforce` · `salesforceai` · `snowflake` · `snowflakeai` · `databricks` · `databricksai` · `informatica` · `stibo` · `aws` · `gcp` · `tagetik` · `anaplan` · `blackline` · `blacklineai` · `planful` · `prophix`

Run template 8 (`list_platforms`) for the live list with article counts.

---

*© 2026 Donyati, LLC. Internal use only. Companion to `README.md` and `USAGE_GUIDE.md`.*
