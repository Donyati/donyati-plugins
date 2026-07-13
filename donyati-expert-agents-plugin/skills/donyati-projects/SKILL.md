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

## When to use

- Before `/donyati-client-search` or `/donyati-add-knowledge` when you want to scope to a single engagement
- To see which engagements are active vs archived for a client
