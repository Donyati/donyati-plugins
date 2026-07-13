---
name: donyati-client-search
description: Search a client or project's confirmed knowledge base — facts, decisions, stakeholders, and context captured from meetings and documents. For Sales, Presales, and Customer Success prep.
---

# /donyati-client-search — Search Client Knowledge

Search what Donyati already knows about a client or a specific project.

## Usage

```
/donyati-client-search <client> <what to search for>
```

## How it works

1. Resolve the client with the `list_organizations` tool to get its `organizationId`.
2. (Optional) Resolve a project with `list_projects` to get a `projectId`.
3. Call the `search_client_knowledge` tool with that `organizationId` (and `projectId`) and your query.
4. Summarize the matching items, citing each.

## Examples

```
/donyati-client-search Joyson close timeline
/donyati-client-search Meridian who are the key stakeholders
/donyati-client-search Joyson — project: FCCS Implementation — open risks
```

## Output

Confirmed knowledge items only (subject, detail, category, confidence). Use these as briefing material — they reflect Donyati's captured understanding of the client, not public marketing.
