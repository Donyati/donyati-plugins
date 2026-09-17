---
name: donyati-projects
description: List the projects/engagements for a Donyati client — useful for scoping client-knowledge searches and adds to a specific engagement.
---

# /donyati-projects — List Client Projects

List the projects (engagements) for a given client.

## Usage

```
/donyati-projects <client>
```

## How it works

1. Resolve the client with `list_organizations` to get its `organizationId`.
2. Call the `list_projects` tool to list that client's projects (id, name, slug, status).

## Examples

```
/donyati-projects Joyson
/donyati-projects Meridian
```

## Where a project or POC came from

Call `get_lineage` with a `projectId` (from this command) or a `pocSlug` (from `list_pocs`) to
walk the funnel behind it: the opportunities and demos that converted into it, the project a POC
delivers through, and any conversion that was later undone. A record with nothing behind it is
reported as created directly.

## When to use

- Before `/donyati-client-search` or `/donyati-add-knowledge` when you want to scope to a single engagement
- To see which engagements are active vs archived for a client
- To answer "where did this project come from?" before a QBR or a renewal conversation

## Projects managed in DDO

Projects created in Discovery to Delivery (DDO) appear here automatically, marked **managed in DDO** with a link back. Their name, status and team are owned by DDO; edit them there, not here. Everything DDO's team approves (scope matrix, SOW, estimate, handoff) is searchable through `/donyati-client-search` and listed by `/donyati-briefing`.
