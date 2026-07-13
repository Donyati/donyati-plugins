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
