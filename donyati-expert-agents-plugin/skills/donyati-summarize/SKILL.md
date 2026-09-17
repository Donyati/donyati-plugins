---
name: donyati-summarize
description: Summarize documents with Donyati platform domain expertise — proposals, SOWs, assessments, reports, meeting notes
---

# /donyati-summarize — Document Summarization

Use the `summarize` MCP tool from the `expert-agents` server to summarize documents with platform-specific domain expertise.

## Usage

```
/donyati-summarize <paste or reference document content>
```

## Options

- **content_type**: `proposal`, `sow`, `assessment`, `report`, `general`
- **platform**: Platform slug for domain context (e.g., `oracle-epm`, `anaplan`, `snowflake`)
- **output**: `executive_summary`, `key_points`, `action_items`, `full`

## Examples

```
/donyati-summarize [paste SOW content] — content_type: sow, output: executive_summary
/donyati-summarize [meeting notes] — output: action_items
/donyati-summarize [assessment report] — platform: oracle-epm, output: full
```

## Output

- **Summary** — Main summary in the requested format
- **Key Points** — 5-10 bullet points
- **Platform Insights** — Technology-specific observations and recommendations

## Summarizing for a specific client

To have the summary written with the engagement in mind:

1. Call `list_organizations` (or run `/donyati-clients`) and take the `organizationId`.
2. Optionally call `list_projects` for that org and take the `projectId`.
3. Pass `organizationId` (and `projectId`) to `summarize`.

The summarizer sees the client's **confirmed** knowledge only — a summary is routinely pasted
in front of the client. Never guess an id; the tool refuses an organization you are not
granted.

```
/donyati-summarize [paste status report] — client: Apex Manufacturing, project: FCC Phase 2
```
