---
name: donyati-knowledge
description: Search Donyati's platform knowledge base for verified articles — useful for marketing copy, fact-checking, and pulling vetted technical claims
---

# /donyati-knowledge — Search Knowledge Base

Use the `search_knowledge` MCP tool to search Donyati's curated knowledge base across all covered platforms.

## Usage

```
/donyati-knowledge <query>
```

## Examples

```
/donyati-knowledge intercompany eliminations
/donyati-knowledge migration from Hyperion to Oracle EPM
/donyati-knowledge Snowflake Cortex
/donyati-knowledge AIH Tagetik analytics
```

## Filtering

You can scope to a single platform:

```
/donyati-knowledge platform: oracle-epm query: data integration
/donyati-knowledge platform: anaplan query: PlanIQ
```

Run `/donyati-platforms` to see available platform slugs.

## Output

Returns up to 10 matching articles with:
- Title and platform/domain
- Excerpt (first 200 characters)

Use these as **vetted citations** for marketing content, RFP responses, or client briefings — not vendor marketing claims.
