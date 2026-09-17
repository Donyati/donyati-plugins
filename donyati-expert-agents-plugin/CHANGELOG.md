# Changelog

All notable changes to the Donyati Expert Agents plugin are documented here. Version numbering follows [Semantic Versioning](https://semver.org/).

## [2.10.0] — 2026-09-17

**Note:** the MCP server this plugin talks to is production. Nothing below is live for plugin
users until it is promoted to `main` and published with
`ORG=Donyati tools/sync-plugin-public.sh` — a separate step from this branch merging to `dev`.

### Fixed
- **Seven commands existed and could not be reached from Claude Desktop (AB#6718).** A
  `/donyati-*` command appears in Desktop, claude.ai and any other MCP client only because the
  server registers it as a **prompt**. There were 30 skills and 21 prompts. `/donyati-accelerators`,
  `/donyati-assessment`, `/donyati-demo-assessment`, `/donyati-interactive`, `/donyati-poc`,
  `/donyati-proposals` and `/donyati-review-assessment` are now registered, so all three surfaces
  carry the same 28 commands. Two of the seven were the sharper case: accelerators and vendor-demo
  scoring were **published and live in production**, so every connector user had the tools and no
  way to know the commands existed.
- **The gap is now a test, not a convention.** `frontend/lib/mcp/__tests__/prompt-per-plugin-skill.test.ts`
  fails if a skill has no prompt, if a prompt has no skill, or if the Code-only exemption list goes
  stale. `/donyati-setup` and `/donyati-case-study` are the only exemptions and each records why it
  cannot work over a connector. Nothing caught this drift for weeks because nothing was looking.
- **Version reconciliation.** The public marketplace was serving **2.9.0** while both branches read
  2.8.0: `9409506b` cut 2.9.0 on `main`, then the AB#6296 promotion tail ported `dev`'s
  `plugin.json` over it and took the version back, dropping the changelog entry too. The 2.9.0
  entry is restored below for history and this release moves past it. Treat a version file as
  something a whole-file promotion can silently revert.

### Added
- **`/donyati-proposals`** — Answer an RFP question by question: list proposals, read the question tree, draft grounded answers with sources, accept answers, export to Word.
- **ChatGPT setup and a straight answer about what differs (AB#6286).** The docs described the
  connector as a Claude-only route; it serves any MCP client and we have teammates on ChatGPT.
  `USAGE_GUIDE.md` gains §3C (add the connector, sign in, verify, troubleshoot) and README gains a
  matching section. The one real difference is stated wherever a reader could be misled: the
  `/donyati-*` slash commands are MCP **prompts**, which Claude surfaces and ChatGPT does not, so
  ChatGPT users ask in plain English — a full translation table is in §3C. Tools and access rules
  are identical, so connecting from a different client grants nothing extra.

### Fixed
- The README and USAGE_GUIDE headers still advertised **v2.4.0** while `plugin.json` was on 2.8.0.

- **`/donyati-poc`, `/donyati-review-assessment` (AB#6256).** Two new skills close a gap where the
  MCP tool existed but nothing taught it: `/donyati-poc` covers `list_pocs`/`create_poc`/
  `update_poc`; `/donyati-review-assessment` covers `review_assessment` — expert critique of an
  assessment *template* before it's published, distinct from `/donyati-assessment` (drafts a
  client-specific proposal) and `/donyati-demo-assessment` (scores a vendor demo). `/donyati-
  interactive` now documents `revoke_share_links`, `create_collection_link` and
  `revoke_collection_link`; `/donyati-demo-assessment` documents `connect_readai` and
  `list_my_readai_meetings` as the two steps before `ingest_readai_meeting`, plus its `title`
  parameter, the Word export link, and the 120k/30k character truncation limits.
- **`/donyati-poc` now says what the tools refuse (AB#6256).** The skill shipped describing the
  happy path only: it said `killed` takes a `killedReason` and stopped there. It now carries the
  three rules a caller hits in practice — a graduated or killed POC is **final** and refuses
  every edit, `killedReason` is required for **parked as well as killed**, and Azure spend on
  `list_pocs` is withheld without the cost permissions and always withheld from a `dea_*` key.
  A skill that describes only what works turns a refusal into a bug report.
- **Client context on the expert commands (AB#6246).** `/donyati-ask`, `/donyati-compare`,
  `/donyati-review`, `/donyati-sow-review` and `/donyati-summarize` now resolve the client
  before they ask: `list_organizations` for the `organizationId`, `list_projects` for the
  `projectId`. The underlying `consult`, `compare`, `summarize`, `review_document` and
  `review_sow` tools accept both, and the answer is grounded in that client's confirmed
  knowledge — `/donyati-ask` also uses the engagement wiki, matching what the web app does.
  Omit both arguments for the generic platform answer these commands gave before.
- Each command refuses an organization you are not granted, and refuses a `projectId` sent
  without an `organizationId`, rather than answering from a client name.
- **`create_poc` takes an `engagementId` (AB#6250).** A POC started from Claude can now be
  linked to its Project in the same call, instead of needing a follow-up `update_poc`
  before it shows up in any lineage. The Project has to belong to the same client.
- **Assessment lineage on the two write commands (AB#6130, AB#6257).** `/donyati-add-knowledge`
  and `/donyati-upload` can now record which engagement and which assessment a result came
  from: `engagementExternalId`, `assessmentRef`, `assessmentName` and `validAsOf`, the four
  fields the v1 ingest API has taken since AB#6130 while the plugin could record none of them.
  Rows sharing an `assessmentRef` supersede each other, so a re-run replaces last quarter's
  score instead of sitting next to it. The externalId is resolved inside the client you named
  and nowhere else, and if it matches no project the reply says so rather than landing the
  facts at organization level in silence.
- **`get_lineage` (AB#6257).** Walk the funnel behind a POC or a project: which opportunities
  and demos converted into it, the project a POC delivers through, and any conversion that was
  later undone. Documented under `/donyati-projects`. There was no lineage read on the
  connector or the API at all before this.
- **`/donyati-assessment` (AB#6252)** — draft a client assessment from what has already been
  ingested for a project, aimed at the topics that material does not answer, and ask what
  became of the ones already drafted. `propose_assessment` takes a new `list: true` operation
  reporting each proposal's status, its participant link, and — this is the one that matters —
  whether a **pushed** proposal has been edited since, because until someone pushes again the
  client is still answering the older version. Listing needs Clients (View); drafting still
  needs Clients (Manage). Publishing and withdrawing stay in the admin console: making an
  assessment client-facing is a signed-in human act, not a connector call.
- `list_projects` marks projects managed in Discovery to Delivery and links back; `get_client_briefing` lists approved DDO artifacts. Skill text updated to say where Oracle EPM projects are created.
- **A "Plugin reach" table in USAGE_GUIDE.md §12 (AB#6256).** Six features from the Aug–Sep
  batch were reported as having no connector surface: client contacts, client references,
  the lineage read, assessment proposals on the v1 API, per-POC Azure cost, and POC → SOW.
  Only the contacts one said anywhere that this was deliberate. The table records, per
  feature, whether it is web-only on purpose and why, or a tracked gap and under which work
  item, so someone asking "why can't I do this from Claude" gets an answer instead of filing
  it again. One of the six has closed since the review: `get_lineage` shipped with AB#6257.

### Changed
- **`update_poc` now refuses what the POC page refuses (AB#6250).** A **graduated or killed
  POC is final**: every edit is refused, including its stage, its budgets and its linked
  Project. Parking is the reversible pause, and moving to **killed or parked** requires
  `killedReason` in the same call. The connector used to check only that a stage was a
  legal value, so it could reopen a graduated POC or kill one with no reason recorded.
- **`list_pocs` no longer returns Azure spend to every caller (AB#6250).** Per-POC spend
  needs the Products permission together with Product Costs or Financial Actuals — the same
  grants the POC Budget tab asks for — and is **never** returned to a `dea_*` API key,
  because a key carries scopes rather than roles and no scope can express cost access. The
  linked Project needs Products, or the `projects` scope on a key. When spend is withheld
  the reply says so and names the permission, so an answer does not read as "no cost".

### Fixed
- **Backfilled the missing 2.7.0 and 2.8.0 sections below (AB#6256).** `plugin.json` reached
  2.8.0 with nothing written down for either release — this file jumped straight from
  `[Unreleased]` to `[2.6.0]`. README's `/donyati-deliverables` row claimed 8 deliverable types;
  the connector has served 11 (three interactive HTML types) since AB#5639, and
  `/donyati-accelerators` was missing from README entirely. USAGE_GUIDE.md and
  usage-guide-content.json never mentioned `/donyati-demo-assessment`.
- **This section had two `### Added` headings, two `### Fixed` headings, and a `### Note` saying
  there is no `/donyati-poc` command (AB#6256).** Six branches wrote into `[Unreleased]` in the
  same fortnight and each merge kept both sides, which is right for content and wrong for
  structure. The note was true when it was written and false by the time it merged — the same
  release that added the command still shipped a changelog telling readers it did not exist.
  Merged into one Added / Changed / Fixed set.
- **README said v2.4.0 while `plugin.json` said 2.8.0 (AB#6256).** The headline version had not
  moved in four releases; the command table below it already said 2.8.0, so the file disagreed
  with itself on the same page. Its `/donyati-add-knowledge` row still promised facts were
  "confirmed immediately", the exact claim the skills were corrected for below. Its two tool
  lists — the MCP-only install and the Desktop slash commands — were also years of releases
  behind: the install list named 24 of the connector's 34 tools, omitting every POC, lineage,
  accelerator, demo-assessment and share-link tool.
- **USAGE_GUIDE.md §4 listed 18 of the 28 commands (AB#6256).** `/donyati-accelerators`,
  `/donyati-new-client`, `/donyati-new-project`, `/donyati-upload`, `/donyati-interactive`,
  `/donyati-demo-assessment`, `/donyati-review-assessment`, `/donyati-assessment`,
  `/donyati-poc` and `/donyati-start` were all missing from the tables a new user reads first;
  `/donyati-demo-assessment` had a whole subsection at §4.10 and no row pointing to it. The
  deliverables row still said 8 types where README said 11.
- **A correction to a scorecard row can be undone, and it says who made it (AB#6249).** Editing
  a row overwrote it with no record of what was there. That is worst on the edit that loses the
  most: a row moved off `deferred` has no stored score, so once the edit landed the previous
  value was gone. `assess_vendor_demo` takes `operation: "undo-row"` with a `rowId` and puts the
  row back exactly as it stood, and the scorecard in the web app has the same Undo against any
  corrected row. Undoing is itself recorded, so a second undo steps one correction further back.
- **A final scorecard refuses corrections (AB#6249).** `edit-row`, `undo-row` and `reweight` are
  all refused with an explanation until you reopen it with `set-status: "draft"`. Marking one
  final used to be labelling only.
- **The reply leads with the truncation counts (AB#6249).** When a transcript over 120,000
  characters or notes over 30,000 are cut to fit the scoring prompt, the reply now opens with
  ⚠️ and the exact counts — on a re-read and after a correction too, not only on the first run.
  `/donyati-demo-assessment` says to report them ahead of the scores. The warning was previously
  below the headline score, which is where a summary drops it.
- **A downgrade names the tier it fell from, and only where one happened (AB#6249).** The
  scorecard used to infer a downgrade from "fallback tier plus an unverifiable quote", which is
  also what the model writes for a capability that simply never came up. Every `not-addressed`
  row carrying a citation was reported as "a deferral was claimed but not found in the
  transcript" — including in the Word export that goes to the client. It now reads "recorded as
  Deferred and lowered to Not addressed", and only when a deferral was genuinely asserted.
- **`assess_vendor_demo` needs Deliverables (Manage), and a listing is per project (AB#6248).**
  The tool had no permission check at all, so anyone who could reach the connector could spend
  model budget scoring a client's demo transcript and could rewrite a stored scorecard — both
  of which need Deliverables (Manage) in the browser. It now needs the same grant for every
  call, and `dea_*` server keys stay exempt. `list: true` requires a `projectId`: it used to
  list a whole client, which showed a consultant granted one project the vendor, score and id
  of every scorecard on that client's other projects — and an id from that listing then opened.
  Naming a project you were not granted lists nothing rather than that project's scorecards.
  `/donyati-demo-assessment` documents both.
- **An `edit-row` carrying only a `rowId` is refused (AB#6248).** It was not the no-op it looked
  like: an empty edit of a *deferred* row wrote a 0 over its absent score and re-derived the
  row's evidence around it. The web form has always refused an empty edit; the connector and the
  v1 API now do too.
- `/donyati-add-knowledge` and `/donyati-upload` said items were "confirmed immediately and
  searchable right away". They are saved as drafts pending admin review, and have been for as
  long as the tools have existed.
- **`assess_vendor_demo` no longer offers demo/synthetic documents as candidate transcripts
  (AB#6242).** Documents an admin has marked as *fixture* were listed alongside real ones, so
  a seeded sample transcript could be scored as if it were a genuine vendor demo. The same
  exclusion now applies to the source text behind document-mode `generate_deliverable`.
  `/donyati-demo-assessment` says so at the discovery step.

### Production parity

**Resolved 2026-09-09.** The section formerly here (measured 2026-09-07) listed skills,
tools and changelog releases that `dev` had and `main` did not; the 2026-09-08 `dev` →
`main` promotion carried all of it over. Re-measured today: `git diff azure/main azure/dev
-- donyati-expert-agents-plugin/` is empty, and both branches register the same 29 skills
and 34 MCP tools.


## [2.9.0] — 2026-09-07

> Recovered 2026-09-17. This release was cut on `main` (`9409506b`) and published to the
> marketplace, then lost from the repository when the AB#6296 promotion tail ported `dev`'s
> files over it. It is restored here so the published history and the repository agree.

### Added
- **`/donyati-case-study`** — help writing an engagement up, for the consultant who has the story
  and not the two documents. A case study on the platform is a linked pair: an internal account
  that names the client and never reaches an expert agent, and a sanitized version that does. The
  skill interviews first — pushing on what people leave out, specifically what was tried that did
  not work, how the fix was verified, and where it fell short — then drafts the internal face in
  full and **derives** the sanitized one from it paragraph by paragraph, which is the order that
  produces a sanitized version worth reading. It carries the four sanitization moves from the
  first case study in the corpus, including the one that is easy to get backwards: vendor
  internals stay, client internals go, because sanitizing is not vagueness. **It is honest about
  what it cannot do** — there is no MCP tool for case studies, so it does not submit, and it says
  the real client-name scan runs at publish time against every org name in the live database
  rather than implying it has run here. It also states up front that submitting freezes editing,
  that a reviewer publishes rather than the author, and that the author cannot publish their own
  case study even holding approve. **Claude Code only, deliberately**: every other command here is
  an MCP prompt, which is what makes it appear in Desktop, so this one will not — the README says
  that is expected rather than a broken install. (AB#6292)

## [2.8.0] — 2026-09-01

### Added
- **`/donyati-demo-assessment`** — score a software vendor's demo against a client's requirement
  areas, with a verified transcript citation behind every score. Evidence tiers
  (`demonstrated` / `discussed` / `claimed` / `not-addressed` / `deferred`) mean demonstrated
  capability always outranks anything merely claimed, and a vendor's own deferral is tracked
  separately from a genuine gap rather than scored as one. Reports per-area coverage alongside
  the score, and each area can be weighted must-have / important / nice-to-have. Calls the new
  `assess_vendor_demo` tool, and is correctable afterward — reweight an area, fix a row's score
  or citation, or finalize/reopen — without leaving the conversation.
- read.ai meeting import: `connect_readai` links your account, `list_my_readai_meetings` finds a
  recent call, `ingest_readai_meeting` pulls its transcript in as a project document — the fastest
  way to get a recorded demo into `/donyati-demo-assessment`.

## [2.7.0] — 2026-08-25

### Added
- **`/donyati-accelerators`** — search Donyati's delivery accelerator library: the reusable
  scripts, templates, workbooks and toolkits consultants have already built, mostly EPM. Filter
  by `platform:` and `category:` (`script`, `template`, `workbook`, `toolkit`, `document`). It
  calls the new `search_accelerators` MCP tool, so it works from the desktop connector as well
  as the CLI plugin.

  The library returns **published** accelerators only. It will look empty until contributors
  publish into it, which is expected on day one rather than a fault.

**On the version number:** this release is 2.7.0 rather than 2.6.0 because 2.6.0 is already
assigned, unshipped, to the `/donyati-interactive` skill. Reusing it would put two different
plugins into the world under one version, so the number is skipped instead.

## [2.6.0] — 2026-08-18

**Note:** the MCP server this plugin talks to is production. These tools are not live for
plugin users until this release is promoted to `main` and published with
`ORG=Donyati tools/sync-plugin-public.sh` — a separate step from this branch merging to `dev`.
**[Correction, 2026-09-09: resolved by the 2026-09-08 promotion — `create_collection_link`,
`revoke_collection_link` and `revoke_share_links` are on `main` and published; this note was
true when 2.6.0 shipped and is not current.]**

### Added
- **Interactive HTML deliverables** — `/donyati-interactive` walks through authoring a
  process swimlane, technology roadmap, or architecture diagram from real engagement
  content (`search_client_knowledge`, `get_client_briefing`) as a JSON spec, then calls
  `generate_deliverable` to render it as a self-contained, clickable HTML page. Returns an
  in-app viewer link, an email-able HTML download, and — on request — a revocable, expiring
  public share link.
- `swimlane`, `roadmap`, and `architecture-diagram` added to the `/donyati-deliverables` slug
  table.

## [2.5.0] — 2026-08-05

### Added
- **Designed briefing decks** — `generate_deliverable` now accepts the optional
  `deck_style: designed` option for client briefings, assessment summaries, and custom
  summaries. Executive summaries remain standard-only by design.

## [2.4.1] — 2026-08-02

Documentation release. **No command or tool changes** — the behaviour described below is
server-side and already applies to existing installs; updating the plugin is not required to
get it, only to read about it.

### Added
- **"What gets logged" section** (USAGE_GUIDE §2) — tool calls are recorded and attributed to
  your key owner email or Microsoft 365 sign-in, and for expert-agent commands the question
  text is stored (first 2,000 chars) with model and token counts. Answers are not stored by
  that telemetry.
- Note that Desktop / web verification shows a **User ID** rather than a Key ID, since there
  is no API key on that surface.

### Changed
- **Rate limits are now actually enforced** on the MCP endpoint. They were advertised in
  `whoami` but never applied. Documented the real behaviour: 60 req/min per caller per server
  instance, connect/tools-list exempt, explicit 429 with a retry delay instead of a silent
  failure.
- `/donyati-setup` sample output updated to match (`60 req/min per server instance`).
- Key requests should now include your Donyati email — every key is issued to a named owner so
  usage attributes to a person instead of an anonymous bucket.
- Usage guide document metadata refreshed (was stamped v2.1 / May 2026).

## [2.4.0] — 2026-07-12

### Added
- **/donyati-sow-review** — Havagi, Donyati's CIO-style SOW/RFP reviewer, now available through the connector (`review_sow` tool)
- **/donyati-agents** — full agent roster (platform + industry + Donyati specialty agents)
- **/donyati-posture** — cloud compliance posture queries (admin-only), matching the Desktop prompt
- **Industry lens on /donyati-ask** — `industry: insurance` (and 15 more verticals) frames answers in that vertical
- **4 new deliverable types** via /donyati-deliverables: assessment summary, requirements document, data source inventory, response repository

### Changed
- **/donyati-deck merged into /donyati-deliverables** — same web decks, one command
- Proposal Author placeholder agent retired from the public roster; Methodology labeled "coming soon"

### Fixed
- README MCP tool list brought up to date (24 tools)
- Documented the GitHub-marketplace release step for maintainers

## [2.3.0] — 2026-07-03

### Added
- **Connector Tier 1** — new `/donyati-start` guided entry point for new users
- **Organization & project management** — `/donyati-new-client` and `/donyati-new-project` commands
- **Document ingestion** — `/donyati-upload` command for adding RFP, transcripts, and meeting notes
- **Deliverables generation** — `/donyati-deliverables`, `/donyati-rfp-response`, and `/donyati-deck` commands for Sales and Presales teams
- **Tier 2 updates** — expanded help page and skill documentation

### Fixed
- Plugin skills and help page coverage for all 20 commands

## [2.2.0] — 2026-06-15

### Added
- **Desktop MCP prompts** — slash commands (`/donyati-*`) now available in Claude Desktop via Custom Connector
- **Reusable prompt templates** — copy-paste prompts in `PROMPT_TEMPLATES.md`
- **Detailed Word usage guide** — comprehensive `USAGE_GUIDE.md` with role-based workflows and troubleshooting

### Fixed
- Plugin manifest (author field type correction)

### Improved
- Desktop and claude.ai web integration documentation

## [2.1.0] — 2026-06-01

### Added
- **Marketplace distribution** — plugin available via Claude Code marketplace (`/plugin marketplace add`)
- **Non-technical onboarding** — streamlined setup for Claude Desktop and web users (SSO instead of API keys)
- **SSO documentation** — OAuth 2.1 flow explained for Desktop/web users

### Changed
- Enhanced installation instructions for cross-platform support

## [2.0.0] — 2026-05-20

### Added
- **Universal commands** — single set of `/donyati-*` commands works in Claude Code, Claude Desktop, and claude.ai web
- **Admin usage view** — per-user and per-key API usage tracking at `/admin/api-usage`
- **Client knowledge** — `/donyati-clients`, `/donyati-projects`, `/donyati-client-search`, `/donyati-add-knowledge`, `/donyati-briefing` for Sales, Presales, and Customer Success teams
- **Document review** — `/donyati-review` command for expert feedback
- **Knowledge base search** — `/donyati-knowledge` for fact-checking and citations

### Changed
- **Plugin architecture refactored** — unified MCP server for all surfaces
- **Command routing** — auto-detection of platform keywords

## [1.4.0] — 2025-10-15

### Added
- Industry and vertical support
- Enhanced presentation generation
- Multi-tenant document uploads

## [1.3.0] — 2025-08-10

### Added
- Changelog tracking
- Client knowledge base features

## [1.2.0] — 2025-07-05

### Fixed
- Client knowledge population from assessments API
- Content extraction and re-extraction UI

## [1.1.0] — 2025-06-20

### Added
- Embeddings and vector search
- Enhanced content pages
- Organization research features

## [1.0.0] — 2025-05-15

### Added
- Initial plugin release
- SOW document generator for EPM Cloud Suite
- Platform expert agents (Oracle, SAP, OneStream, Microsoft, Workday, Salesforce, Snowflake, Databricks, Informatica, Stibo, AWS, GCP, Tagetik, Anaplan, BlackLine, Planful, Prophix)
- Multi-agent comparison with neutral synthesis
- Platform knowledge base search
