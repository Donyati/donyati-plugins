# Changelog

All notable changes to the Donyati Expert Agents plugin are documented here. Version numbering follows [Semantic Versioning](https://semver.org/).

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
