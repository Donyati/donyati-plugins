---
name: donyati-briefing
description: Pull generated briefing artifacts for a client — pre-meeting briefings, summaries, and reviews. Ideal prep before a sales call or status meeting.
---

# /donyati-briefing — Client Briefing

Retrieve the briefing material Donyati has generated for a client.

## Usage

```
/donyati-briefing <client> [project]
```

## How it works

1. Resolve the client with `list_organizations` to get its `organizationId`.
2. (Optional) Resolve a project with `list_projects`.
3. Call the `get_client_briefing` tool and present the artifacts.

## Examples

```
/donyati-briefing Joyson
/donyati-briefing Meridian — project: Discovery
```

## Output

Briefing artifacts (pre-meeting briefings, summaries, reviews) with their content and whether they're marked stale. If a briefing is stale, new knowledge has been added since it was generated — regenerate it in the web app at https://expert-agents.donyati.com.
