# Changelog

All notable changes to the Donyati Expert Agents plugin are documented here. Version numbering follows [Semantic Versioning](https://semver.org/).

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
